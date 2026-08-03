# 本地知识库（不上传 GitHub）

目录：`notes/local/`（已在根目录 `.gitignore` 中忽略）

## 当前内容

- **集中教学/**：完整文档已导入，仅本机笔记页可见
- **catalog.json**：本地目录树；`notes.html` 启动时自动合并进侧栏「本地专区」

## 本机预览

```bash
cd "/media/zyb/DateB/Zhao Yongbo's Personal Homepage"
python3 -m http.server 8765
```

打开 `http://127.0.0.1:8765/notes.html`，应看到「已加载本地库」。

## 会上传 GitHub 的内容

知识口袋其余领域在 `notes/files/kb/` + `notes/kb-tree.js`，会随仓库推送。

已排除：单文件 &gt;40MB、空文件、软件激活/破解相关、多数二进制安装包与模型权重。
