# 在 Docker 中執行 msbuild 編譯 C++ 專案發生 LNK1318 錯誤


Windows Container 中安裝 Visual Studio 2017 BuildTools，編譯 C++ 專案發生錯誤

```text
LINK : fatal error LNK1318: Unexpected PDB error; RPC (23) '(0x000006E7)
```

<!--more-->

根據[查到的資料](https://github.com/docker/for-win/issues/829#issuecomment-397149736)，應該是 Docker 的 bug

裡面提到的 workround 有 3 個
1. `docker run` 加入參數 `--isolation=process`
2. `Debug Information Format` 改成 None or C7 compatible (/Z7)，msbuild 編譯參數加入 `/property:_IsNativeEnvironment=true`
3. 不要把 source mount 出來，只存在 container 中編譯

因為是用 cmake 編譯，裡面會自動呼叫 msbuild 命令，一時間也不知道怎麼用解法1,2
所以暫時先把 docker volume mount 取消掉，再測試後就可以正常編譯了

