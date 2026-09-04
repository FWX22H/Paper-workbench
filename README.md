# 论文阅读工作台 · Paper Reading Workbench

一个本地优先的论文阅读与笔记工作台，把“导入论文 → 阅读批注 → 笔记整理 → AI 问答 → 公式识别 → 进度复盘”放在同一个界面中完成。

PDF、笔记索引和设置默认保存在浏览器本地；也可以授权一个本地文件夹，把 PDF 与 Markdown 笔记写入磁盘，方便后续用 Obsidian 等工具继续管理。

## 功能特性

### 论文管理

- 拖拽或批量导入 PDF，自动提取标题、作者、年份、期刊、DOI 等元数据
- 输入 DOI 或 arXiv ID，在线获取元数据后导入
- 按元数据或全文搜索论文库，并按未读 / 在读 / 已读筛选
- 记录阅读时长，生成每日学习报告

### PDF 阅读

- 内置 EmbedPDF / MuPDF / PDFium 阅读器，也支持唤起 SumatraPDF 外部阅读
- 页码跳转、缩放、适合宽度、双栏对照
- 文本高亮、批注、划词复制 / 翻译，可将译文直接插入笔记
- 本地后端通过 PyMuPDF 提供 PDF 信息、页面渲染和文本提取接口

### 笔记

- Markdown 编辑与预览
- 导入论文后按可编辑模板自动生成笔记
- 每篇论文提供“阅读笔记 / AI 分析 / 公式 / AI 问答”页签
- 可选择本地保存文件夹，把 PDF 和 `.md` 笔记写入磁盘

### AI 服务

- 内置 OpenAI、DeepSeek、Anthropic Claude、Google Gemini、Ollama 预设，也可填写自定义 OpenAI 兼容 Base URL
- 支持 AI 元数据补全、单篇论文分析、结合当前论文上下文的问答、划词翻译、公式修正与公式解释
- API Key 保存在浏览器本地配置中，不写入代码或仓库

### 公式识别

- 在 PDF 页面上拖拽框选公式，识别为 LaTeX
- 支持 AI 多模态联网识别，无需安装本地模型
- 也支持本地 FreeTex / UniMERNet 引擎
- 输出兼容 Obsidian / KaTeX，可手动修正后插入笔记

## 快速开始

### 使用打包版本

1. 从 GitHub Releases 下载 `PaperWorkbench.zip`（本地开发产物位于 `dist/PaperWorkbench.zip`）。
2. 解压后运行 `PaperWorkbench.exe`。
3. 程序会启动本地服务并自动打开 `http://127.0.0.1:8504/paper-workbench.html`；若浏览器未自动打开，可手动访问该地址。
4. 首次使用请点击右上角“设置”，在“AI 服务”中填写服务商、Base URL、模型和 API Key。

打包版默认使用“AI 联网公式识别”，不需要本地安装 UniMERNet 模型。


## 配置说明

### AI 服务

在页面的“设置 → AI 服务”中配置。内置服务商会自动填充常用 Base URL 和模型名称，也可以切换到“自定义”填写任意 OpenAI 兼容接口。

### 公式识别

在“设置 → 公式识别”中可选择：

- **AI 联网识别**：将公式截图发送给已配置的多模态 AI 服务，无需本地模型
- **本地 FreeTex (UniMERNet)**：调用本地模型目录进行识别

本地模型目录需包含 `demo.yaml` 和 `models/unimernet_small/unimernet_small_fp16.pth`。可通过页面中的“模型目录”配置，或写入 `freetex-model.json`：

```json
{
  "model_dir": "D:/FreeTex"
}
```

`freetex-model.json.example` 是公开示例；实际运行时的 `freetex-model.json` 含本机路径，建议加入 `.gitignore`，不要提交到仓库。

### 笔记与文件存储

- 未选择保存文件夹时，论文索引、设置保存在浏览器 `localStorage`，PDF 文件保存在浏览器 `IndexedDB`
- 选择本地文件夹后，PDF 与 `.md` 笔记会按模板子目录（如 `assets/pdfs/{{year}}`）写入磁盘
- “设置 → 存储”中可导出或导入配置
- 文件夹选择依赖浏览器 File System Access API，请使用 Chrome / Edge


## 端口一览

| 端口 | 用途 |
| --- | --- |
| `8504` | FreeTex / FastAPI 后端，打包版也在此提供主页面 |
| `8642` | 源码运行时的无 CSP 静态预览服务 |

所有服务默认只监听 `127.0.0.1`，不会把论文或笔记上传到除用户配置的 AI 服务之外的第三方服务器。

## 隐私提示

- 仓库中只保留 `freetex-model.json.example`，不要把本机模型路径写入公开示例
- 不要提交 `paper-workbench.log`、浏览器导出的配置或包含 API Key 的文件
- AI 分析、翻译和联网公式识别会把对应的文本或公式截图发送到你配置的 AI 服务，使用前请确认内容可接受

---

# 如果各位喜欢不妨给作者点杯奶茶，您的支持就是作者继续优化的动力

<img width="1325" height="1806" alt="233bb30f5ff4a2e07adf543adc14dcf3" src="https://github.com/user-attachments/assets/327b7c25-160b-4d95-a4f4-c1dc8802cb51" />


