介绍：

Xray 前置（监听 443 端口），利用 VLESS+Vision+TLS 回落及分流 WebSocket 特性与 Nginx 为 gRPC 提供反向代理，实现除 Xray 的 mKCP 应用外共用 443 端口，其应用如下：

1、E=VLESS+Vision+TLS（回落/分流配置，TLS 由自己启用及处理。）

2、B=VMess+WebSocket+TLS（TLS 由 VLESS+Vision+TLS 启用及处理，不需配置。）

3、G=Shadowsocks+gRPC+TLS（TLS 由 VLESS+Vision+TLS 启用及处理，不需配置。）

4、A=VLESS+mKCP+seed

注意：

1、Xray 的监听地址不支持 Shadowsocks 协议使用 UDS 监听。

2、Xray 版本不小于 v1.7.2 才完美支持 VLESS 协议的 XTLS Vision 应用。

3、Nginx 支持 H2C server 及 gRPC proxy 需要 Nginx 包含 http_v2_module 模块。

4、Nginx 支持 H2C server，但不支持 HTTP/1.1 server 与 H2C server 共用一个端口或一个进程；故回落分成 http/1.1 回落与 h2 回落分别对应 Nginx 的 HTTP/1.1 server 与 H2C server。

5、Nginx 支持请求标头还原为真实客户端地址需要 Nginx 包含 http_realip_module 模块。

6、不要使用 ACME 客户端在采用本示例的服务器上以 HTTP-01 或 TLS-ALPN-01 验证方式申请与更新 TLS 证书，因 HTTP-01 或 TLS-ALPN-01 验证方式申请与更新 TLS 证书需监听 80 或 443 端口，从而与当前应用端口冲突。

7、配置1：使用 Local Loopback 连接，且启用了 PROXY protocol。配置2：使用 UDS 连接（对应 Shadowsocks+gRPC+TLS 除外），且启用了 PROXY protocol。
