# 用 GitHub Codespaces 來操作 Hugo 網站


GitHub 在今年的 GitHub satellite [公佈了 Codespaces]({{< ref "/posts/2020/05/github-codespaces-feature.md" >}})，經過一段時間的等待終於申請到試用了

<!--more-->

拿來測試 Hugo 的 repository，結果居然有內建 Hugo XD

```shell
$ uname -a
Linux codespaces_93f23e 5.4.0-1031-azure #32~18.04.1-Ubuntu SMP Tue Oct 6 10:03:22 UTC 2020 x86_64 GNU/Linux

$ hugo version
Hugo Static Site Generator v0.71.0-06150C87/extended linux/amd64 BuildDate: 2020-05-18T16:15:29Z
```

想要測試 hugo 內建 server，預設會用 `localhost` 來建立網站，但外面連不到

```txt
$ hugo server -D
.
.
.
.
Built in 620 ms
Watching for changes in /home/codespace/workspace/HugoRepo/{archetypes,content,data,static,themes}
Watching for config changes in /home/codespace/workspace/HugoRepo/config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

可以透過指定 baseURL 到 `https://${CLOUDENV_ENVIRONMENT_ID}-1313.apps.codespaces.githubusercontent.com`，就可以進行測試了

```shell
hugo server -D --baseURL https://${CLOUDENV_ENVIRONMENT_ID}-1313.apps.codespaces.githubusercontent.com --appendPort=false
```

## Reference
- https://ddulic.dev/github-codespaces-on-ipad
- https://www.isaaclevin.com/post/github-codespaces/

