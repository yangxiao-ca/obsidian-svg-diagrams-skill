# Obsidian SVG Diagrams Skill

为 Obsidian 笔记生成出版级质量的 SVG 图表。包含布局计算、边界校验、视觉对齐、配色系统和验证工具。

## 功能特性

- **自动布局计算**：所有坐标先算后画，杜绝溢出
- **边界校验**：安全区规则（顶40px/底20px/左40px/右40px）
- **视觉对齐**：同级元素大小均等、容器内边距均匀、文字严格居中
- **配色系统**：9 个色系 × 7 个层级，兼容深色/浅色主题
- **验证工具**：Python 脚本自动检查 XML 合法性和边界溢出
- **布局引擎**：可选的 Python 布局引擎，程序化生成 SVG

## 安装方式

### 方式 1：直接克隆到 skills 目录

```bash
# 用户级 skill（所有项目可用）
git clone https://github.com/yangxiao-ca/obsidian-svg-diagrams-skill.git \
  ~/.workbuddy/skills/obsidian-svg-diagrams

# 或项目级 skill（仅当前项目）
git clone https://github.com/yangxiao-ca/obsidian-svg-diagrams-skill.git \
  ./workbuddy/skills/obsidian-svg-diagrams
```

### 方式 2：软链接（多 Agent 共享）

```bash
# 克隆到源目录
git clone https://github.com/yangxiao-ca/obsidian-svg-diagrams-skill.git \
  ~/.agents/skills/obsidian-svg-diagrams

# 软链接到各个 Agent
ln -s ~/.agents/skills/obsidian-svg-diagrams ~/.workbuddy/skills/obsidian-svg-diagrams
ln -s ~/.agents/skills/obsidian-svg-diagrams ~/.claude/skills/obsidian-svg-diagrams
ln -s ~/.agents/skills/obsidian-svg-diagrams ~/.codex/skills/obsidian-svg-diagrams
```

更新时只需在源目录 `git pull`，所有 Agent 自动同步。

## 触发词

当你在对话中提到以下关键词时，Agent 会自动加载这个 skill：

### 核心触发词

- **给我画SVG图**
- **画SVG**
- **画个SVG图**
- **生成SVG**
- **SVG图**

### 图表类型触发词

- 画流程图
- 画结构图
- 画时间线
- 画对照图
- 画层级图
- 画架构图
- 画对比图
- 画决策图
- 画因果图
- 画排除图
- 画公式图
- 画示意图
- 画逻辑图
- 画关系图
- 画思维导图
- 画时间轴
- 画决策树
- 画因果链
- 画排除法
- 画公式链

### 使用示例

```
用户：帮我把这个流程画成SVG图
用户：给我画SVG图，展示这个四层结构
用户：画个时间线，从2011年到2025年
用户：把这个架构画成SVG，存到我的Obsidian库
```

## 适用场景

- 为 Obsidian 笔记生成图表
- 交付 Markdown 文件时自动配图
- 需要出版级质量的 SVG 图表
- 需要严格布局计算和边界校验
- 需要兼容深色/浅色主题

## 文件结构

```
obsidian-svg-diagrams/
├── SKILL.md                    # 主技能文件（完整规范）
├── README.md                   # 本文件
└── references/
    ├── COLOR_PALETTE.md        # 配色系统参考
    └── LAYOUT_ENGINE.md        # Python 布局引擎 API
```

## 核心规则

### 1. 硬编码颜色

Obsidian 环境没有 CSS 变量，所有颜色必须硬编码为十六进制值。

### 2. 自包含样式

- 禁用 `<style>` 块
- 禁用 HTML 注释
- 禁用渐变、阴影、模糊
- 设计为「浅底填充 + 深色文字」，兼容深色/浅色主题

### 3. 布局计算

- 所有坐标先算后画
- 容器高度 = 子元素总高 + 子元素间距 + 上下 padding × 2
- 同级元素大小均等
- 容器内边距均匀（通常 20px）
- 文字严格居中

### 4. 边界校验

生成后必须运行 `svg_boundary_check.py` 验证，确保 100% 通过。

## 验证工具

```bash
# 检查所有 SVG
python3 _scripts/svg_boundary_check.py

# 检查单个文件
python3 _scripts/svg_boundary_check.py path/to/file.svg

# 自动修复 viewBox 高度
python3 _scripts/svg_boundary_check.py --fix path/to/file.svg
```

## 配色系统

9 个色系 × 7 个层级：

| 色系 | 用途 |
|------|------|
| blue | 主要信息、技术架构 |
| teal | 成功、完成、健康状态 |
| gray | 中性信息、辅助说明 |
| amber | 警告、注意、待处理 |
| red | 错误、危险、失败 |
| coral | 强调、重要提醒 |
| green | 正向、增长、收益 |
| pink | 女性、创意、设计 |
| purple | 高端、专业、权威 |

每个色系 7 个层级：50（最浅）→ 900（最深）

## 仓库地址

https://github.com/yangxiao-ca/obsidian-svg-diagrams-skill

## 许可证

MIT
