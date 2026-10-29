# universal-rules

分流规则配置。名单由上游项目的公开数据转换而来，每天更新。

规则文件不在本仓库目录里。仓库只保留这份说明，可导入的文件在 [最新 Release](https://github.com/chushijack/universal-rules/releases/latest) 的附件中。下面的地址会跟着最新一次发布走。

这里没有代理节点，也不能代替你的订阅。规则只决定流量去向：直连、拦截，或交给你已经选中的节点。

## 使用前请知悉

本仓库的配置和名单仅供学习、研究网络规则格式。

- 不提供代理服务，不出售、不代配节点。
- 与任何上游项目、客户端作者没有隶属、合作或担保关系。名单版权和许可归原作者，见文末出处。
- 名单可能过时、遗漏或误伤。是否下载、导入和使用，由你自己判断并自行承担后果。
- 请同时遵守你所在地的法律，以及各上游项目自己的许可。

## 黑名单和白名单

两套规则的分类顺序相同。黑名单里，没有命中的流量走代理。白名单里，没有命中的流量直连。

分类顺序是：局域网 → 广告 → 国内 → AI → 流媒体 → Google → 其他需要代理的网站。没有命中的流量进「🐟 漏网之鱼」。

## FlClash

1. 「配置」里继续用你自己的机场订阅。
2. 打开扩展，模式选「脚本」。
3. 打开下面的地址，全选复制到新建脚本里，保存后选中，再应用。脚本框不能直接填链接。

```text
https://github.com/chushijack/universal-rules/releases/latest/download/flclash.js
```

应用后，你的节点分组后面会出现「🚫 广告」「🤖 AI」「🎬 媒体」「🔍 Google」「🚀 代理」「🐟 漏网之鱼」。广告分组默认是拦截。漏网之鱼接收没有命中的流量，默认仍是订阅里原来的最终去向，可以再改。国外域名的 DNS 交给「🚀 代理」解析，国内和局域网用国内 DoH 解析真实地址。

系统代理拦不住 DNS，浏览器仍会问运营商。请打开「虚拟网卡」。工具里关掉「追加系统 DNS」；如果开了「覆写 DNS」，客户端会换成自己的 DNS，脚本里的设置不会生效。

## Clash Verge Rev

1. 「订阅」里继续用你自己的机场订阅。
2. 右键这个订阅，打开「编辑扩展/脚本」，进入「全局扩展脚本」。
3. 打开下面的地址，全选复制进去并保存，再重新激活订阅。脚本框不能直接填链接。

```text
https://github.com/chushijack/universal-rules/releases/latest/download/clash-verge.js
```

保存后，订阅原有分组后面会出现「🚫 广告」「🤖 AI」「🎬 媒体」「🔍 Google」「🚀 代理」「🐟 漏网之鱼」。广告分组默认是拦截。漏网之鱼接收没有命中的流量。系统代理拦不住 DNS，请在 Clash Verge 里打开虚拟网卡。

## 小火箭 Shadowrocket

从 URL 下载下面的配置。`PROXY` 就是小火箭里当前选中的节点。

黑名单：

```text
https://github.com/chushijack/universal-rules/releases/latest/download/shadowrocket-blacklist.conf
```

白名单：

```text
https://github.com/chushijack/universal-rules/releases/latest/download/shadowrocket-whitelist.conf
```

## v2rayN

两种用法二选一：**方案 A** 使用本仓库 Geo 与 `v2rayN.json`；**方案 B** 使用 v2rayN 自带 dat 与 `v2rayN-builtin-geo.json`。  
两种方案都建议配置 **DNS 与 Tun**（见「DNS 与防泄露」），路由与 DNS 一起生效，降低 DNS 泄露风险。

### 方案 A：本仓库 Geo + `v2rayN.json`

#### 1. 更新 Geo（geosite.dat + geoip.dat）

**不要**填在「添加规则 → 可选地址」里（那里只用于 `v2rayN.json`）。也**不要**填「路由规则集来源」——那是 sing-box 的 `.srs`，和 Geo 无关。

**只改「Geo 文件来源」不会立刻生效。** 这里只是下载地址模板；必须再 **触发一次 Geo 更新**，或 **手动覆盖 `bin` 里的文件**（见下）。改完参数要点 **确定**。

Geo 模板（`{0}` 会被换成 `geosite` / `geoip`）：

```text
https://github.com/chushijack/universal-rules/releases/latest/download/{0}.dat
```

**方式 A：走 v2rayN 自动下载**

1. **设置 → 参数设置 → v2rayN 设置 → Geo 文件来源（可选）** 填上面模板 → **确定**。
2. **帮助 → 检查更新**，在更新项里 **勾选 GeoFiles**（或 Geo 相关项），点 **检查更新**。  
   官方说明：这一步 **会走当前代理** 去拉 GitHub；节点不可用或 GitHub 异常时，**界面无报错也可能没下成功**，`bin` 里仍是旧 Loyalsoldier 的 dat。
3. 打开 **`D:\software\v2rayN-windows-64\bin`**（便携版）或 **设置 → 打开存储所在的位置 → `bin`**，看 `geosite.dat` 修改时间与大小（约 **7～8 MB**）。仍是几 MB 以下旧体积 → 当方式 A 失败，改用方式 B。
4. 建议勾选 **更新 Core 时忽略 Geo 文件**，避免以后升级 Xray 核又把 `bin` 里的 dat 盖回默认。

**方式 B：手动覆盖（社区最常用，推荐）**

浏览器分别下载（或从本仓库 `output/geo/` 复制）：

```text
https://github.com/chushijack/universal-rules/releases/latest/download/geosite.dat
https://github.com/chushijack/universal-rules/releases/latest/download/geoip.dat
```

**完全退出 v2rayN** 后，覆盖 **`bin\geosite.dat`** 与 **`bin\geoip.dat`**（便携安装目录一般是 `v2rayN-windows-64\bin`）。再启动。

必须用 **本仓库 Release** 的 dat，规则里的 `geosite:reject`、`geosite:cn` 等才和 `v2rayN.json` 一致。Loyalsoldier 默认 dat **没有** 这些分类名，会继续报 `failed to check code REJECT … EOF`。

#### 2. 导入路由

**规则集设置 → 从订阅 Url 中导入规则**，可选地址：

```text
https://github.com/chushijack/universal-rules/releases/latest/download/v2rayN.json
```

导入后分类与顺序同本文「黑名单和白名单」：局域网 → 🚫 广告（`block`）→ 国内（`direct`）→ 🤖 AI、🎬 媒体、🔍 Google、🚀 代理（`proxy`，当前选中节点）→ 未命中走 `proxy`（「🐟 漏网之鱼」）。名单在 Geo 的 dat 里，路由只写 `geosite:` / `geoip:` 引用。路由编辑里把 **域名解析策略** 设为 **AsIs**。

### 方案 B：内置 Geo，只导入分流 JSON（省事）

**不用改 Geo 文件来源**，也**不用**覆盖 `bin` 里的 `geosite.dat` / `geoip.dat`。保持 v2rayN 默认（或 **帮助 → 检查更新 → GeoFiles** 更新的 Loyalsoldier 即可）。

**规则集设置 → 从订阅 Url 中导入规则**，只填：

```text
https://github.com/chushijack/universal-rules/releases/latest/download/v2rayN-builtin-geo.json
```

路由顺序同上；名单来自 Loyalsoldier 等社区 Geo 分类，**没有**本仓库 `geosite:reject`、`geosite:proxy` 等大名单，与方案 A 在细节上会有差别，例如：

| 分组 | 内置 Geo 引用 |
| --- | --- |
| 局域网 | `geosite:private` + `geoip:private` |
| 广告 | `geosite:category-ads-all` |
| 国内 | `geosite:cn` + `geoip:cn` |
| AI | `geosite:category-ai-!cn` |
| 媒体 | `youtube` / `netflix` / `spotify` 等 |
| Google | `geosite:google` |
| 漏网之鱼 | 未命中 → `proxy`（无 `geosite:proxy` 大名单） |

域名解析策略同样建议 **AsIs**。需要与本仓库 Geo 完全同一套名单时，用 **方案 A**。

### DNS 与防泄露（建议）

路由 JSON（`v2rayN.json` / `v2rayN-builtin-geo.json`）**已自带** 前置规则：「🌐 绕过中国公共 DNS IP / 域名」（连 Ali、DoH.pub **走直连**）、「🚫 阻断 UDP 443」（减少 QUIC 绕过域名分流）。顺序在局域网/国内/漏网之鱼 **之前**，避免 DoH 被漏网送进代理。再按下面配 DNS。

**1. 简易 DNS**

Release 提供 **`simple_dns.json`**（v2rayN `SimpleDNSItem` 格式）。在 **设置 → Core 基础设置 → DNS → DNS 基础设置** 中按文件填写，或下载后对照：

```text
https://github.com/chushijack/universal-rules/releases/latest/download/simple_dns.json
```

| 项 | 建议值 |
| --- | --- |
| 直连 DNS | `https://dns.alidns.com/dns-query,https://doh.pub/dns-query` |
| 远程 DNS | `https://1.1.1.1/dns-query,https://8.8.8.8/dns-query` |
| Bootstrap DNS | `223.5.5.5,119.29.29.29` |
| 直连目标解析策略 | **UseIPv4**（用直连 DNS，不用系统 DNS） |
| 代理目标解析策略 | **UseIPv4**（用远程 DNS，经代理访问） |
| 并行查询 | **关** |
| 使用系统 hosts | **关** |
| 添加常用 DNS Hosts | **开** |

**国内/局域网域名** 走国内 DoH 解析；**其余** 走 1.1.1.1 / 8.8.8.8，且经 **当前节点** 访问解析服务，避免国外测速域名先落到运营商 DNS。

**2. Tun**

仅 **系统代理** 拦不住 DNS。请 **设置 → Tun 模式设置** 打开 **启用 Tun**，并开启 **严格路由（Strict Route）**，减少 Windows 把 53 端口直接发给运营商的情况。改完后 **重启 Core**。

**3. 自测**

用 [browserleaks.com/dns](https://browserleaks.com/dns) 看是否出现 **本地 ISP 解析器**；分流下 [ip.skk.moe](https://ip.skk.moe) 出现多个解析器不一定等于泄露，以 browserleaks 为准。

## 不要单独导入的文件

`clash-blacklist.yaml` 和 `clash-whitelist.yaml` 是带占位节点的完整配置。导入后会换掉你订阅里的节点。已有订阅时用上面的脚本，不要用这两份。

`*.yaml` 和 `*.list` 是规则名单，由上面的配置自动下载，不用逐个导入。

## sing-box

从最新 Release 下载 `singbox.zip`，解压后配置里的 `path` 指向同目录的 JSON。

## 上游数据出处

规则不是本仓库编写的网站名单，而是拉取上游公开数据后，按分类转换成各客户端格式。感谢这些项目长期维护并公开名单和图标：

- [Loyalsoldier/clash-rules](https://github.com/Loyalsoldier/clash-rules)
- [felixonmars/dnsmasq-china-list](https://github.com/felixonmars/dnsmasq-china-list)
- [ACL4SSR/ACL4SSR](https://github.com/ACL4SSR/ACL4SSR)
- [Koolson/Qure](https://github.com/Koolson/Qure)

| 用在哪 | 上游 | 许可 | 取用的文件 |
| --- | --- | --- | --- |
| 国内域名和 IP、广告、其他代理域名 | [Loyalsoldier/clash-rules](https://github.com/Loyalsoldier/clash-rules) | GPL-3.0 | `direct.txt`、`cncidr.txt`、`reject.txt`、`proxy.txt` |
| 国内域名补充 | [felixonmars/dnsmasq-china-list](https://github.com/felixonmars/dnsmasq-china-list) | WTFPL | `accelerated-domains.china.conf` |
| AI、流媒体、Google | [ACL4SSR/ACL4SSR](https://github.com/ACL4SSR/ACL4SSR) | CC-BY-SA-4.0 | `AI.list`、`OpenAi.list`、`Claude.list`、`Gemini.list`、`Netflix.list`、`YouTube.list`、`DisneyPlus.list`、`Spotify.list`、`HBO.list`、`AbemaTV.list`、`Google.list` |
| 局域网和保留地址 | IANA / RFC 1918、3927、4193、4291、6598、6761、6762、8375 | 公开的地址分配事实 | 本仓库内置的私有地址表 |
| 分组图标 | [Koolson/Qure](https://github.com/Koolson/Qure) | 以该仓库说明为准 | `IconSet/Color/` 中的 ChatGPT、YouTube、Proxy、Advertising |

## Star 趋势

[![Star History](https://api.star-history.com/svg?repos=chushijack/universal-rules&type=Date)](https://www.star-history.com/#chushijack/universal-rules&Date)
