# Visual Studio 奇怪的 Build error


在公司準備要建置一個平常的 .NET 專案時忽然跳了從沒看過的錯誤

<!--more-->

這個 sln 專案裡面有 256 個 .NET 專案，之前都順利編譯，但前幾天突然跳建置失敗，其中某個專案跳了以下錯誤

```
error : Your project does not reference ".NETFramework,Version=v4.0" framework. Add a reference to ".NETFramework,Version=v4.0" in the "TargetFrameworks" property of your project file and then re-run NuGet restore.
```

詢問其他同事，卻可以正常建置，所以問題應該不在專案上

再嘗試過以下幾個動作，通通無效 🙄

1. 使用 rebuild
2. 重開 Visual Studio
3. 用不同版本的 Visual Studio 編譯 (VS2017, VS2019)
4. 刪掉 VS 的 cached (.vs 資料夾)

最後將出問題的專案的 `bin` 及 `obj` 資料夾刪掉後，再重新 Build 就成功了 🎉

推測是最近有升級 Windows 10 更新到 ver 2004...導致系統某些地方秀逗了

