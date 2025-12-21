# home.shabbyserver.com cloudflared的配置和改动记录
## 概要
- 运行环境：docker cloudflare/cloudflared
- 网络环境：docker bridge（依赖v2rayA tProxy绕过代理）
- 网络协议：--protocol http2（默认为quic）
- 参数：--no-autoupdate

## 改动记录
- 2025-12-21：添加参数--protocol http2，将协议从quic改为http2