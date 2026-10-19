# Website — PixelBlue LLC 公司官网

> 先读总文件夹的 `../CLAUDE.md`（工作规则），再读本文件。
> 最后更新：2026-10-19（官网从 Northwest 的 WordPress 迁到 GitHub Pages 并上线；Prec 状态改为已上架）

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
├── index.html  services.html  about.html  contact.html  404.html
├── products/             产品列表 index.html + 每个产品一个详情页（prec.html）
├── blog/                 博客列表 index.html + 每篇一个 .html
├── assets/               style.css、logo.svg（像素 PB）、favicon.svg
│   └── products/prec/    Prec 的图标（icon.svg）
├── CNAME                 pixelbluellc.com —— 不要删，删了自定义域名会失效
├── _config.yml           把 README.md、CLAUDE.md、docs/ 排除在发布之外
├── README.md             英文简介
├── CLAUDE.md             本文件
└── docs/HOSTING.md       域名、DNS、托管的配置记录和变更历史
```

`/prec-privacy/privacy-policy.html` 不在这个仓库，来自另一个仓库 `PixelBlue-LLC/prec-privacy`（项目页，自动挂在同一域名下）。

## 3. 怎么改网站

- **页头导航和页脚在每个页面里各复制了一份**（共 10 个 HTML 文件）。导航顺序：Home / Products / Services / About / Blog / Contact。改导航、页脚、邮箱时要把所有页面一起改，可以用 `grep -rl` 找到再批量替换。
- 样式全部在 `assets/style.css`，顶部 `:root` 是颜色变量：
  - `--ink #14213D`（正文）、`--deep #16388A`（主色/按钮）、`--blue #2F64D6`（强调）、`--pale #DCE8FA`（浅底）、`--line #D8E1EE`（分隔线）
  - 字体：Schibsted Grotesk（Google Fonts）
- 首页右侧的像素 PB 是内联 SVG，`rect.on` 是字母像素（加载时有一次动画，尊重“减少动态效果”设置）。
- **加博客文章**：复制一篇 `blog/*.html` 改内容，再在 `blog/index.html` 的列表最上面加一条。日期写 `<time datetime="YYYY-MM-DD">`。
- **Products 页**（`products/index.html`）刻意保持简洁：每个产品一行（图标、名字、一句话、链接或状态），不放截图和参数。
- **产品详情页**不要做成 App Store 那种样式（图标 + 参数栏 + 截图条）。详情页**统一用公司配色**（2026-10-19 用户决定，不用产品自己的配色）；`body.theme-prec` 只是把页面变量映射到公司颜色。文案用该产品品牌文档的语气，首屏是一个可以勾选和点“Ready for Next Time”的小清单演示（页面底部的内联脚本）。
- **Prec 发新版本时**改 `products/prec.html` 两处：首屏下方的版本号，以及“Release notes”最上面加一条（版本号、月份、一句话概括、App Store 的 What's New 条目）。What's New 原文在 `../Prec/docs/RELEASE-x.x.md`。
- **加新产品**（如 MultiDock 公开发布后）：`products/` 下新建详情页（参考 `prec.html` 的结构，同样用公司配色），在 `products/index.html` 加一行，首页“What we're building”也加一行。
- 产品状态（首页“What we're building”和 Products 页的“In the works”）要和实际一致：
  - Prec：On the App Store，链接 https://apps.apple.com/us/app/prec/id6771575207
  - Family Care Coordination：In research（还没代码）
  - Housekeeping Route Scheduling：Planned
- MultiDock 还没公开、名字也要改，**正式发布前不要放到网站上**。以后它的落地页可以放这里（见 `../MultiDock/docs/LAUNCH-PLAN.md`）。
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
- GitHub Pages 不允许用作电商/SaaS 主站；展示公司、产品介绍、隐私政策、支持页面没问题。卖 MultiDock 的付款走 Lemon Squeezy 托管页面。
- Cloudflare 里 GitHub Pages 的 5 条记录必须是 **DNS only（灰色云）**，否则 HTTPS 证书续期会失败。

## 6. 待办

- [ ] Prec 下次发版时，把 App Store Connect 的 Privacy Policy URL 改成 `https://pixelbluellc.com/prec-privacy/privacy-policy.html`（只能随新版本改；旧 github.io 链接仍可用）
- [ ] 等 Northwest 回复：WordPress 备份、删除 WordPress 主机、开启 Registrar Lock、确认域名续费包含在注册代理服务里（2026-10-19 已发邮件）
- [x] 在 Northwest 开启 Domain Privacy（2026-10-19）
- [x] Cloudflare 删除两条 `_acme-challenge` TXT 记录（2026-10-19）
- [ ] 商业计划书封面的 pixelbluellcus.com / info@pixelbluellcus.com 改成 pixelbluellc.com
- [ ] 审阅网站文字和两篇博客初稿（2026-10-19 由 Claude 根据商业计划书和 Prec 文档撰写）
- [x] GitHub 组织资料填上网站 https://pixelbluellc.com（2026-10-19）
- [ ] 可选：`prec-privacy` 仓库的 About 里填上网站
