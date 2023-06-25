介绍：

本示例使用 Caddy 源码加改进版 forwardproxy 插件、caddy-trojan 插件编译而成 Caddy 文件，实现了 NaiveProxy 与 Trojan-Go 服务端加伪装网站应用。

注意：

1、使用本人 Releases 中编译好的 Caddy 文件，可同时支持 NaiveProxy、Trojan-Go 等应用。

2、本示例 NaiveProxy 除了支持 HTTP/2 代理应用，还同时支持 HTTP/3 代理应用，即 QUIC 协议传输。若 NaiveProxy 使用 HTTP/3 代理应用，建议[增加 UDP 接收缓冲区大小](https://github.com/lucas-clemente/quic-go/wiki/UDP-Receive-Buffer-Size)。

3、本示例 Trojan-Go 仅兼容原版服务端的核心应用：支持 Trojan 应用与 Trojan-Go 的 WebSocket 应用共存，支持 CDN 流量中转(基于 WebSocket over TLS)。客户端推荐选择 Xray 客户端，支持使用指纹伪造；WebSocket 应用不能启用 WebSocket 0-RTT 与多路复用，不兼容。

4、本示例 Caddy 支持自动 HTTPS，即自动申请与更新 TLS 证书，自动 HTTP 重定向到 HTTPS。
