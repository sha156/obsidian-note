---
tags: [工具, Templater, 说明]
created: 2026-04-01
updated: 2026-04-01
---

# Templater 插件安装与使用说明

Templater 让模板文件变得"活的"——打开模板时自动填入日期、自动命名文件、自动移动到指定文件夹。

---

## 第一步：安装插件

1. 打开 Obsidian，进入 **设置（Settings）**
2. 左侧点击 **第三方插件（Community plugins）**
3. 关闭**安全模式**（如果还没关的话）
4. 点击 **浏览（Browse）**，搜索 `Templater`
5. 找到 **Templater**（作者 SilentVoid13），点击安装
6. 安装后点击**启用（Enable）**

---

## 第二步：配置插件

1. 进入 **设置 → Templater**
2. 找到 **Template folder location**，填入：`04-日志`
   （这样 Obsidian 知道去哪里找模板）
3. 开启 **Trigger Templater on new file creation**
   （新建文件时自动触发模板）

---

## 第三步：新建当天日记

有两种方式：

### 方式 A：命令面板（推荐）

1. 按 `Ctrl+P`（Mac 为 `Cmd+P`）打开命令面板
2. 输入 `Templater: Create new note from template`
3. 选择 **日记模板**
4. 文件会自动命名为今天的日期，自动保存到 `04-日志/` 文件夹

### 方式 B：直接打开模板

1. 打开 `04-日志/日记模板.md`
2. 按 `Ctrl+P` → 输入 `Templater: Replace templates in the active file`
3. 模板变量会替换为真实内容

---

## 模板语法速查

| 语法 | 含义 |
|------|------|
| `<% tp.date.now("YYYY-MM-DD") %>` | 今天的日期，格式：2026-04-01 |
| `<% tp.date.now("YYYY年MM月DD日") %>` | 中文格式日期 |
| `<% tp.date.now("dddd") %>` | 今天是星期几（英文） |
| `<%* await tp.file.rename(...) %>` | 重命名当前文件 |
| `<%* await tp.file.move(...) %>` | 移动文件到指定文件夹 |
| `<%* ... %>` | 执行 JS 代码块（不输出内容） |

---

## 常见问题

**Q：模板变量没有被替换，还是显示 `<% ... %>`？**
A：确认插件已启用，且用的是"Create new note from template"命令，不是直接复制粘贴。

**Q：文件没有自动移动到 04-日志/ 文件夹？**
A：检查 `tp.file.move()` 里的路径是否从 vault 根目录开始，且文件夹已存在。

**Q：想换日期格式怎么办？**
A：修改模板文件里 `tp.date.now()` 的格式字符串，格式规则见 [Moment.js 文档](https://momentjs.com/docs/#/displaying/format/)。
