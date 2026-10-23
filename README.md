# Shadowrocket 分流与去广告配置

个人整理版本：**Jack-Optimized，2026-10-23**。用于广告域名拦截、境内外连接分流及加密域名解析。由 [jackwoo725-ux](https://github.com/jackwoo725-ux) 整理、验证并发布，原规则作者与项目页面见下方「来源与致谢」。

这是配置文件，不是代理节点订阅；使用者需要自行保留或配置可用节点。配置不含节点账号、订阅令牌、证书、私钥或脚本。

## 配置地址与二维码

**固定地址**（后续整理版本可在同一地址更新）：

```text
https://raw.githubusercontent.com/jackwoo725-ux/shadowrocket-rules/main/configs/Jack-Optimized.conf
```

[打开配置原文](https://raw.githubusercontent.com/jackwoo725-ux/shadowrocket-rules/main/configs/Jack-Optimized.conf) · [2026-10-23 版本](configs/Jack-Optimized-20261023.conf) · [校验摘要](SHA256SUMS)

![配置地址二维码](qr/Jack-Optimized-url.png)

[下载二维码 PNG](qr/Jack-Optimized-url.png) · [下载矢量版 SVG](qr/Jack-Optimized-url.svg)

二维码编码的是上方完整配置地址，没有中转或短链。它不含代理节点，也不含个人凭据。若扫码后仅打开浏览器，请复制地址并按下列步骤导入。

## 使用

1. 保留自己原有的配置，以便回退。
2. 在 Shadowrocket 的「配置」页添加上方配置地址并下载。
3. 点击下载后的配置，选择「使用配置」；全局路由选择「配置」。
4. 确认 4 个远程规则集加载成功，再检查常用应用是否正常连接。

仅下载配置不会保证自动启用。发布本文件也不会自动替换手机上已导入的本地副本。此仓库未配置定时构建；配置引用的外部规则集由各上游维护，客户端刷新后的实际内容可能变化。

## 当前策略

- 本地网络直连；广告域名先匹配拦截。
- 常用境外服务与登录、资源域名走当前选中的代理。
- 境内服务直连，并优先处理可在境内使用的苹果服务。
- 未匹配域名最终走代理；中国地址的兜底规则不主动发起域名解析。
- DNS（Domain Name System，域名系统）：默认 Google 加密解析经代理，直连与代理服务器域名使用阿里加密解析，备用 Cloudflare 加密解析经代理。
- HTTPS（Hypertext Transfer Protocol Secure，安全超文本传输协议）解密关闭，无脚本、无内容重写。IPv6（Internet Protocol version 6，互联网协议第六版）关闭。

## 验证范围

2026-10-23 在 Shadowrocket 2.2.92 (3445) 上完成以下验证：

| 项目 | 结果 |
| --- | --- |
| 远程规则集 | 4/4 加载成功 |
| 百度规则测试 | DIRECT，直连 |
| ChatGPT 规则测试 | PROXY，代理 |
| doubleclick.net 规则测试 | REJECT，拒绝 |
| apps.apple.com 规则测试 | DIRECT，直连 |
| 实际网页访问 | 百度、腾讯、Google、维基百科均加载成功 |
| 实际连接记录 | 腾讯及相关资源直连，维基百科走代理，所观察到的 iCloud 服务直连 |
| 直连域名解析记录 | 所观察样本使用阿里加密解析 |

置信度：**高，限于上述设置和样本**。广告拦截以规则测试验证，未将短时流量样本称为全应用去广告证明。视频流广告、同域嵌入广告不保证可移除；应用登录、支付等流程未全面测试，也未进行抓包级域名解析泄漏审计。规则误拦或漏分流应按具体应用、域名调整。

## 来源与致谢

### 本配置实际引用的规则

| 项目与作者/维护者 | 项目页面 | 本配置用途 | 上游许可 |
| --- | --- | --- | --- |
| **anti-AD**：privacy-protection-tools 项目及贡献者；许可证版权署名 gently | [privacy-protection-tools/anti-AD](https://github.com/privacy-protection-tools/anti-AD) | 广告域名拦截，`anti-ad-surge2.txt` | [MIT](https://github.com/privacy-protection-tools/anti-AD/blob/master/LICENSE) |
| **Surge Rules**：Loyalsoldier 及贡献者 | [Loyalsoldier/surge-rules](https://github.com/Loyalsoldier/surge-rules) | `apple.txt`、`proxy.txt`、`direct.txt` 三个域名集 | [GPL-3.0](https://github.com/Loyalsoldier/surge-rules/blob/master/LICENSE) |

### 原配置与文档参考

原配置研究对象为 [Johnshall/Shadowrocket-ADBlock-Rules-Forever](https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever)（[原发布页面](https://johnshall.github.io/Shadowrocket-ADBlock-Rules-Forever/)），原文件为 [`sr_top500_whitelist_ad.conf`](https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever/blob/release/sr_top500_whitelist_ad.conf)。该项目注明原作者为 **h2y**，原项目为 [h2y/Shadowrocket-ADBlock-Rules](https://github.com/h2y/Shadowrocket-ADBlock-Rules)，由 **Johnshall** 延续维护；上游采用 [CC BY-SA 4.0](https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever/blob/release/LICENSE)。

本仓库发布的是重新整理的 Jack-Optimized 配置，不是上述 Top500 原文件的原样镜像；未继续使用其 Top500 直连列表。本次调整采用独立的远程广告域名集与分流域名集，并关闭解密，不使用脚本或内容重写。

语法与使用参考：[LOWERTOP/Shadowrocket 社区手册](https://github.com/LOWERTOP/Shadowrocket)，作者/维护者为 LOWERTOP 及其贡献者；[在线文档页面](https://lowertop.github.io/Shadowrocket/)。该手册是社区资料，不代表应用开发商为本配置背书。

第三方项目的作者归属与许可保持不变。本仓库仅在配置中引用其远程列表，没有复制完整第三方规则库；本文列出的许可说明也不替代各上游许可正文。更多原始文件链接见 [SOURCES.md](SOURCES.md)。
