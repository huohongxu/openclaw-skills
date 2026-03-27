# Paper Design - AI 驱动的设计画布

## 概述

Paper 是一个基于 Web 标准的连接式设计画布，支持 AI 代理直接读写设计文件。通过 MCP (Model Context Protocol) 服务器，可以让 AI 助手（如 Claude、Cursor、Codex）与 Paper 设计文件无缝协作。

**官网**: https://paper.design  
**文档**: https://paper.design/docs/mcp

## 核心特性

### 🎨 设计到代码的闭环
- 设计稿自动导出为 HTML/CSS 代码
- 代码修改可以反向同步到设计文件
- 一个数据源，始终保持最新

### 🔗 连接任何 AI 代理
支持连接到主流 AI 编程工具：
- **Cursor** - AI 代码编辑器
- **Claude Desktop** - Anthropic 官方客户端
- **Claude Code** - Claude 编码扩展
- **Codex** - OpenAI 编码代理
- **Copilot** - VS Code AI 助手
- **OpenCode** - 开源编码代理

### 📊 真实数据驱动设计
- 连接 Notion 数据库获取真实内容
- 从 Figma 同步设计 Token
- 使用真实内容替代 Lorem Ipsum

### 🤖 自动化繁琐工作
- 自动生成响应式布局
- 创建样式变体
- 一致性检查
- 重复性任务自动化

## 安装与配置

### 1. 下载 Paper Desktop

访问 https://paper.design/downloads 下载 Paper Desktop 应用。

### 2. 连接到 AI 工具

#### 连接到 Cursor

```bash
# Cursor 会自动检测 Paper MCP 服务器
# 打开 Cursor → Settings → Tools & MCP
# 点击 "Install" 安装 Paper MCP
```

#### 连接到 Claude Desktop

编辑 Claude Desktop 配置文件：

**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`  
**Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

添加以下内容：

```json
{
  "mcpServers": {
    "paper": {
      "command": "npx",
      "args": ["mcp-remote", "http://127.0.0.1:29979/mcp"]
    }
  }
}
```

#### 连接到 Claude Code

```bash
claude mcp add paper --transport http http://127.0.0.1:29979/mcp --scope user
```

#### 连接到 Codex

1. 打开 Codex Settings
2. 导航到 MCP Servers
3. 选择 "Streamable HTTP"
4. 名称: `paper`
5. URL: `http://127.0.0.1:29979/mcp`
6. 点击 "Save"

#### 连接到 OpenClaw

在 OpenClaw 配置中添加 MCP 服务器：

```json
{
  "mcpServers": {
    "paper": {
      "serverUrl": "http://127.0.0.1:29979/mcp"
    }
  }
}
```

### 3. 验证连接

在 AI 助手中输入：
```
在 Paper 中创建一个红色矩形
```

如果看到矩形出现在 Paper 中，说明连接成功！

## MCP 工具列表

Paper MCP 服务器提供以下工具：

### 读取工具 (Read-only)

| 工具 | 功能 |
|------|------|
| `get_basic_info` | 获取文件名、页面名、节点数、画板列表 |
| `get_selection` | 获取当前选中节点详情 |
| `get_node_info` | 通过 ID 获取节点详情 |
| `get_children` | 获取节点的直接子节点 |
| `get_tree_summary` | 获取节点树结构摘要 |
| `get_screenshot` | 截取节点截图 (base64) |
| `get_jsx` | 导出节点为 JSX (Tailwind 或内联样式) |
| `get_computed_styles` | 获取计算后的 CSS 样式 |
| `get_fill_image` | 获取图片填充数据 |
| `get_font_family_info` | 查询字体可用性 |
| `get_guide` | 获取引导式工作流 |
| `find_placement` | 查找画板放置位置 |

### 写入工具 (Write)

