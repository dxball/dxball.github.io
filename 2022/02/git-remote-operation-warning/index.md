# Git 遠端操作出現 warning: auto-detection of host provider took too long


最近更新 git for windows 到 2.34.1 之後，執行 fetch/pull/push 等遠端操作都會出現警告訊息，執行時間也變久，紀錄一下怎麼處理這個問題的

<!--more-->

## 原因
執行 git pull 時發現有警告訊息

```txt
$ git pull
warning: auto-detection of host provider took too long (>2000ms)
warning: see https://aka.ms/gcmcore-autodetect for more information.
warning: auto-detection of host provider took too long (>2000ms)
warning: see https://aka.ms/gcmcore-autodetect for more information.
warning: auto-detection of host provider took too long (>2000ms)
warning: see https://aka.ms/gcmcore-autodetect for more information.
Already up to date.
```

git 某一版(懶的找是哪一版了)更新後加入這個偵測 host provider 的功能，但因為公司是自架 git host，而且沒有連外網路，所以 git 可能就無法偵測

## 解法
手動把 git provider 設定為 generic，他就不會去檢查那個 server 了

```sh
git config --global credential.xxx.example.com.provider generic
```

或是 `.gitconfig` 加入

```ini
[credential "https://xxx.example.com"]
    provider = generic
```

## Reference
- https://github.com/GitCredentialManager/git-credential-manager/blob/main/docs/configuration.md#credentialprovider
- https://stackoverflow.com/a/69575498
- https://stackoverflow.com/a/69992938

