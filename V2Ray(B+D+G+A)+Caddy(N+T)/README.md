介绍：

利用 Caddy 支持 WebSocket、H2C、gRPC 代理与 forwardproxy、caddy-trojan 插件应用，实现除 V2Ray 或 Xray 的 mKCP 应用外各应用共用 443 端口，其应用如下：

1、B=VMess+WebSocket+TLS（TLS 由 Caddy 提供及处理，不需配置。）

2、D=VLESS+H2C+TLS（TLS 由 Caddy 提供及处理，不需配置。）

3、G=Shadowsocks+gRPC+TLS（TLS 由 Caddy 提供及处理，不需配置。）

4、A=VLESS+mKCP+seed

5、N=NaiveProxy（基于 Caddy 的改进版 forwardproxy 插件实现，TLS 由 Caddy 提供及处理。）

6、T=Trojan-Go（基于 Caddy 的 caddy-trojan 插件实现，TLS 由 Caddy 提供及处理。）

注意：

1、V2Ray 或 Xray 的监听地址不支持 Shadowsocks 协议使用 UDS 监听。

2、V2Ray 版本不小于 v4.36.2 或 Xray 版本不小于 v1.4.0 才支持 gRPC 传输方式。

3、Caddy 版本不小于 v2.6.0 才支持 H2C/gRPC proxy 的 UDS 转发。

4、使用本人 Releases 中编译好的 Caddy 文件，可同时支持 H2C/gRPC proxy、NaiveProxy、Trojan-Go 等应用。

5、本示例 NaiveProxy 除了支持 HTTP/2 代理应用，还同时支持 HTTP/3 代理应用，即 QUIC 协议传输。若 NaiveProxy 使用 HTTP/3 代理应用，建议[增加 UDP 接收缓冲区大小](https://github.com/lucas-clemente/quic-go/wiki/UDP-Receive-Buffer-Size)。

6、本示例 Trojan-Go 仅推荐使用 Trojan 应用（不使用 WebSocket 应用）：因为已有同类 WebSocket 应用，且其 WebSocket 应用不支持 WebSocket 0-RTT 与多路复用等；另外还可使用 [Trojan+WebSocket+TLS](https://github.com/lxhao61/integrated-examples/tree/main/V2Ray(Trojan%2BWebSocket)%2BNginx%5CCaddy) 应用替代，性能及兼容性更好。客户端推荐选择 Xray 客户端，支持使用指纹伪造。

7、本示例 Caddy 支持自动 HTTPS，即自动申请与更新 TLS 证书，自动 HTTP 重定向到 HTTPS。

8、配置1：使用 Local Loopback 连接。配置2：使用 UDS 连接（对应 Shadowsocks+gRPC+TLS 除外）。
