# Clash

## 配置解读

### DNS 配置部分

```yaml
dns:
  enable: true
  ipv6: false
  default-nameserver: [223.5.5.5, 119.29.29.29]
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  use-hosts: true
  nameserver: ['https://doh.pub/dns-query', 'https://dns.alidns.com/dns-query']
  fallback: ['https://doh.dns.sb/dns-query', 'https://dns.cloudflare.com/dns-query', 'https://dns.twnic.tw/dns-query', 'tls://8.8.4.4:853']
  fallback-filter: { geoip: true, ipcidr: [240.0.0.0/4, 0.0.0.0/32] }
```

| 配置项                        | 作用                                                 |
| -------------------------- | -------------------------------------------------- |
| **enable: true**           | 开启 Clash 内置 DNS 服务。                                |
| **ipv6: false**            | 禁用 IPv6 DNS 解析。                                    |
| **default-nameserver**     | 用于解析 `nameserver` / `fallback` 里域名的“底层”DNS。        |
| **enhanced-mode: fake-ip** | DNS 增强模式：返回一个假 IP（198.18.0.0/16），并在内部维护真实 IP 对应关系。 |
| **fake-ip-range**          | fake-ip 分配的地址段。                                    |
| **use-hosts: true**        | 是否使用系统 hosts 文件。                                   |
| **nameserver**             | 主 DNS 服务器列表（高优先级）。                                 |
| **fallback**               | 备用 DNS 列表（在主 DNS 无法解析时使用）。                         |
| **fallback-filter**        | 控制哪些情况才使用 fallback。                                |

## 问题解决

```bash
!! [TCP] dial failed
static.leetcode.cn:443
ERROR → all DNS requests failed, first error: Post "https://doh.dns.sb/dns-query": dial tcp4 127.0.0.1:443: connectex: No connection could be made because the target machine actively refused it. PROXY → DIRECT FROM → 127.0.0.1:54xxx RULE  → RuleSet PAYLOAD → direct
```

`!![TCP] dial failed`

Clash 试图发起一个 TCP 连接，但建连失败（dial 是发起连接）。

`ERROR -> all DNS requests failed`

尝试的所有 DNS 查询都失败了。

`first error: Post "https://doh.dns.sb/dns-query"`

第一个失败 DNS 尝试：Clash 采用 DoH（DNS over HTTPS）向 `https://doh.dns.sb/dns-query` 发送 HTTP POST 请求以解析域名。

`dial tcp4 127.0.0.1:443`

在尝试连接 `doh.dns.sb` 时，这个域名被解析成了 `127.0.0.1` 本机回环地址，而不是真正的公网 IP，于是 Clash 去连接本地主机的 443 端口。

`connectx: No connection could be made because the target machine actively refused it`

Windows 的套接字错误：目标端口主动拒绝（没有任何进程在 127.0.0.1:443 上监听，所以系统立即拒绝连接）。
