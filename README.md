# 超星原始课件下载 · Chaoxing Courseware Downloader

**按章节批量下载超星学习通课件的油猴（Tampermonkey）脚本：有 PPT / PPTX / PDF 原文件时优先保留原扩展名下载；当章节或资源没有开放原文件（或原件链接受限）时，自动回退下载 PDF 预览版本并规范保存为 `.pdf`。**

> A Tampermonkey userscript for Chaoxing (超星学习通) that batch-downloads course slides and documents: preserves original `.ppt`, `.pptx`, and `.pdf` files whenever available, and automatically falls back to converted `.pdf` previews when originals are absent or restricted.

[中文](#中文) · [English](#english)

---

## 中文

### 核心特性（对比原版 `Chaoxing PDF Dumper 0.6.1`）

| 能力 | 原版 `0.6.1` | 本版 `0.8.0` |
|---|---|---|
| 有原文件（PPT / PPTX / PDF） | 强制加 `.pdf` 后缀，甚至把预览 PDF 存成假 PPT | **完整保留 `.pptx` / `.ppt` / `.pdf` 原扩展名与原始字节** |
| 无原文件（只有在线预览） | 下载为 `.pdf` | **自动回退下载 PDF 预览**，并剥离原文档后缀、保存为 `.pdf`（绝不出现 `.pptx.pdf`），面板带橙色 `PDF 预览` 徽标提示 |
| 原件下载遇到 403/404 或返回 PDF 正文 | 直接报错或存成错格式 | **运行时自动回退到 `pdf` 预览流并重新校验文件头** |
| 文件头完整性检查 | 无 | **下载前校验 ZIP/OLE/PDF 魔数**，拒收登录失效 HTML 页与空响应（兼容带 UTF-8 BOM 的 PDF） |
| 批量下载容错 | 遇单个失败即中断 | **跳过失败文件并继续下载其余选中课件**，结束后汇总成功/失败数 |
| 多域名与防冲突 | 仅限两个主域名、面板 ID 易冲突 | 支持 `*.chaoxing.com`（含高校定制二级域名），**独立面板 ID 与全局命名空间** |

> 音视频资源（`.mp4` / `.mp3` 等）会自动排除，避免被误下载。

---

### 安装方法

#### 方式一：Greasy Fork 一键安装（推荐）

1. 在浏览器安装 **Tampermonkey（篡改猴）** 扩展。
2. 打开 [Greasy Fork 安装页](https://greasyfork.org/zh-CN/scripts/607172)，点击「**安装此脚本**」并在油猴确认页保存。

#### 方式二：从 GitHub 直接安装

1. 安装 **Tampermonkey** 后，直接点击打开：[chaoxing-original-downloader.user.js (Raw)](https://raw.githubusercontent.com/li-tao-yao/chaoxing-original-downloader/main/chaoxing-original-downloader.user.js)。
2. 油猴会自动弹出安装确认界面，点击「安装」即可。

> **升级提示**：如果之前装过原版 `Chaoxing PDF Dumper` 或旧版，建议在油猴管理面板停用旧脚本，避免页面右下角出现两个悬浮按钮。

---

### ⚠️ Edge / Chrome 安装后必看（常见“找不到按钮”原因）

在基于 Chromium MV3 的新版 **Edge / Chrome** 中安装油猴后，必须满足以下两项权限脚本才会注入：

1. **开启「允许用户脚本 / 开发人员模式」**：
   * 打开浏览器扩展管理页（Edge：`edge://extensions/`；Chrome：`chrome://extensions/`）。
   * 找到 **Tampermonkey → 详细信息**，开启 **「允许用户脚本（Allow User Scripts）」**（若无此开关，则在扩展管理页开启右上角/左下角的 **「开发人员模式（Developer mode）」**）。
2. **检查站点访问权限**：确保油猴扩展的「网站访问权限」包含 `chaoxing.com`（建议选“在所有网站上”或包含 `*.chaoxing.com`）。

---

### 在哪些页面会显示「课件下载」按钮？

脚本不会在课程刚打开的首页门户显示，请进入以下任一页面（出现后点击右下角蓝色「**课件下载**」悬浮按钮）：

1. **新版课程章节列表页**（`mooc2-ans.chaoxing.com/mooc2-ans/mycourse/...`）：点击顶部 **「章节」** 标签页（URL 中带有 **`pageHeader=1`**），可扫描并勾选全课程所有章节课件。
2. **旧版课程目录页**（`*.chaoxing.com/mycourse/studentcourse?...`）：带 `courseid` 与 `clazzid` 时显示，支持全课程扫描。
3. **具体章节学习页**（`*.chaoxing.com/mycourse/studentstudy?...`）：带 `chapterId`、`courseid`、`clazzid` 时显示，仅扫描当前章节（并额外展示本章节中的其他资源条目方便排查）。

---

### 常见问题（FAQ）

* **为什么面板里显示 `PPTX → PDF 预览`？**
  说明教师关闭了原文件下载权限或平台未提供 `download` 原件地址（仅开放了在线预览），脚本已自动为你切到 PDF 预览下载，保存后文件名会自动变为 `xxx.pdf`。
* **为什么刷新页面还是没有按钮，油猴图标也没有数字？**
  90% 以上是因为 Edge/Chrome 未开启「允许用户脚本 / 开发者模式」，或当前处于课程首页而不是带有 `pageHeader=1` 的「章节」页面。请按上方两节核对。
* **批量下载中个别文件失败怎么办？**
  可能是临时签名链接过期。脚本会自动跳过失败文件并继续下载其余课件；结束后点击面板上的「**重新扫描**」获取新链接再重试失败项即可。

---

## English

### Overview

**Chaoxing Courseware Downloader** is a Tampermonkey userscript for Chaoxing (`*.chaoxing.com`) that batch-downloads course slides and handouts:

- **Original format first**: Saves `.pptx`, `.ppt`, and `.pdf` files with their exact original filenames and binary payloads whenever `download` links are available.
- **Automatic PDF preview fallback**: When an attachment or chapter has no original download URL (or the original endpoint returns `403`/`404`/converted PDF bytes), it automatically falls back to the converted `pdf` preview stream and saves it cleanly as `.pdf` (never `.pptx.pdf`), marked with a `PDF 预览` badge in the UI.
- **Magic-byte validation**: Verifies ZIP (`PK`), OLE, or `%PDF-` headers (including PDFs with a leading UTF-8 BOM) before saving, preventing expired HTML login pages from being saved as broken files.
- **Fault-tolerant batch downloads**: Logs and skips failed items without aborting the remaining queue.
- **Isolated & conflict-free**: Uses its own panel ID (`cxorig-downloader-panel`) and global API (`window.ChaoxingOriginalDownloader`), and supports custom school subdomains (`*.chaoxing.com`).

### Installation

1. Install **Tampermonkey** in Edge, Chrome, or Firefox.
   - **Important for Edge / Chrome (MV3)**: Open `edge://extensions/` (or `chrome://extensions/`), open Tampermonkey **Details**, and enable **Allow User Scripts** (or toggle **Developer mode**), then ensure site access allows `*.chaoxing.com`.
2. Install from [Greasy Fork](https://greasyfork.org/zh-CN/scripts/607172) or directly from [GitHub Raw](https://raw.githubusercontent.com/li-tao-yao/chaoxing-original-downloader/main/chaoxing-original-downloader.user.js).
3. Disable older copies / upstream `Chaoxing PDF Dumper` if already installed.
4. Open your course's **Chapters (章节)** tab (`pageHeader=1` on `mooc2-ans`) or a `studentstudy` / `studentcourse` page, then click the floating **课件下载** button at the bottom right.

## 来源与许可 · Attribution & License

基于 [Fiatnorm 的 Chaoxing PDF Dumper 0.6.1](https://github.com/Fiatnorm/Chaoxing-PDF-Dumper) 二次开发与重构。完整许可声明见 [LICENSE](LICENSE)。
