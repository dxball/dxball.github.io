# 專案參考 Nerdbank.GitVersioning 後，使用 dotnet build 編譯失敗


為了簡化專案版號的控制，測試引入 [Nerdbank.GitVersioning](https://github.com/dotnet/Nerdbank.GitVersioning) 來協助自動加入 Assembly version 和 git commit id  
本機測試正常，但 CI 環境上使用 `dotnet build` 編譯的時候跳錯誤

<!--more-->

錯誤訊息如下

```text
C:\Users\XXX\.nuget\packages\nerdbank.gitversioning\3.6.133\build\Nerdbank.GitVersioning.Inner.targets(17,5):
error MSB4062: 無法從組件 C:\Users\XXX\.nuget\packages\nerdbank.gitversioning\3.6.133\build\MSBuildCore/Nerdbank.GitVersioning.Tasks.dll 載入 "Nerdbank.GitVersioning.Tasks.GetBuildVersion" 工作。
Could not load file or assembly 'System.Runtime.Loader, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a'. 系統找不到指定的檔案。 請確認 <UsingTask> 宣告正確、該組件和其所有相依性都可使用，以及該工作包含一個實作 Microsoft.Build.Framework.ITask 的公用類別。
[C:\Users\XXX\.nuget\packages\nerdbank.gitversioning\3.6.133\build\PrivateP2PCaching.proj]
```

查到後面發現 CI 環境上只有安裝到 .NET 5.0 SDK，而 Nerdbank.GitVersioning 需要 6.0 以上

在 CI 環境上安裝 [.NET 6.0 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/6.0) 後就可以正常對專案執行 `dotnet build` 了  
最後在專案根目錄加入 `global.json` 指定 .NET SDK 版本

```json
{
  "sdk": {
    "version": "6.0.417"
  }
}
```

## 參考
- https://github.com/dotnet/Nerdbank.GitVersioning/issues/934
- https://learn.microsoft.com/en-us/dotnet/core/tools/global-json

