# 北化体育视频文件

[公开视频目录](https://repo.yourba.top)使用 Cloudflare Pages，发布目录为 `public`，无需构建命令。原片保留原分辨率、帧率和编码音轨。

- `public/videos/events/*.mp4`：三段用户提供的活动原片，按字节复制。
- `public/videos/teaching/*.mp4`：八段用户提供的教学原片，按字节复制。
- `public/images/channel-r09-qr.png`：舞动院周特辑官方视频号原链接的二维码，用于微信内扫码观看。
- `originals/events/sports54-recap.mp4`：北京化工大学官方微博公开发布的 1080p 校运会录像原件；大于 Pages 单文件上限，保存在发布目录之外。
- `public/videos/events/sports54-recap/index.m3u8` 与同目录 TS：使用 `ffmpeg -c copy` 的 VOD HLS，无视频或音频转码，每个文件小于 25 MiB；HE-AAC 直接封装，没有音频过滤器。
- `public/manifest.json`：源文件大小/哈希、公开资源大小/哈希、画质参数和编码载荷及解码验证摘要。`bytes`/`sha256` 始终标识原片；HLS 容器使用独立的 `playlist`/`parts` 哈希。`totalSourceBytes` 与 `totalPublishedMediaBytes` 分别记录源文件及实际发布的媒体文件总量。

校运会录像来自[北京化工大学官方微博](https://weibo.com/tv/show/1034:5291899490336854?from=old_pc_videoshow)，对应[原视频号发布](https://weixin.qq.com/sph/AmdpdBAybJ)。1080p 原件完整保存；HLS 只调整封装。验证比较原片与 HLS 的 H.264 图像/SEI 载荷、SPS/PPS、原始 AAC 包载荷和全部 4,730 帧的解码哈希，并完整解码视频及音频。MP4 容器总时长 189.266009 秒包含音频尾部，HLS 视频帧时长合计 189.2 秒；全部原音频包保留。

在小程序工作区准备更新（脚本不提交、不推送、不部署）：

```text
node /path/to/BUCT-PE/tools/publish-media.cjs --repo /path/to/file-repo --teaching-manifest /path/to/teaching-manifest.json --channel-manifest /path/to/channel-manifest.json --report /path/to/validation-report.json
```

频道清单格式为 `{ "files": [{ "id": "sports54-recap", "package": "marathon", "title": "第54届校运会 · 赛事回顾", "sourcePath": "/path/to/original.mp4", "bytes": 69593107, "sha256": "c0eb0a6bd097032a6e4eea297530dc4bcf379b0ad46bf302f4a94a6d0183bfd0", "source": "北京化工大学官方微博 · 同步发布", "sourceUrl": "https://weibo.com/tv/show/1034:5291899490336854?from=old_pc_videoshow", "originalChannelsSource": "https://weixin.qq.com/sph/AmdpdBAybJ" }] }`。输出报告包含复现命令、编码载荷和帧哈希。脚本保留未拥有的额外公开文件。
