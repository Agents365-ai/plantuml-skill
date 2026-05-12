# plantuml-skill — From Text to Professional UML Diagrams

[中文文档](README_CN.md)

A skill that turns natural-language descriptions into `.puml` PlantUML source and exports diagrams to PNG / SVG via the Kroki rendering API — no Java, no Graphviz, no local install (just `curl`). Local Kroki Docker and `plantuml.jar` fallbacks are available for offline / air-gapped use.

Works with Claude Code, Cursor, Copilot, OpenClaw, Codex, Hermes, and any agent that supports the [Agent Skills](https://agentskills.io) format.

Supports 10+ diagram types — sequence, component, class, ER, activity, use case, state, C4, mind map, gantt — each with idiomatic syntax templates, themes (`plain` / `cerulean` / `blueprint` / `aws-orange` / `vibrant`), and a curated "Common Mistakes" guide.

## Documentation

| Doc | What's inside |
|---|---|
| [docs/features.md](docs/features.md) | Comparison vs. native / other PlantUML tooling, key advantages, supported diagram types |
| [docs/setup.md](docs/setup.md) | Rendering backends (Kroki API / local Docker / jar) and known limitations |
| [skills/plantuml-skill/SKILL.md](skills/plantuml-skill/SKILL.md) | Workflow guide loaded by the agent |

## Quick Start

Install the skill:

```bash
# Any agent (Claude Code, Cursor, Copilot, etc.)
npx skills add Agents365-ai/365-skills -g

# Claude Code only
> /plugin marketplace add Agents365-ai/365-skills
> /plugin install plantuml
```

Manual install — clone into your agent's skills directory:

```bash
git clone https://github.com/Agents365-ai/plantuml-skill.git ~/.claude/skills/plantuml-skill
```

Common paths: `~/.claude/skills/` (Claude Code), `~/.config/opencode/skills/` (Opencode), `~/.openclaw/skills/` (OpenClaw), `~/.agents/skills/` (Codex). Also indexed on [SkillsMP](https://skillsmp.com).

Default rendering uses the public Kroki API — `curl` is the only requirement. For offline or air-gapped use see [docs/setup.md](docs/setup.md).

## Usage

Just describe what you want:

```
Create a sequence diagram showing the OAuth 2.0 authorization code flow with
Client, Authorization Server, Resource Server, and User
```

The agent will generate the `.puml` file and export it to PNG via Kroki automatically.

## Example

**Prompt:** *Create a microservices e-commerce architecture with Mobile/Web/Admin clients, API Gateway, User/Order/Product/Payment services, Kafka event bus, Notification service, and User DB / Order DB / Product DB / Redis Cache / Stripe API*

![Microservices Architecture](assets/example.png)

## Support

If this skill helps you, consider supporting the author:

<table>
  <tr>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/qrcode/wechat-pay.png" width="180" alt="WeChat Pay">
      <br>
      <b>WeChat Pay</b>
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/qrcode/alipay.png" width="180" alt="Alipay">
      <br>
      <b>Alipay</b>
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/qrcode/buymeacoffee.png" width="180" alt="Buy Me a Coffee">
      <br>
      <b>Buy Me a Coffee</b>
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/awarding/award.gif" width="180" alt="Give a Reward">
      <br>
      <b>Give a Reward</b>
    </td>
  </tr>
</table>

## Author

**Agents365-ai**

- Bilibili: https://space.bilibili.com/441831884
- GitHub: https://github.com/Agents365-ai

## License

MIT
