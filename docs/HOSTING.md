# 域名、DNS 与托管

pixelbluellc.com 由三家服务分工：

| 层 | 服务 | 作用 | 费用 |
|---|---|---|---|
| 域名注册 | Northwest Registered Agent | 域名归属与续费（2026-02-28 注册，**2027-02-28 到期**） | 随注册代理服务附送 |
| DNS | Cloudflare（免费版，nameserver `hope` / `thomas.ns.cloudflare.com`） | 网站、邮箱的解析记录 | 免费 |
| 网站托管 | GitHub Pages（仓库 `PixelBlue-LLC/PixelBlue-LLC.github.io`） | 静态网页 + HTTPS 证书 | 免费 |
| 邮箱 | Google Workspace | info@pixelbluellc.com | 另计 |

改网站只碰 GitHub；改解析只碰 Cloudflare；Northwest 只管续费和域名锁。

## Cloudflare DNS 记录（2026-10-19）

### 网站（GitHub Pages）——必须 DNS only（灰色云）

| 类型 | 名称 | 内容 |
|---|---|---|
| A | `@` | 185.199.108.153 |
| A | `@` | 185.199.109.153 |
| A | `@` | 185.199.110.153 |
| A | `@` | 185.199.111.153 |
| CNAME | `www` | pixelblue-llc.github.io |
| TXT | `_github-pages-challenge-PixelBlue-LLC` | GitHub 组织域名验证值 —— **一直保留**，删了验证会失效 |

### 邮箱（Google Workspace）——不要动

| 类型 | 名称 | 内容 |
|---|---|---|
| MX | `@` | aspmx.l.google.com (1)、alt1/alt2 (5)、alt3/alt4 (10) |
| TXT | `@` | `v=spf1 include:_spf.google.com ~all` |
| TXT | `google._domainkey` | Google DKIM 公钥 |
| TXT | `_dmarc` | `v=DMARC1; p=none;` |
| TXT | `@` | `google-site-verification=…` |
| TXT | `cf2024-1._domainkey` | Cloudflare 的 DKIM（邮件相关，保留） |

### 已删除

- TXT ×2 `_acme-challenge`：旧 Northwest 主机签 SSL 证书用的，2026-10-19 删除。当前共 16 条记录。

## GitHub 设置

- 仓库 Settings → Pages：Deploy from a branch，`main` / `(root)`；Custom domain `pixelbluellc.com`；**Enforce HTTPS 已勾选**。
- 组织 Settings → Pages：`pixelbluellc.com` 已验证（Verified），防止其他账号冒用该域名。
- `prec-privacy` 仓库的 Pages 是项目页，因为组织主页绑定了自定义域名，它自动出现在 `https://pixelbluellc.com/prec-privacy/`；旧的 `pixelblue-llc.github.io/prec-privacy/…` 链接也仍然能打开。

## Northwest 账户状态

- Domain Status / Plan：Active；Nameserver：Cloudflare（**不要改回去**，邮箱和网站都依赖 Cloudflare 上的记录）。
- Registrar Lock：**On**（2026-10-20 Northwest 确认已开启）。以后要转移域名时才关。
- Domain Privacy：2026-10-19 已开启（免费），WHOIS 不公开联系人信息。
- Domain Parking：保持 Off。
- Hosting：免费的 Website Hosting 套餐（agency 建的旧 WordPress，66.223.49.89）**已于 2026-10-20 取消**。取消前 Northwest 提供了完整备份（网站文件 + 数据库），存在本机 `~/Documents/PixelBlue-LLC/_archive/transfer-01a15a16/`，**不进 Git**（含 `wp-config.php` 数据库密码）。备份里只有 3 个模板占位页面、无图片、1 条垃圾表单提交，没有需要迁移的内容。
- 域名续费：还在等 Northwest 确认是否包含在注册代理服务里（到期 2027-02-28，最晚 2027-01 问清楚）。
- 域名联系邮箱保持用**不依赖本域名**的邮箱，否则域名出问题时收不到通知。

### 要不要把域名转到 Cloudflare Registrar？（2026-10-19 结论：暂不转）

- 现在：只要继续用 Northwest 的注册代理服务，续费包含在内（以 Northwest 回信确认为准）。
- Cloudflare Registrar：.com 注册/续费/转入约 $10.46/年（成本价）。
- 什么时候转：不再用 Northwest 做注册代理时。提前 1–2 个月处理（到期 2027-02-28）。流程：Northwest 关 Registrar Lock → 生成授权码（像密码一样保管）→ 在 Cloudflare 发起转入。转移时 nameserver 不变，网站和邮箱不中断。

## 变更历史

### 2026-10-19 网站打不开（Cloudflare 520）→ 迁到 GitHub Pages

- **现象**：pixelbluellc.com 显示 Cloudflare Error 520（浏览器、Cloudflare 正常，源站出错）。
- **原因**：之前为了做 GitHub Pages 把 nameserver 从 Northwest 改到了 Cloudflare，但迁移没做完。Cloudflare 自动复制的 A 记录（`@`、`www`、`*` → 66.223.49.89，橙色云代理）仍指向 Northwest 的 WordPress 主机，而该主机不正常响应经 Cloudflare 代理的请求。
- **处理**：
  1. 新建仓库 `PixelBlue-LLC.github.io`，上传新官网（Home / Services / About / Blog / Contact），开启 Pages，绑定 `pixelbluellc.com`。
  2. Cloudflare 删除三条旧 A 记录，加 4 条 GitHub Pages A 记录 + `www` CNAME，全部 DNS only。
  3. GitHub DNS check 通过 → 证书签发 → 勾选 Enforce HTTPS。
  4. 在组织层面验证域名（加 `_github-pages-challenge` TXT）。
  5. 网站上 Prec 从“Coming soon”改为已上架，加 App Store 链接。
  6. 收尾：开 Domain Privacy、删 `_acme-challenge` 记录、GitHub 组织资料填网站地址；给 Northwest 发邮件处理旧 WordPress 和 Registrar Lock。
  7. 2026-10-20：Northwest 开启 Registrar Lock → 发来 WordPress 备份（已检查，无可迁移内容）→ 取消旧主机。取消后检查网站、NS、MX、A 记录全部正常。
- 3 月时的背景：当时 DNS 还在 Northwest，Google Workspace 的 MX 记录是请 Northwest 客服加的；切到 Cloudflare 时这些记录被自动复制过来，所以邮箱一直正常。

### 以后改 DNS 前先确认

- 改 nameserver 之前，确认新 DNS 里已经有全部邮箱记录（MX、SPF、DKIM、DMARC）。
- 不要给 GitHub Pages 的记录开橙色云代理。
- 改完在 GitHub 仓库 Settings → Pages 看 DNS check 状态。
