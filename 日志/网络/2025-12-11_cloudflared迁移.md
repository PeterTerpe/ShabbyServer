# Cloudflare Tunnel 与 v2rayA 冲突排查（cloudflared Docker 化）


## 概要

```
类型：架构调整
等级：/
时间：2025-12-11
问题：cloudflared 与 v2rayA 透明代理冲突，导致 tunnel 无法稳定启动
摘要：大部分网页服务已迁移至MC服务器，使用CF tunnel分发。减轻frp服务器压力，优化链路。
```

## 主要问题
v2rayA代理导致CF tunnel无法正常启动，修改nftables未果
- 解决方案：使用docker容器运行tunnel、通过bridge网络，并将v2rayA设置为tProxy模式（不能代理docker）

## 疑难解答
### docker bridge网络问题
检查nftables中docker的链路是否完整，不完整时可以重启docker，也许有能直接恢复nftables的docker链路而无需重启的方法？
### docker容器内应用访问本机host端口
host服务必须绑定0.0.0.0或172.17.0.1，然后容器内应用通过172.17.0.1访问
### CF tunnel对mcsm daemon流量兼容不佳
直连或frp端口转发比较稳定

## 改动记录
- 在MC服务器上使用Docker运行CF tunnel，使用token文件
- 将frp服务器tunnel的routes全部迁移至MC服务器tunnel
- 新增若干routes（管理组工具/玩家文件服务）
- filebroswer绑定0.0.0.0
- 为filebrowser添加fail2ban
- 删除mcsm面板URL前缀
- mcsm现通过home.shabbyserver.com连接daemon
- 恢复nftables至默认配置
- 更改docker配置Live Restore为true（未验证重启docker对容器的影响）
- duplicati现接受所有hostname
- 为router.shabbyserver.com添加CF application policy（WAF）
- 修补CF application policy MFA验证邮箱
