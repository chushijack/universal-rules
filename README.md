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

把 Geo 文件地址改成下面两个，然后更新 Geo 文件：

```text
https://github.com/chushijack/universal-rules/releases/latest/download/geosite.dat
https://github.com/chushijack/universal-rules/releases/latest/download/geoip.dat
```

路由用这一份，里面没有节点。`proxy` 表示当前选中的服务器，`direct` 直连，`block` 拦截。

```text
https://github.com/chushijack/universal-rules/releases/latest/download/v2ray-blacklist-routing.json
```

白名单路由：

```text
https://github.com/chushijack/universal-rules/releases/latest/download/v2ray-whitelist-routing.json
```

`domainStrategy` 使用 `AsIs`，避免在 fake-ip 下用假地址判断是不是国内。

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
