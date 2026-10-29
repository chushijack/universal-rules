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

### 1. 更新 Geo（geosite.dat + geoip.dat）

**不要**填在「添加规则 → 可选地址」里（那里只用于 `v2rayN.json`）。也**不要**填「路由规则集来源」——那是 sing-box 的 `.srs`，和 Geo 无关。

任选一种方式：

**方式 A（推荐，一条模板对应两个文件）**

1. **设置 → 参数设置 → v2rayN 设置**
2. **Geo 文件来源（可选）**：若下拉可选手动/自定义，填（注意中间的 `{0}`，程序更新时会分别换成 `geosite` 和 `geoip`）：

```text
https://github.com/chushijack/universal-rules/releases/latest/download/{0}.dat
```

3. 回到主界面 **检查更新 → 更新 Geo 文件**（或右键菜单里的同类项）。

等价于下面两个地址，不必分开填两次：

```text
https://github.com/chushijack/universal-rules/releases/latest/download/geosite.dat
https://github.com/chushijack/universal-rules/releases/latest/download/geoip.dat
```

**方式 B（界面里没有自定义 Geo 来源时）**

浏览器分别下载上面两个文件，覆盖到 v2rayN 的 **bin** 目录（**设置 → 打开存储所在的位置**，再进 `bin`，替换同名 `geosite.dat`、`geoip.dat`）。

必须用 **本仓库 Release** 的 dat，规则里的 `geosite:cn`、`geosite:google` 等分类名才和名单一致；不要用 Loyalsoldier 默认 Geo，否则和 `v2rayN.json` 对不上。更新后 `geosite.dat` 体积应在 **数 MB** 量级；若只有几 KB，多半是下载失败（HTML 错误页），需重新更新。

### 2. 导入路由（一个 Url）

**规则集设置 → 从订阅 Url 中导入规则**，可选地址只填下面这一条。

```text
https://github.com/chushijack/universal-rules/releases/latest/download/v2rayN.json
```

导入后与 FlClash 脚本是同一套分类和顺序（名单在 Geo 的 dat 里，路由只写 `geosite:` / `geoip:` 引用）：局域网 → 🚫 广告（`block`）→ 国内（`direct`）→ 🤖 AI、🎬 媒体、🔍 Google、🚀 代理（`proxy`，当前选中节点）→ 未命中走 `proxy`（规则备注为「🐟 漏网之鱼」，相当于脚本里的漏网之鱼，v2rayN 不能做成可选分组）。路由编辑里把 **域名解析策略** 设为 **AsIs**。

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
