# GitLab Runner fetch 專案時保留指定目錄不被刪除


GitLab Runner 在 fetch 專案時，預設會在 fetch 後執行 `git clean -ffdx` 命令，讓專案目錄維持在乾淨的狀態

<!--more-->

然而有時候我們會想要保留一些目錄或檔案，讓下次編譯時不需要重新產生以節省編譯時間 (例如 Unity 專案的 Library 目錄)，GitLab 有提供一些方式來達成這個目標

## 解決方式

### 1. 使用 cache 機制
在 job 中設定好 [cache](https://docs.gitlab.com/ee/ci/yaml/README.html#cache)，可以讓指定目錄或檔案於 jobs 之間互相參考，缺點是如果 Runner 沒有啟用，或是超過 Runner cache 設定的上限就無法使用，另外 cache 只能在同一個 pipeline 中使用

### 2. 把專案要保留的東西複製到外部
缺點是會污染 Runner 之外的環境

### 3. 設定 `GIT_CLEAN_FLAGS` 排除指定目標被清除
[GitLab Runner 11.10](https://gitlab.com/gitlab-org/gitlab-runner/blob/v11.10.0/CHANGELOG.md) 版以後[提供](https://gitlab.com/gitlab-org/gitlab-runner/-/merge_requests/1281)可覆寫 `git clean` 執行時的參數，在 `variables` 加上 `GIT_CLEAN_FLAGS`
- 未指定時，`git clean` 預設使用 `-ffdx`
- 指定為 `none` 時，不執行 `git clean`
- 另外也可以設定自己想要的參數，例如設定要排除的目標: `-e <pattern>`

```yaml
# 排除 Library 目錄被 clean 掉
variables:
  GIT_CLEAN_FLAGS: -ffdx -e [Ll]ibrary/
```

`<pattern>` 請參考 [git clean](https://git-scm.com/docs/git-clean) 說明

## Reference
- https://docs.gitlab.com/11.11/ee/ci/yaml/README.html#git-clean-flags
- https://docs.gitlab.com/11.11/ee/ci/large_repositories/index.html#git-clean-flags
- https://git-scm.com/docs/git-clean

