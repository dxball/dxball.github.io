# yt-dlp 使用範例


收集 [yt-dlp](https://github.com/yt-dlp/yt-dlp) 基本使用方式

<!--more-->

## 更新

[Update help](https://github.com/yt-dlp/yt-dlp#update)

```shell
yt-dlp -U
yt-dlp --update-to stable

# To update to nightly from stable executable/binary:
yt-dlp --update-to nightly
```

## 設定

[Configuration help](https://github.com/yt-dlp/yt-dlp#configuration)

1. 設定檔路徑 (Windows):  `%APPDATA%\yt-dlp\`

內容範例

```conf
# 從瀏覽器讀取 cookies
--cookies-from-browser brave:Default

# 預設下載位置
-o "E:\Downloads\YouTube\%(title)s.%(ext)s"
```

若發生以下錯誤

```text
PermissionError: [Errno 13] Permission denied: 'Path/To/Chromium/Cookies'
```

可嘗試加入另外下載 plugins: [yt-dlp-ChromeCookieUnlock](https://github.com/seproDev/yt-dlp-ChromeCookieUnlock)
放到 `%APPDATA%\ytl-dlp\plugins\` 裡面

https://github.com/yt-dlp/yt-dlp/issues/7271#issuecomment-1979912216

## 列出格式

[Format help](https://github.com/yt-dlp/yt-dlp#format-selection)

```shell
yt-dlp -F [YT_Link]
yt-dlp --list-formats [YT_Link]
```

## 下載

```shell
-f bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best
```

