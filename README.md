# Zecheng Zhou 个人主页

基于 [Jekyll](https://jekyllrb.com/) 与 [Just the Docs](https://just-the-docs.github.io/just-the-docs/) 主题构建，通过 GitHub Pages 发布。

**在线地址：** https://pku-zhou.github.io

---

## 仓库结构

```
├── index.md              # 首页（个人介绍，layout: home）
├── _config.yml           # 站点标题、URL、默认布局等
├── notes/                # 学习笔记
│   ├── index.md          # 「我的笔记」入口
│   ├── tapeout/          # 流片笔记
│   ├── gpu-coding/       # GPU 编程笔记
│   └── embodied-ai/      # 具身智能笔记
└── assets/               # 图片等静态资源
```

侧栏层级示意：

```
Home
└── 我的笔记
    ├── 流片笔记
    │   └── …（子分支 / 单篇笔记）
    ├── 具身智能笔记
    └── GPU 编程笔记
```

---

## 写作规范

1. **Front matter 必填**  
   每个要出现在网站上的 `.md` 文件，开头必须用 `---` 包裹 YAML 配置，至少包含 `title`。

2. **`parent` 与 `title` 必须完全一致**  
   子页的 `parent: xxx` 中的 `xxx`，必须和父页 front matter 里的 `title: xxx` 一字不差（含中文）。

3. **同级排序用 `nav_order`**  
   数字越小越靠上；只影响同一 `parent` 下的兄弟页面。三个领域笔记在 `parent: 我的笔记` 下分别设置 `nav_order: 1/2/3` 即可调整顺序。

4. **静态资源放 `assets/`**  
   例如 `assets/images/avatar.jpg`，在 Markdown 中引用：`![说明](/assets/images/avatar.jpg)`。


5. **在仓库根目录运行 Jekyll**  
   不要在 `notes/` 子目录里执行 `jekyll serve`，否则只会构建笔记子目录，首页和完整导航会丢失。

---

## 如何修改内容?

### 改首页

编辑根目录 [`index.md`](index.md) 的正文；front matter 中保持 `layout: home`。

### 在已有领域下增加一篇笔记

在对应领域目录下新建 `.md` 或 `子目录/index.md`，例如 `notes/tapeout/workflow/xxx.md`：

```yaml
---
title: 页面标题
parent: 流片笔记          # 父页 title；若挂在子分支下，则 parent 填子分支的 title
nav_order: 2
---

正文内容……
```

若该页下还有子页，父页需加 `has_children: true`。

### 新开一个领域（侧栏多一项）

1. 新建目录，如 `notes/new-topic/index.md`。
2. Front matter 示例：

```yaml
---
title: 新领域笔记
parent: 我的笔记
nav_order: 4              # 在「我的笔记」下的显示顺序
has_children: true        # 若该领域下会有子页
---

栏目简介……
```

3. 在该目录下继续添加子页面，并正确设置 `parent` 与 `nav_order`。

### 改站点名称、描述、URL

编辑 [`_config.yml`](_config.yml) 中的 `title`、`description`、`url`。

---

## 如何修改格式/字体/颜色...?

### 字体

### 虚线
```txt
<hr style="border: none; height: 2px; background: repeating-linear-gradient(90deg, #666 0px, #666 4px, transparent 4px, transparent 8px); border-radius: 2px; margin: 1.5em 0;">
```

### 文本框

样式定义在 [`_sass/custom/custom.scss`](_sass/custom/custom.scss)，片段在 [`_includes/callout.html`](_includes/callout.html)。在任意 Markdown 正文中写入：

```liquid
{% include callout.html type="tip" content="这是一条信息提示，用于说明注意事项。" %}
```

`type` 可选值：

| type | 效果 |
|------|------|
| `neutral` 或 `gray` | 灰色背景说明框 |
| `tip` 或 `info` | 蓝色左边框 + 💡 提示 |
| `warning` | 黄色左边框 + ⚠️ 警告 |
| `success` | 绿色左边框 + ✅ 成功 |
| `error` 或 `danger` | 红色左边框 + ❌ 错误 |
| `bordered` 或 `card` | 白底边框阴影 |
| `gradient` 或 `highlight` | 紫色渐变强调 |
| `modern` | 蓝色全边框圆角 |

多行或含 Markdown 语法时，用 `capture` 包一层：

```liquid
{% capture note_body %}
这是**多行**说明，可写列表：

- 第一项
- 第二项
{% endcapture %}
{% include callout.html type="neutral" content=note_body %}
```

自定义标题（或去掉默认标题）：

```liquid
{% include callout.html type="tip" label="📌 注意：" content="自定义标题的提示框。" %}
{% include callout.html type="warning" label=false content="无默认「警告」前缀的警告框。" %}
```

## 本地预览

首次或依赖变更后：

```bash
bundle install
```

在**仓库根目录**启动：

```bash
bundle exec jekyll serve
```

浏览器打开 http://127.0.0.1:4000 预览。修改 Markdown 后保存，页面会自动刷新（或稍等片刻）。

---

## 发布到 GitHub Pages

推送到 `main` 分支后，由 [GitHub Actions](.github/workflows/pages.yml) 自动构建并部署：

```bash
git status
git add .
git commit -m "简要说明本次修改"
git push
```

部署完成后访问 https://pku-zhou.github.io

构建与上线可能需要数分钟。

---

## 参考

- [Just the Docs 文档](https://just-the-docs.github.io/just-the-docs/)
- [Jekyll 文档](https://jekyllrb.com/docs/)
- [Qiyu Chen的教程](https://classicalqy.github.io/website_building/)
