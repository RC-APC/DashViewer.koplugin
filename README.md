# DashViewer —— KOReader 通用「看板查看器」插件

在 KOReader 内查看**多个云端看板的纯文本摘要**。数据源可自由添加 / 删除，
适用于**任意「产出纯文本的看板」**：肿瘤新药、豆瓣影视、基金净值、读书进度、RSS…… 与具体业务无关。

> 这是 [`onco_tracker`](https://github.com/) 项目的子组件（KOReader 插件部分）。
> 单独上传本插件即可，无需带上整套抓取管线。

---

## ✨ 功能

- **通用**：数据源只是一个「返回 UTF-8 纯文本的 URL」，任何看板都能接入。
- **多源管理**：内置三个数据源，可自由增删，设置持久化到设备本地。
- **纯文字显示**：KOReader 原生文字界面，E Ink 清晰、省电。
- **从文件批量导入**：在电脑写好 `dashviewer_sources.txt`，推到设备后一键导入，
  完全不依赖屏幕键盘（适配软键盘会被遮挡的定制 KOReader）。

## ⚠️ 设计说明（重要）

- 本插件**只显示纯文本**，不解码图片。原因：部分定制版 KOReader（如 MiuRead / Paperwhite）
  **图片解码会让程序直接崩出**（`imagewidget.lua:264 cannot render image`），因此图片模式已被移除。
- 界面文字全部硬编码中文，**不使用 `gettext` 翻译函数**（定制版 KOReader 的全局 `_` 是数值，
  `_()` 调用会令插件加载期崩溃、从菜单消失）。
- 设置读写整体包在 `pcall` 里：即使存档损坏，插件也不会在加载期崩溃消失。

## 📦 安装

把 **`DashViewer.koplugin`** 文件夹复制到 KOReader 插件目录：

| 设备 | 插件目录 |
|---|---|
| Kindle | `/mnt/us/koreader/plugins/` |
| Kobo | `/.adds/koreader/plugins/` |

重启 KOReader，主菜单「更多工具 → 看板查看器」出现即成功。
（阅读界面打开书后的顶部菜单同样可见。）

## 🚀 使用

- **点击**数据源名称 → 拉取并以纯文字方式显示最新摘要；
- **长按**数据源名称 → 删除该数据源；
- 「📄 从文件导入」→ 批量添加数据源。

### 新增自定义数据源

在电脑新建 `dashviewer_sources.txt`，**每行**写：

```
名称<TAB>URL
```

例如：

```
肿瘤新药动态	https://2435cc319f464e0eaaded08a80644163.app.workbuddy.link/digest.txt
豆瓣影视新书	https://b102a44faaf04c8ebaadb30e4783a396.app.workbuddy.link/digest.txt
微信读书榜单	https://9c18b55628f847a3a6628b3e2cada237.app.workbuddy.link/weread_digest.txt
我的基金	https://example.com/my-fund-digest.txt
```

> 插件**开箱内置**前三个数据源（肿瘤新药 / 豆瓣影视新书 / 微信读书榜单），安装即可用，无需手动添加。
> 这些 URL 指向作者本人的 WorkBuddy 公网链接，公开无害；也可长按删除、再导入自己的看板地址。

把文件放进 KOReader 数据目录（与 `dashviewer.lua` 同目录），再点「从文件导入」即可。

> 该 URL 用 GET 访问须返回 **UTF-8 纯文本**（如各看板的 `digest.txt`）。
> 局域网模式同样可用：运行本地服务后填 `http://<电脑IP>:PORT/digest.txt`。

## 🔧 接入任意看板的条件

只要你的看板能产出一份纯文本（例如用脚本生成 `digest.txt` 并发布到可访问网址），
把网址加为数据源即可。与数据来源、技术栈无关。

## 📁 目录结构

```
DashViewer/
├── DashViewer.koplugin/        ← 插件本体，复制进 KOReader 的 plugins/ 目录
│   ├── _meta.lua              ← 元信息（name=DashViewer, version=1.0.0）
│   └── main.lua               ← 插件逻辑
├── README.md
├── LICENSE                    ← MIT
└── .gitignore
```

## 📜 许可

MIT © RC-APC
