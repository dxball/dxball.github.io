# GitLab Runner 在 Windows cmd 環境沒辦法執行兩句以上的 npm 指令


使用 GitLab 跑 nodejs 專案遇到 script 執行不完全的問題

<!--more-->

GitLab Runner 版本: 13.9.0
執行環境是 Win10 x64，Executor 為 Shell (CMD)

.gitlab-ci.yml 內容如下

```yml
build_job:
  stage: build
  script:
    - npm install
    - npm run build
  cache:
    paths:
      - node_modules/
  artifacts:
    paths:
      - build/*
```

執行 Log

```txt
Executing "step_script" stage of the job script
$ npm install
up to date, audited 234 packages in 978ms
17 packages are looking for funding
  run `npm fund` for details
found 0 vulnerabilities
Saving cache for successful job
00:03
Creating cache default...
Runtime platform                                    arch=amd64 os=windows pid=12496 revision=4aae0f02 version=13.9.0~beta.104.g4aae0f02
node_modules/: found 4295 matching files and directories 
No URL provided, cache will be not uploaded to shared cache server. Cache will be stored only locally. 
Created cache
Uploading artifacts for successful job
00:00
Uploading artifacts...
Runtime platform                                    arch=amd64 os=windows pid=9576 revision=4aae0f02 version=13.9.0~beta.104.g4aae0f02
WARNING: build/*: no matching files                
ERROR: No files to upload                          
Cleaning up file based variables
00:01
Job succeeded
```

可以看到只執行了 `npm install` 就結束工作了，搜尋了一下發現也有人遇到[一樣的問題](https://gitlab.com/gitlab-org/gitlab-runner/-/issues/2730)

目前的 workaround 是用 `call` 去呼叫 `npm`

```yml
build_job:
  script:
    - call npm install
    - call npm run build
```

## 後記

Windows Batch 已經被 Gitlab Runner [標記為 deprecated](https://docs.gitlab.com/runner/shells/#windows-batch) 了，不過因為環境的關係我們還是暫時用 cmd 來執行 runner，之後有機會的話再把環境改成 powershell

> In GitLab 11.11, we announced the deprecation of the Windows Batch executor, cmd shell, for the GitLab Runner in favor of PowerShell. The cmd shell remains included in future versions of GitLab Runner however, any new feature for Windows is to be tested and supported only for use with PowerShell. Only critical bugs and regressions to the cmd shell will be investigated and fixed.

## Reference
- https://gitlab.com/gitlab-org/gitlab-runner/-/issues/2730#note_39661770

