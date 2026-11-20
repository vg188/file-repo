# 北化体育视频文件

活动录像与教学原片的静态文件仓库。文件按字节复制，不重新编码；大小、SHA-256、尺寸、帧率及编码信息记录在 `public/manifest.json`。

Cloudflare Pages 发布目录为 `public`，无需构建命令。公开地址为 https://repo.yourba.top 。

- `public/videos/events/`：三段活动原片。
- `public/videos/teaching/`：八段提供的教学原片。
- `public/manifest.json`：媒体清单与完整性校验。

更新时同步媒体清单与小程序中的播放地址。
