# Website — PixelBlue LLC 公司官网

> 先读总文件夹的 `../CLAUDE.md`（工作规则），再读本文件。
> 最后更新：2026-10-19（新增 TandemDock 产品页，Coming soon；官网从 Northwest 的 WordPress 迁到 GitHub Pages 并上线）

---

## 1. 这是什么

pixelbluellc.com 的源码。纯静态 HTML，**没有构建步骤**，推到 `main` 后 GitHub Pages 自动发布（1–2 分钟）。

| 项目 | 值 |
|---|---|
| 网址 | https://pixelbluellc.com （HTTPS 已强制） |
| GitHub 仓库 | `PixelBlue-LLC/PixelBlue-LLC.github.io`（**公开**，组织主页仓库） |
| 本机路径 | `~/Documents/PixelBlue-LLC/Website` |
| 托管 | GitHub Pages，`main` 分支 `/ (root)`，免费 |
| DNS | Cloudflare（免费版），记录见 `docs/HOSTING.md` |
| 域名注册商 | Northwest Registered Agent（随注册代理服务附送） |

## 2. 文件结构

```
Website/
├── index.html  services.html  about.html  contact.html  404.html  legal.html（Privacy and Terms 总页，列出每个产品的隐私政策和条款）
├── products/             产品列表 index.html + 每个产品一个详情页（prec.html、tandemdock.html）
│                         + TandemDock 的隐私政策 tandemdock-privacy.html 和授权条款 tandemdock-terms.html
├── blog/                 博客列表 index.html + 每篇一个 .html
├── assets/               style.css、logo.svg（像素 PB）、favicon.svg
│   └── products/         每个产品的图标：prec/icon.svg、tandemdock/icon.svg
├── tandemdock/appcast.xml  TandemDock 的 Sparkle 更新源（App 每天读一次）；条目由 TandemDock 仓库的 scripts/publish.sh 加，`<!-- items -->` 标记不能删
├── CNAME                 pixelbluellc.com —— 不要删，删了自定义域名会失效
├── _config.yml           把 README.md、CLAUDE.md、docs/ 排除在发布之外
├── README.md             英文简介
├── CLAUDE.md             本文件
└── docs/HOSTING.md       域名、DNS、托管的配置记录和变更历史
```

`/prec-privacy/privacy-policy.html` 不在这个仓库，来自另一个仓库 `PixelBlue-LLC/prec-privacy`（项目页，自动挂在同一域名下）。

## 3. 怎么改网站

- **页头导航和页脚在每个页面里各复制了一份**（共 15 个 HTML 文件）。导航顺序：Home / Products / Services / About / Blog / Contact。**页脚所有页面一样**（2026-10-23 统一）：版权 · 公司邮箱 · “Privacy and Terms”（`/legal.html`）。新产品的隐私政策、条款加到 `legal.html`，不要再往页脚里加产品链接；产品页正文里可以直接链自己的政策。改导航、页脚、邮箱时要把所有页面一起改，可以用 `grep -rl` 找到再批量替换。
- 样式全部在 `assets/style.css`，顶部 `:root` 是颜色变量：
  - `--ink #14213D`（正文）、`--deep #16388A`（主色/按钮）、`--blue #2F64D6`（强调）、`--pale #DCE8FA`（浅底）、`--line #D8E1EE`（分隔线）
  - 字体：Schibsted Grotesk（Google Fonts）
- 首页右侧的像素 PB 是内联 SVG，`rect.on` 是字母像素（加载时有一次动画，尊重“减少动态效果”设置）。
- **加博客文章**：复制一篇 `blog/*.html` 改内容，再在 `blog/index.html` 的列表最上面加一条。日期写 `<time datetime="YYYY-MM-DD">`。
- **Products 页**（`products/index.html`）刻意保持简洁：每个产品一行（图标、名字、一句话、链接或状态），不放截图和参数。
- **产品详情页**不要做成 App Store 那种样式（图标 + 参数栏 + 截图条）。详情页**统一用公司配色**（2026-10-19 用户决定，不用产品自己的配色）；`body.theme-prec` 只是把页面变量映射到公司颜色。文案用该产品品牌文档的语气，首屏是一个可以勾选和点“Ready for Next Time”的小清单演示（页面底部的内联脚本）。
- **Prec 发新版本时**改 `products/prec.html` 两处：首屏下方的版本号，以及“Release notes”最上面加一条（版本号、月份、一句话概括、App Store 的 What's New 条目）。What's New 原文在 `../Prec/docs/RELEASE-x.x.md`。
- **TandemDock 页**（`products/tandemdock.html`，`body.theme-tandemdock`，和 Prec 共用公司配色的详情页样式）：首屏右边是可互动的 Dock 演示（鼠标划过放大、切换底/左/右），脚本在页面底部，样式在 `style.css` 末尾 `.ed-*`。现在是 **Coming soon**：主按钮是 `mailto:` 发邮件“告诉我上线”，没有下载和价格。
  - 正式发布时要改：状态改成 Available（首页、Products 页、详情页三处）；主按钮换成下载 DMG；加 “Buy Pro” 按钮（Lemon Squeezy 付款链接）；首屏下方写版本号；加 Release notes（照 Prec 页）。
  - **隐私政策和条款**（2026-10-23）：`products/tandemdock-privacy.html`、`products/tandemdock-terms.html`，TandemDock 页和这两页的页脚有链接，App 的 About 页也链到这里。条款里的数字是用户定的：一人最多 3 台 Mac、14 天无理由退款、加州法律。App 联网行为变了（如接入 Sparkle）要一起改隐私政策。
  - 页面上**不写具体价格**，直到 Lemon Squeezy 定价确定。
  - 图标 `assets/products/tandemdock/icon.svg` 和 App 图标是同一张图（2026-10-22 定稿方案 F4，原稿在 TandemDock 仓库 `Support/AppIcon.svg`）。App 图标改了，这里要一起换。
