# plantuml-skill — 文本到专业 UML 图表

[English](README.md) | [在线文档](https://agents365-ai.github.io/plantuml-skill/zh.html)

一个把自然语言描述变成 `.puml` PlantUML 源文件,并通过 Kroki 渲染 API 导出为 PNG / SVG 的技能 —— 无需 Java、无需 Graphviz、无需本地安装(只要 `curl`)。离线 / 气隙场景可使用本地 Kroki Docker 或 `plantuml.jar`。

支持 Claude Code、Cursor、Copilot、OpenClaw、Codex、Hermes 等任何兼容 [Agent Skills](https://agentskills.io) 规范的 agent。

支持 10+ 种图表类型 —— 时序图、组件图、类图、ER 图、活动图、用例图、状态图、C4、思维导图、Gantt —— 各自附带地道语法模板、主题(`plain` / `cerulean` / `blueprint` / `aws-orange` / `vibrant`),以及精选"常见错误"指南。

## 文档导航

| 文档 | 内容 |
|---|---|
| [docs/features_CN.md](docs/features_CN.md) | 与原生 / 其他 PlantUML 方案的对比、核心优势、支持的图表类型 |
| [docs/setup_CN.md](docs/setup_CN.md) | 三种渲染后端(Kroki API / 本地 Docker / jar)与已知限制 |
| [skills/plantuml-skill/SKILL.md](skills/plantuml-skill/SKILL.md) | agent 加载的工作流指南 |

## 快速开始

安装技能:

```bash
# 任意 Agent(Claude Code、Cursor、Copilot 等)
npx skills add Agents365-ai/365-skills -g

# 仅 Claude Code
> /plugin marketplace add Agents365-ai/365-skills
> /plugin install plantuml
```

手动安装 —— 克隆到你的 agent skills 目录:

```bash
git clone https://github.com/Agents365-ai/plantuml-skill.git ~/.claude/skills/plantuml-skill
```

常用路径:`~/.claude/skills/`(Claude Code)、`~/.config/opencode/skills/`(Opencode)、`~/.openclaw/skills/`(OpenClaw)、`~/.agents/skills/`(Codex)。同时已索引于 [SkillsMP](https://skillsmp.com)。

默认通过公共 Kroki API 渲染,只需要 `curl`。离线 / 气隙场景请参考 [docs/setup_CN.md](docs/setup_CN.md)。

## 使用方式

直接描述你想要的图表:

```
画一个 OAuth 2.0 授权码流程的时序图,包含 Client、Authorization Server、Resource Server 和 User
```

智能体会自动生成 `.puml` 文件并通过 Kroki 导出为 PNG。

## 示例

**提示词:** *画一个微服务电商架构图,包含 Mobile/Web/Admin 客户端,API Gateway,User/Order/Product/Payment 微服务,Kafka 事件总线,Notification 服务,User DB / Order DB / Product DB / Redis Cache / Stripe API*

![微服务架构图](assets/example.png)

## 支持作者

如果这个 skill 对你有帮助,欢迎支持作者:

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

## License

MIT
