# Windows Batch 取得桌面路徑的方法


有個需求是要用 Batch file 在桌面上建立一些檔案，遍尋不著桌面路徑的環境變數，只好透過其他方式來取得桌面路徑

<!--more-->

## 桌面路徑

正常情況下的桌面路徑應該是 `%UserProfile%\Desktop\`，但僅限於英文語系的 Windows，有些語系桌面不是`Desktop`，另外像是有些人也會修改系統預設桌面路徑的位置，所以保險的方式應該是要從系統 API 去取得正確的路徑

## 使用 Regedit 取得桌面路徑

```batch
FOR /f "usebackq tokens=1,2,*" %%B IN (`REG QUERY "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders" /v Desktop`) DO SET DESKTOPDIR=%%D
FOR /F "usebackq delims=" %%i in (`ECHO %DESKTOPDIR%`) DO SET DESKTOPDIR=%%i

ECHO %DESKTOPDIR%
```

系統桌面路徑會記錄在 `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders` 裡面，先查詢出 Desktop 的值後再把路徑展開來

## 使用 Powershell 取得桌面路徑

```batch
FOR /F "usebackq delims=" %%i in (`powershell -Command "& {[Environment]::GetFolderPath('Desktop')}"`) DO SET DESKTOPDIR=%%i

ECHO %DESKTOPDIR%
```

執行 powershell 命令 `[Environment]::GetFolderPath('Desktop')` 取得桌面路徑

## 使用 VBScript 取得桌面路徑

```batch
SET FIND_DESKTOP_SCRIPT=%TEMP%\%RANDOM%-%RANDOM%-%RANDOM%-%RANDOM%.vbs
>"%FIND_DESKTOP_SCRIPT%" (
    ECHO set WshShell = WScript.CreateObject^("WScript.Shell"^)
    ECHO strDesktop = WshShell.SpecialFolders^("Desktop"^)
    ECHO WScript.Echo^(strDesktop^)
)
FOR /F "usebackq delims=" %%i in (`cscript //nologo "%FIND_DESKTOP_SCRIPT%"`) DO SET DESKTOPDIR=%%i
DEL "%FIND_DESKTOP_SCRIPT%"

ECHO %DESKTOPDIR%
```

在 temp 目錄建立一個暫存的 VBScript 檔，用 VBScript 查詢桌面路徑後再刪除，注意輸出 vbs 檔的時候要 escape `(` `)` 字元

## Reference
- https://docs.microsoft.com/en-US/dotnet/api/system.environment.getfolderpath
- https://ss64.com/vb/special.html
- https://stackoverflow.com/a/34706178/1568102
- https://stackoverflow.com/a/2643817/1568102

