# Visual Studio Build Tools 下載點整理


Visual Studio Build Tools 下載點整理

<!--more-->

- [Visual Studio 2017 Community](https://aka.ms/vs/15/release/vs_community.exe)
- [Build Tools for Visual Studio 2017](https://aka.ms/vs/15/release/vs_buildtools.exe)
- [Visual Studio 2019 Community](https://aka.ms/vs/16/release/vs_community.exe)
- [Build Tools for Visual Studio 2019](https://aka.ms/vs/16/release/vs_buildtools.exe)
- [Visual Studio 2022 Community](https://aka.ms/vs/17/release/vs_community.exe)
- [Build Tools for Visual Studio 2022](https://aka.ms/vs/17/release/vs_buildtools.exe)

離線下載

```batch
vs_BuildTools.exe --layout C:\offline_layouts --lang en-US ^ 
 --add Microsoft.VisualStudio.Workload.MSBuildTools ^ 
 --add Microsoft.VisualStudio.Workload.ManagedDesktopBuildTools ^ 
 --add Microsoft.VisualStudio.Workload.VCTools ^ 
 --add Microsoft.VisualStudio.Component.VC.ATLMFC ^ 
 --add Microsoft.VisualStudio.Component.Windows81SDK ^ 
 --includeRecommended --includeOptional
```

離線安裝

```batch
C:\offline_layouts\vs_buildtools.exe --noweb
```

