# OBS GStreamer 筆記


有需求要在 OBS 中加入 RTSP 的來源，用 VLC source 或是內建的 Media Source 延遲都滿高的，找了一下發現有人做了 GStreamer 的 plugin，Latency 可以降到很低

<!--more-->

## GStreamer binary

- [Windows](https://gstreamer.freedesktop.org/data/pkg/windows/1.18.4/mingw/)

## OBS Studio GStreamer source plugin

- [obs-gstreamer](https://github.com/fzwoch/obs-gstreamer)

## GStreamer RTSP source pipeline (for OBS)

`rtspsrc` 是 RTSP 來源，`latency=0` 可以讓延遲減少到最小，`protocols=tcp` 是強制使用 TCP 傳輸，經測試長時間跑 UDP 傳輸可能會發生解碼失敗的問題
`video.` 是 obs-gstreamer plugin 使用的 sink

- 使用 decodebin

  ```txt
  rtspsrc location=rtsp://192.168.1.168:554/stream latency=0 protocols=tcp ! rtpjitterbuffer latency=0 ! watchdog timeout=10000 ! decodebin ! queue ! video.
  ```
  
decodebin 預設應該是用 CPU 解碼，如果要指定解碼器, 可以手動選擇，但要先用 `gst-inspect` 查詢看看有沒有支援

- 使用自定 decoder，使用前需用 `rtph264depay` 及 `h264parse` 處理

  ```txt
  rtspsrc location=rtsp://192.168.1.168:554/stream latency=0 protocols=tcp ! rtpjitterbuffer latency=0 ! rtph264depay ! h264parse ! avdec_h264 ! video.
  ```
  
  其中 `avdec_h264` 可使用其他 decoder 替換，目前測試可以用的有  
  - avdec_h264
  - nvh264dec
  - d3d11h264dec
  - openh264dec


