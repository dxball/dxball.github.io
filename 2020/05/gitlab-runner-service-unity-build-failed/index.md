# Gitlab Runner as Windows service freezed when building Unity project


用 Gitlab CI 建置自動化編譯 Unity 專案流程，在 [Runner 註冊為服務][service]時，若專案過大可能會發生任務卡住的問題

<!--more-->

```
CD C:\GitLab-Runner
.\gitlab-runner.exe install
.\gitlab-runner.exe start
```

若用直接執行的方式執行 Job 就不會卡住
```
CD C:\GitLab-Runner
.\gitlab-runner.exe run
```

卡住之後查看 Unity Editor Log，發現有異常訊息 `Failed to get socket connection from UnityShaderCompiler.exe shader compiler!`

```
Failed to get socket connection from UnityShaderCompiler.exe shader compiler! C:/Program Files/Unity/Editor/Data/Tools/UnityShaderCompiler.exe
UnityEditor.BuildPipeline:BuildPlayerInternalNoCheck(String[], String, String, BuildTargetGroup, BuildTarget, BuildOptions, Boolean)
UnityEditor.BuildPipeline:BuildPlayerInternal(String[], String, String, BuildTargetGroup, BuildTarget, BuildOptions) (at C:\buildslave\unity\build\Editor\Mono\BuildPipeline.bindings.cs:358)
UnityEditor.BuildPipeline:BuildPlayer(String[], String, String, BuildTargetGroup, BuildTarget, BuildOptions) (at C:\buildslave\unity\build\Editor\Mono\BuildPipeline.bindings.cs:257)
UnityEditor.BuildPipeline:BuildPlayer(BuildPlayerOptions) (at C:\buildslave\unity\build\Editor\Mono\BuildPipeline.bindings.cs:240)

[C:\buildslave\unity\build\Tools/UnityShaderCompiler/ShaderCompilerClient.cpp line 290] 
(Filename: Assets/xxxx.cs Line: 218)

```

## 發生原因
根據[這邊][stackoverflow]提到的，Service 的 Desktop heap 比較小，所以有可能在編譯大專案的時候資源不足讓程式卡住

## 解法
調大 non-interactive (services) sessions 的 heap

1. 用 Administrator 執行 regedit.exe
2. 編輯 `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager\SubSystems\Windows`
3. 找到 `SharedSection=1024,20480,768`，調整第三組數字

```diff
- SharedSection=1024,20480,768
+ SharedSection=1024,20480,2048
```

{{< admonition warning "Warning" >}}
數值不要調太高避免影響原有桌面使用
{{< /admonition >}}


## Reference
- https://answers.unity.com/questions/1430683/failed-to-get-socket-connection-from-unityshaderco-2.html
- https://stackoverflow.com/questions/17472389/how-to-increase-the-maximum-number-of-child-processes-that-can-be-spawned-by-a-w/17472390#17472390

[service]: https://docs.gitlab.com/runner/install/windows.html#installation "Install GitLab Runner on Windows"
[stackoverflow]: https://stackoverflow.com/questions/17472389/how-to-increase-the-maximum-number-of-child-processes-that-can-be-spawned-by-a-w/17472390#17472390 "Services get smaller desktop heaps than interactive sessions"

