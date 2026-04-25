# plantuml-skill — 文本到专业 UML 图表

[English](README.md)

## 功能说明

- 根据自然语言描述生成 `.puml` PlantUML 源文件
- 通过 **Kroki 渲染 API** 导出为 **PNG 或 SVG** — 无需 Java、无需 Graphviz、无需本地安装（仅依赖 `curl`）
- **10+ 种图表类型**：时序图、组件图、类图、ER 图、活动图、用例图、状态图、C4、思维导图、Gantt — 每种都附带地道的语法模板
- **内置主题**：`plain`、`cerulean`、`blueprint`、`aws-orange`、`vibrant` — 同时支持完整的 `skinparam` 自定义样式
- **三种渲染方案** — 公共 Kroki API（零安装）、本地 Kroki Docker（离线使用）、传统 PlantUML jar + Java
- **C4 图支持**：通过 Kroki 的 `c4plantuml` 端点（已内联解析 PlantUML 公服的 `!include` 指令）
- 当解释 API、类层次、状态机或 3+ 组件的系统时自动触发画图

## 多平台支持

兼容所有主流支持 [Agent Skills](https://agentskills.io) 格式的 AI 编码智能体：

| 平台 | 支持状态 | 说明 |
|------|----------|------|
| **Claude Code** | ✅ 完全支持 | 原生 SKILL.md 格式 |
| **Opencode** | ✅ 完全支持 | 通过原生 `skill` 工具加载 SKILL.md，同时兼容 `.claude/skills/` 路径 |
| **OpenClaw / ClawHub** | ✅ 完全支持 | `metadata.openclaw` 命名空间 + `curl` 依赖检测 |
| **Hermes Agent** | ✅ 完全支持 | `metadata.hermes` 命名空间，标签分类，工具门控 |
| **OpenAI Codex** | ✅ 完全支持 | 标准 SKILL.md 直接读取 |
| **SkillsMP** | ✅ 可索引 | GitHub topics 已配置 |

## 对比

### 与无 skill 的原生智能体对比

| 功能 | 原生智能体 | 本 skill |
|------|-----------|---------|
| 生成 PlantUML 源码 | 是 — LLM 本身了解语法 | 是 |
| 导出为 PNG/SVG | 否 — 仅输出文本 | 是 — 一次 `curl` POST 到 Kroki |
| 渲染器选择 | 无 | 公共 Kroki / 本地 Kroki / `plantuml.jar` |
| 主动触发 | 否 — 仅在明确要求时 | 是 — 3+ 组件、API、类层次、状态机时自动建议 |
| 图表类型目录 | 隐式 | 10+ 种，附形状词汇、箭头词汇、多重性标记 |
| 主题默认值 | 每次随机 | `!theme plain` 基线 + 精选低饱和度配色 |
| 云端 C4 图 | 经常失效（`!include` 404） | 使用 Kroki 的 `c4plantuml` 端点（已预合并 includes） |
| 常见错误清单 | 无 | 9 类已知坑（箭头方向、布局溢出、标签转义、参与者顺序等） |
| 离线支持 | 否 | 是 — 本地 Kroki Docker 或本地 jar |

### 与其他 PlantUML 方案对比

| 方案 | 安装体积 | 离线 | 智能体感知 | 备注 |
|------|---------|------|-----------|------|
| **本 skill（Kroki API）** | 仅 `curl` | 可选（Docker） | ✅ | 默认方案，零安装 |
| **本 skill（本地 Kroki）** | Docker | ✅ | ✅ | 单容器，无需 Java |
| **本 skill（`plantuml.jar`）** | Java + Graphviz + jar | ✅ | ✅ | 体积最大但默认离线 |
| 公共 PlantUML 网站 | 仅浏览器 | ❌ | 手动 | 智能体无法脚本化调用 |
| `node-plantuml` / npm 包装 | Node + Java | ✅ | ❌ | 仍是同一个 jar 的封装 |
| VS Code PlantUML 扩展 | 编辑器 + Java | ✅ | ❌ | 编写时使用，不适合智能体 |

### 核心优势

1. **零安装默认值** — `curl` 无处不在；macOS、Linux、Windows（Git Bash / WSL）开箱即用
2. **三种渲染路径** — 公共 Kroki 求快、本地 Kroki 求离线/隐私、jar 包应对气隙环境
3. **图表类型剧本** — 10+ 种图表都有可复制的语法模板、形状词汇、箭头规范
4. **常见错误规避** — SKILL.md 内置精选"Common Mistakes"表，覆盖箭头方向、布局溢出、标签转义、参与者顺序、C4 includes 等坑
5. **多智能体单文件** — 一个 `SKILL.md` 跑通 Claude Code、Opencode、OpenClaw、Hermes、Codex、SkillsMP

## 支持的图表类型

- **时序图**：API 调用、OAuth 流程、协议追踪 — 含生命线、激活框、异步箭头
- **组件 / 架构图**：服务、模块、队列、数据库、云 — 含 package/rectangle 分组
- **类图**：OOP 模型 — 继承、组合、聚合、多重性（`"1" --> "*"`）
- **ER / 实体图**：数据库 schema — `<<PK>>` / `<<FK>>` 标记，crow's-foot 关系
- **活动图**：工作流、业务流程、决策分支 — `if/then/else/endif` 语法
- **用例图**：参与者、系统边界、用户故事
- **状态图**：状态机、生命周期 — `[*] -->` 起止标记
- **C4**：Context、Container、Component 图 — 通过 Kroki `c4plantuml` 端点
- **其他**：思维导图（`@startmindmap`）、Gantt（`@startgantt`）

## 前置依赖

### 方式 A — Kroki API（推荐，零安装）

```bash
# curl 已在 macOS、Linux、Windows（Git Bash / WSL）上预装
curl --version
```

仅此而已。图表通过 `https://kroki.io` 渲染。

### 方式 B — 本地 Kroki（Docker，离线使用）

```bash
docker run -d -p 8000:8000 yuzutech/kroki

# 然后将 skill 指向本地端点
curl -s -X POST http://localhost:8000/plantuml/png \
  -H "Content-Type: text/plain" \
  --data-binary "@diagram.puml" \
  -o diagram.png
```

### 方式 C — 本地 PlantUML jar（气隙环境）

```bash
# macOS
brew install graphviz openjdk
# Ubuntu/Debian
sudo apt install graphviz default-jre

# 从 https://plantuml.com/download 下载 plantuml.jar
java -jar plantuml.jar diagram.puml
# → diagram.png 会写入源文件同目录
```

| 平台 | 备注 |
|------|------|
| **macOS** | `curl` 系统自带；方式 C 用 Homebrew 安装 `graphviz` / `openjdk` |
| **Windows** | 用 Git Bash 或 WSL 以获得 `curl`；方式 C 需自行安装 Java 与 Graphviz |
| **Linux** | 大多发行版自带 `curl`；用 `apt` / `yum` / `pacman` 安装 `graphviz` 与 `default-jre` |

## Skill 安装

### Claude Code

```bash
# 全局安装（所有项目可用）
git clone https://github.com/Agents365-ai/plantuml-skill.git ~/.claude/skills/plantuml-skill

# 项目级安装
git clone https://github.com/Agents365-ai/plantuml-skill.git .claude/skills/plantuml-skill
```

### Opencode

```bash
# 全局安装（Opencode 原生路径）
git clone https://github.com/Agents365-ai/plantuml-skill.git ~/.config/opencode/skills/plantuml-skill

# 项目级安装
git clone https://github.com/Agents365-ai/plantuml-skill.git .opencode/skills/plantuml-skill
```

Opencode 同时会读取 `~/.claude/skills/` 和 `.claude/skills/`，所以已有的 Claude Code 安装会被自动识别，无需重复 clone。

### OpenClaw

```bash
# 手动安装
git clone https://github.com/Agents365-ai/plantuml-skill.git ~/.openclaw/skills/plantuml-skill

# 项目级安装
git clone https://github.com/Agents365-ai/plantuml-skill.git skills/plantuml-skill
```

### Hermes Agent

```bash
# 安装到 design 分类
git clone https://github.com/Agents365-ai/plantuml-skill.git ~/.hermes/skills/design/plantuml-skill
```

或在 `~/.hermes/config.yaml` 中添加外部目录：

```yaml
skills:
  external_dirs:
    - ~/myskills/plantuml-skill
```

### OpenAI Codex

```bash
# 用户级安装
git clone https://github.com/Agents365-ai/plantuml-skill.git ~/.agents/skills/plantuml-skill

# 项目级安装
git clone https://github.com/Agents365-ai/plantuml-skill.git .agents/skills/plantuml-skill
```

### SkillsMP

在 [SkillsMP](https://skillsmp.com) 浏览或使用 CLI：

```bash
skills install plantuml-skill
```

### 安装路径总结

| 平台 | 全局路径 | 项目路径 |
|------|----------|----------|
| Claude Code | `~/.claude/skills/plantuml-skill/` | `.claude/skills/plantuml-skill/` |
| Opencode | `~/.config/opencode/skills/plantuml-skill/`（同时读取 `~/.claude/skills/`） | `.opencode/skills/plantuml-skill/`（同时读取 `.claude/skills/`） |
| OpenClaw / ClawHub | `~/.openclaw/skills/plantuml-skill/` | `skills/plantuml-skill/` |
| Hermes Agent | `~/.hermes/skills/design/plantuml-skill/` | 通过 `external_dirs` 配置 |
| OpenAI Codex | `~/.agents/skills/plantuml-skill/` | `.agents/skills/plantuml-skill/` |
| SkillsMP | N/A（通过 CLI 安装） | N/A |

## 更新

拉取最新版本：

```bash
cd <你的安装路径>/plantuml-skill && git pull
```

通过包管理器安装的用户直接用对应命令更新：

```bash
# SkillsMP
skills update plantuml-skill
```

## 使用方式

直接描述你想要的图表：

```
画一个 OAuth 2.0 授权码流程的时序图，包含 Client、Authorization Server、Resource Server 和 User
```

智能体会自动生成 `.puml` 文件并通过 Kroki 导出为 PNG。

## 示例

**提示词：**
> 画一个微服务电商架构图，包含 Mobile/Web/Admin 客户端，API Gateway，
> User/Order/Product/Payment 微服务，Kafka 事件总线，Notification 服务，
> User DB / Order DB / Product DB / Redis Cache / Stripe API

**输出效果：**

![微服务架构图](assets/example.png)

## 文件说明

- `SKILL.md` — **唯一必需的文件**，所有平台加载的 skill 指令
- `README.md` — 英文说明（GitHub 首页显示）
- `README_CN.md` — 本文件（中文）
- `assets/` — 示例 `.puml` 源文件与渲染 PNG

> **提示：** 仅 `SKILL.md` 是 skill 运行所必需的。`assets/` 和 README 文件仅为文档用途，可以安全删除以节省空间。

## 已知限制

- **公共 Kroki 限流**：`kroki.io` 是共享尽力而为服务；高负载场景请自建 Kroki 容器（方式 B）
- **默认需要联网**：方式 A 需要访问 `kroki.io`。离线 / 气隙环境请使用方式 B（本地 Docker）或方式 C（jar）
- **C4 `!include` 指令**：PlantUML 公共网站会在渲染时解析 `!include` URL，Kroki 可能不支持。请改用 Kroki 的专用 `c4plantuml` 端点（`https://kroki.io/c4plantuml/png`）
- **部分图表需要 Graphviz**：活动图、类图内部依赖 Graphviz — 方式 C 必须连同 Java 一起安装 `graphviz`
- **无视觉自检**：与 `drawio-skill` 不同，本 skill 不会回读渲染后的 PNG 自动修复布局问题

## 开源协议

MIT

## 支持作者

如果这个 skill 对你有帮助，欢迎支持作者：

<table>
  <tr>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/qrcode/wechat-pay.png" width="180" alt="微信支付">
      <br>
      <b>微信支付</b>
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/qrcode/alipay.png" width="180" alt="支付宝">
      <br>
      <b>支付宝</b>
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/qrcode/buymeacoffee.png" width="180" alt="Buy Me a Coffee">
      <br>
      <b>Buy Me a Coffee</b>
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/awarding/award.gif" width="180" alt="打赏">
      <br>
      <b>打赏</b>
    </td>
  </tr>
</table>

## 作者

**Agents365-ai**

- Bilibili: https://space.bilibili.com/441831884
- GitHub: https://github.com/Agents365-ai
