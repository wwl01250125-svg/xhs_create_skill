---
name: xhs-note-creator
description: 小红书笔记素材创作技能。当用户需要创建小红书笔记素材时使用这个技能。技能包含：根据用户的需求和提供的资料，撰写小红书笔记内容（标题+正文），生成图片卡片（封面+正文卡片），以及发布小红书笔记。
---

# 小红书笔记创作技能

这个技能用于创建专业的小红书笔记素材，包括内容撰写、图片卡片生成和笔记发布。

## 最新更新 ⚡

- **正文内容自动分割**：支持将Markdown正文内容自动分割成3-5张精美的图片卡片
- **优化的发布流程**：支持一键发布封面+多张正文卡片到小红书
- **灵活的配置**：可自定义生成的正文卡片数量

## 使用场景

- 用户需要创建小红书笔记时
- 用户提供资料需要转化为小红书风格内容时
- 用户需要生成精美的图片卡片用于发布时
- 需要将长文本内容分割成多张图片发布时

## 工作流程

### 第一步：撰写小红书笔记内容

根据用户需求和提供的资料，创作符合小红书风格的内容：

#### 标题要求
- 不超过 20 字
- 吸引眼球，制造好奇心
- 可使用数字、疑问句、感叹号增强吸引力
- 示例：「5个让效率翻倍的神器推荐！」「震惊！原来这样做才对」

#### 正文要求
- 使用良好的排版，段落清晰
- 点缀少量 Emoji 增加可读性（每段 1-2 个即可）
- 使用简短的句子和段落
- 结尾给出 SEO 友好的 Tags 标签（5-10 个相关标签）

### 第二步：生成 Markdown 文档

**注意：这里生成的 Markdown 文档是用于渲染卡片的，必须专门生成，禁止直接使用上一步的笔记正文内容。**

Markdown 文件，文件应包含：

1. YAML 头部元数据（封面信息）：
```yaml
---
emoji: "🚀"           # 封面装饰 Emoji
title: "大标题"        # 封面大标题（不超过15字）
subtitle: "副标题文案"  # 封面副标题（不超过15字）
---
```

2. 用于渲染卡片的 Markdown 文本内容：
   - 内容会自动分割成 3-5 张图片
   - 使用标准的 Markdown 语法（标题、列表、引用、代码块等）
   - 内容长度建议在 500-1500 字之间以获得最佳分割效果

完整 Markdown 文档内容示例：

```markdown
---
emoji: "💡"
title: "5个效率神器让工作效率翻倍"
subtitle: "对着抄作业就好了，一起变高效"
---

# 📝 神器一：Notion

> 全能型笔记工具，支持数据库、看板、日历等多种视图...

## 特色功能

- 特色一
- 特色二

# ⚡ 神器二：Raycast

\`\`\`
可使用代码块来增加渲染后图片的视觉丰富度
\`\`\`

## 推荐原因

- 原因一
- 原因二
- ……


# 🌈 神器三：Arc

全新理念的浏览器，侧边栏标签管理...

...

```

### 第三步：渲染图片卡片

将 Markdown 文档渲染为图片卡片，包括封面图和正文卡片。使用以下脚本渲染：

```bash
python scripts/render_xhs.py <markdown_file> [options]
```

#### 渲染参数（Python）

| 参数 | 简写 | 说明 | 默认值 |
|---|---|---|---|
| `--num-cards` | `-n` | 正文卡片数量（3-5） | `4` |
| `--output-dir` | `-o` | 输出目录 | 当前工作目录 |

#### 生成的文件

- `cover.png` - 封面图片
- `card_1.png`, `card_2.png`, ... - 正文卡片图片

#### 常用示例

```bash
# 1) 默认生成4张正文卡片
python scripts/render_xhs.py content.md

# 2) 自定义生成5张正文卡片
python scripts/render_xhs.py content.md -n 5

# 3) 指定输出目录
python scripts/render_xhs.py content.md -o /path/to/output
```

### 第四步：发布小红书笔记（可选）

使用发布脚本将生成的图片发布到小红书：

```bash
python scripts/publish_xhs.py <markdown_file> [options]
```

#### 发布参数（Python）

| 参数 | 简写 | 说明 |
|---|---|---|
| `--images-dir` | `-i` | 图片目录（默认：当前目录） |
| `--cookie` | `-c` | Cookie文件路径（可选） |

#### 前置条件

1. 需配置小红书 Cookie：
```
XHS_COOKIE=your_cookie_string_here
```

2. Cookie 获取方式：
   - 在浏览器中登录小红书（https://www.xiaohongshu.com）
   - 打开开发者工具（F12）
   - 在 Network 标签中查看请求头的 Cookie

#### 常用示例

```bash
# 1) 直接发布（图片在当前目录）
python scripts/publish_xhs.py content.md

# 2) 指定图片目录
python scripts/publish_xhs.py content.md -i /path/to/images

# 3) 指定Cookie文件
python scripts/publish_xhs.py content.md -c /path/to/.env
```

## 完整工作流示例

```bash
# 1. 先渲染图片（生成封面+4张正文卡片）
python scripts/render_xhs.py my-note.md -o output_dir

# 2. 再发布笔记
python scripts/publish_xhs.py my-note.md -i output_dir
```

## 图片规格说明

### 封面卡片
- 尺寸比例：3:4（小红书推荐比例）
- 基准尺寸：1080×1440px
- 包含：Emoji 装饰、大标题、副标题
- 样式：渐变背景 + 圆角内容区

### 正文卡片
- 尺寸比例：3:4
- 基准尺寸：1080×1440px
- 数量：3-5张（可配置）
- 支持：标题、段落、列表、引用、代码块
- 样式：白色卡片 + 渐变背景边框 + 页码标识

## 技能资源

### 脚本文件
- `scripts/render_xhs.py` - Python 渲染脚本（支持正文分割）
- `scripts/publish_xhs.py` - 小红书发布脚本（支持多张图片）

### 资源文件
- `assets/cover.html` - 封面 HTML 模板
- `assets/card.html` - 正文卡片 HTML 模板
- `assets/styles.css` - 共用样式表

## 注意事项

1. Markdown 文件应保存在工作目录，渲染后的图片也保存在工作目录
2. 技能目录 (`md2Redbook/`) 仅存放脚本和模板，不存放用户数据
3. 图片尺寸会根据内容自动调整，但保持 3:4 比例
4. Cookie 有有效期限制，过期后需要重新获取
5. 发布功能依赖 xhs 库，需要安装：`pip install xhs`
6. 正文内容建议在 500-1500 字之间，以获得最佳的分割效果
7. 支持的正文卡片数量：3-5张