- **加新产品**：`products/` 下新建详情页（参考 `prec.html` 的结构，同样用公司配色），在 `products/index.html` 加一行，首页“What we're building”也加一行。
- 产品状态（首页“What we're building”和 Products 页的“In the works”）要和实际一致：
  - Prec：On the App Store，链接 https://apps.apple.com/us/app/prec/id6771575207
  - Family Care Coordination：In research（还没代码）
  - TandemDock：Coming soon（2026-10-19 用户要求挂到 Products 下；App 还没发布）
  - Housekeeping Route Scheduling：Planned
- TandemDock（原名 MultiDock）的上架计划见 `../TandemDock/docs/LAUNCH-PLAN.md`。名字中间考虑过 Everydock，因 GitHub 上已有同名开源 App 放弃，网站上从未出现过。
- 网站文字用英文；`CLAUDE.md` 和 `docs/` 用中文。

## 4. 发布流程

1. 改文件（本机或云端 clone）。
2. 用户说“提交/推送”后 commit 并 push 到 `main`。云端代理可以推分支到这个仓库。
3. 等 1–2 分钟，打开 https://pixelbluellc.com 检查。
4. 本机副本同步：用户在终端 `cd ~/Documents/PixelBlue-LLC/Website && git pull`（本机已用 `gh` 登录）。

本地预览：`cd Website && python3 -m http.server 8000`，浏览器开 http://localhost:8000 。页面用的是 `/assets/...` 绝对路径，直接双击 HTML 文件打开会没有样式。

## 5. 注意事项

- **仓库是公开的**：不要放密码、token、个人住址/个人电话/个人邮箱、财务信息。联系方式只用公司邮箱 info@pixelbluellc.com 和网站上已公开的公司电话。
- GitHub Pages 不能跑后端，联系方式只能用 `mailto:` 链接。将来要表单需接第三方服务（如 Formspree）。
- GitHub Pages 不允许用作电商/SaaS 主站；展示公司、产品介绍、隐私政策、支持页面没问题。卖 TandemDock Pro 的付款走 Lemon Squeezy 托管页面。
- Cloudflare 里 GitHub Pages 的 5 条记录必须是 **DNS only（灰色云）**，否则 HTTPS 证书续期会失败。

## 6. 待办

- [ ] Prec 下次发版时，把 App Store Connect 的 Privacy Policy URL 改成 `https://pixelbluellc.com/prec-privacy/privacy-policy.html`（只能随新版本改；旧 github.io 链接仍可用）
- [x] Northwest：开启 Registrar Lock、WordPress 备份（存 `../_archive/`，不进 Git）、取消旧主机（2026-10-20）
- [x] Northwest：确认域名 2027-02-28 自动免费续费，注册代理服务不受影响（2026-10-20）
- [x] 在 Northwest 开启 Domain Privacy（2026-10-19）
- [x] Cloudflare 删除两条 `_acme-challenge` TXT 记录（2026-10-19）
- [ ] 商业计划书封面的 pixelbluellcus.com / info@pixelbluellcus.com 改成 pixelbluellc.com
- [ ] 审阅网站文字和两篇博客初稿（2026-10-19 由 Claude 根据商业计划书和 Prec 文档撰写）
- [x] GitHub 组织资料填上网站 https://pixelbluellc.com（2026-10-19）
- [ ] 可选：`prec-privacy` 仓库的 About 里填上网站