| 工具 | 功能 |
|------|------|
| `create_artboard` | 创建新画板 |
| `write_html` | 解析 HTML 并添加/替换节点 |
| `set_text_content` | 设置文本节点内容 |
| `rename_nodes` | 重命名图层 |
| `duplicate_nodes` | 深度克隆节点 |
| `update_styles` | 更新 CSS 样式 |
| `delete_nodes` | 删除节点及其后代 |
| `start_working_on_nodes` | 标记正在工作的画板 |
| `finish_working_on_nodes` | 清除工作指示器 |

## 使用场景

### 1. 从设计稿生成网站

**场景**: 你有一个 Landing Page 设计，想快速生成 React + Tailwind 网站。

**步骤**:
1. 在 Paper 中打开设计文件
2. 选中要导出的 Frame
3. 在 AI 助手中输入：
   ```
   使用 React 和 Tailwind，根据我在 Paper 中选中的 hero section 构建网站
   ```
4. AI 会自动读取设计并生成代码

### 2. 同步 Figma 设计 Token

**场景**: 你在 Figma 中有设计系统，想在 Paper 中使用。

**步骤**:
1. 安装 Figma MCP 服务器
2. 在 Figma 中选中包含设计 Token 的元素
3. 在 AI 助手中输入：
   ```
   从 Figma 导入设计 Token 到 Paper
   ```
4. AI 会自动同步颜色、字体等样式

### 3. 使用真实内容替代占位符

**场景**: 设计稿使用 Lorem Ipsum，想替换为真实内容。

**步骤**:
1. 安装 Notion MCP 服务器
2. 在 Notion 中准备真实内容数据库
3. 在 Paper 中选中要替换的区域
4. 在 AI 助手中输入：
   ```
   从 Notion Testimonials 数据库同步内容到 Paper
   ```
5. AI 会自动拉取真实内容并更新设计

### 4. 添加响应式布局

**场景**: 有桌面端设计，想自动生成移动端版本。

**步骤**:
1. 在 Paper 中准备不同断点的设计
2. 在 AI 助手中输入：
   ```
   根据我在 Paper 中选中的不同断点 Frame，为网站添加响应式布局
   ```
3. AI 会自动生成响应式代码

## 最佳实践

### 设计结构优化

为了让 AI 更好地理解设计：
- ✅ 使用 Flex 布局和容器
- ✅ 语义化命名图层
- ✅ 保持设计结构清晰
- ❌ 避免过度嵌套
- ❌ 避免绝对定位滥用

### 版本控制

使用 Git 跟踪代码变更：
```
帮我设置 Git 并提交当前更改
```

### 迭代式开发

从小组件开始，逐步构建：
```
先构建导航栏组件
```

## 常见问题

### 连接失败

**症状**: AI 助手无法连接 Paper MCP

**解决方案**:
1. 确保 Paper Desktop 已启动
2. 确保文件已在 Paper 中打开
3. 重启 AI 工具
4. 检查 MCP 配置是否正确

### 权限问题

**症状**: AI 请求权限时被拒绝

**解决方案**:
- 对于只读工具，可以授予 "Always Allow"
- 对于写入工具，谨慎授予权限

### Windows WSL 问题

**症状**: WSL 中无法访问 MCP 服务器

**解决方案**:
1. 打开 WSL 设置
2. 导航到 Networking
3. 设置 `networkingMode` 为 `mirrored`
4. 重启 WSL

## 示例 Prompt

### 创建设计元素

```
在 Paper 中创建一个包含标题、描述和按钮的 hero section
```

### 导出代码

```
将我在 Paper 中选中的卡片组件导出为 React + Tailwind 代码
```

### 同步设计系统

```
从 Figma 同步颜色变量和字体样式到 Paper
```

### 使用真实数据

```
从 Notion 产品数据库同步内容到 Paper 产品展示区域
```

## 相关链接

- **官网**: https://paper.design
- **文档**: https://paper.design/docs/mcp
- **下载**: https://paper.design/downloads
- **构建日志**: https://paper.design/build-log

---

*Paper: design, share, ship - 连接设计、代码和 AI 的统一画布*
