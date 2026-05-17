# plantuml-skill — From Text to Professional UML Diagrams

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/Agents365-ai/plantuml-skill?style=flat&logo=github)](https://github.com/Agents365-ai/plantuml-skill/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/Agents365-ai/plantuml-skill?style=flat&logo=github)](https://github.com/Agents365-ai/plantuml-skill/network/members)
[![Latest Release](https://img.shields.io/github/v/release/Agents365-ai/plantuml-skill?logo=github)](https://github.com/Agents365-ai/plantuml-skill/releases/latest)
[![Last Commit](https://img.shields.io/github/last-commit/Agents365-ai/plantuml-skill?logo=github)](https://github.com/Agents365-ai/plantuml-skill/commits/main)

[![SkillsMP](https://img.shields.io/badge/SkillsMP-listed-1f6feb)](https://skillsmp.com/skills/agents365-ai-plantuml-skill-skills-plantuml-skill-skill-md)
[![ClawHub](https://img.shields.io/badge/ClawHub-listed-ff6b35)](https://clawhub.ai/agents365-ai/plantuml-pro-skill)
[![Claude Code Plugin](https://img.shields.io/badge/Claude%20Code-plugin-8a2be2)](https://github.com/Agents365-ai/365-skills)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-compatible-2ea44f)](https://agentskills.io)
[![Discord](https://img.shields.io/badge/Discord-Join-5865F2?logo=discord&logoColor=white)](https://discord.gg/79JF5Atuk)

**English** · [中文](README_CN.md) · [📖 Online Docs](https://agents365-ai.github.io/plantuml-skill/)

A skill that turns natural-language descriptions into `.puml` PlantUML source and exports diagrams to PNG / SVG via the [Kroki](https://kroki.io) rendering API — no Java, no Graphviz, no local install (just `curl`). Local Kroki Docker and `plantuml.jar` fallbacks are available for offline / air-gapped use. Works with **Claude Code, Cursor, Copilot, OpenClaw, Codex, Hermes**, and any agent compatible with the [Agent Skills](https://agentskills.io) format.

<p align="center">
  <img src="assets/example.png" width="900" alt="Microservices architecture rendered from a natural-language prompt via PlantUML + Kroki">
</p>

## ✨ Highlights

- **10+ diagram types** — sequence, component, class, ER, activity, use case, state, C4, mind map, gantt — each with idiomatic syntax templates and shape vocabulary
- **Zero-install default** — public Kroki API needs only `curl`; no Node, no Java, no Graphviz
- **3 rendering backends** — public Kroki, local Kroki via Docker (offline), or `plantuml.jar` + Java + Graphviz (air-gapped)
- **5 built-in themes** — `plain`, `cerulean`, `blueprint`, `aws-orange`, `vibrant` — plus full `skinparam` overrides
- **C4 that actually works** — uses Kroki's `c4plantuml` endpoint to sidestep the public PlantUML server's `!include` 404 trap
- **Common Mistakes guide** — 9-row curated table covering arrow direction, layout overflow, label escaping, participant ordering, and the C4 include pitfall

## 🖼️ Examples

> [!TIP]
> **The hero image above was generated from this single prompt:**

```
Create a microservices e-commerce architecture with Mobile/Web/Admin clients,
API Gateway, User/Order/Product/Payment services, Kafka event bus,
Notification service, and User DB / Order DB / Product DB / Redis Cache /
Stripe API
```

Source `.puml` and rendered PNG live in [`assets/`](assets/) — the skill produced both in one shot.

## 🚀 Installation

### 1. Pick a rendering backend

| Option | Install | When to use |
|---|---|---|
| **Kroki API** (default) | `curl` (pre-installed everywhere) | Online — zero setup |
| **Local Kroki** | `docker run -d -p 8000:8000 yuzutech/kroki` | Offline / privacy / heavy workloads |
| **`plantuml.jar`** | `brew install graphviz openjdk` + [download jar](https://plantuml.com/download) | Fully air-gapped |

Full recipes in [docs/setup.md](docs/setup.md).

### 2. Install the skill

```bash
# Any agent (Claude Code, Cursor, Copilot, ...)
npx skills add Agents365-ai/365-skills -g
```

```text
# Claude Code plugin marketplace
> /plugin marketplace add Agents365-ai/365-skills
> /plugin install plantuml
```

```bash
# Manual install
git clone https://github.com/Agents365-ai/plantuml-skill.git \
  ~/.claude/skills/plantuml-skill
```

Common paths: `~/.claude/skills/` (Claude Code), `~/.config/opencode/skills/` (Opencode), `~/.openclaw/skills/` (OpenClaw), `~/.agents/skills/` (Codex). Also indexed on [SkillsMP](https://skillsmp.com/skills/agents365-ai-plantuml-skill-skills-plantuml-skill-skill-md) and [ClawHub](https://clawhub.ai/agents365-ai/plantuml-pro-skill).

**Updating:** `/plugin update plantuml` (Claude Code), `skills update plantuml-skill` (SkillsMP), `clawhub update plantuml-pro-skill` (OpenClaw), or `git pull` for manual installs.

## ⚡ Quick Start

After installation, just describe what you want:

```
Create a sequence diagram showing the OAuth 2.0 authorization code flow with
Client, Authorization Server, Resource Server, and User. Include the redirect,
token exchange, and resource access steps with proper activation boxes.
```

The skill picks the right diagram type, generates the `.puml` source, and exports to PNG/SVG via Kroki.

## 🧩 Supported Diagram Types

| Category | Examples | Notable features |
|---|---|---|
| Sequence | API calls, OAuth flows, protocol traces | Lifelines, activation boxes, async arrows |
| Component / Architecture | services, modules, queues, databases, clouds | `package`/`rectangle` grouping, shape vocabulary |
| Class | OOP models, data structures | Inheritance, composition, aggregation, multiplicities (`"1" --> "*"`) |
| ER / Entity | database schemas | `<<PK>>` / `<<FK>>` notation, crow's-foot relationships |
| Activity / Flowchart | workflows, business processes | `if/then/else/endif` decision branches |
| Use Case | system requirements, user stories | Actors, system boundaries |
| State | state machines, lifecycle flows | `[*] -->` start/end markers |
| C4 | Context, Container, Component | Via Kroki's `c4plantuml` endpoint (no broken includes) |
| Other | mind maps, gantt | `@startmindmap`, `@startgantt` |

## 🔄 How it works

Behind the scenes: **check `curl`** → **pick diagram type** → **generate `.puml` source** with `@startuml`/`@enduml` markers → **POST to Kroki** (`https://kroki.io/plantuml/png` or `…/svg`) → **save output + report paths to the user**. Swap the endpoint for `http://localhost:8000` to use a local Kroki container, or run `java -jar plantuml.jar` for air-gapped renders.

## 🆚 Comparison

### vs Native Agent (no skill)

| Feature | Native agent | plantuml-skill |
|---|---|---|
| Generate PlantUML source | ✅ (LLMs know the syntax) | ✅ |
| Export to PNG/SVG | ❌ outputs text only | ✅ one `curl` POST to Kroki |
| Renderer choice | none | ✅ public Kroki / local Kroki / `plantuml.jar` |
| Diagram-type catalog | implicit | ✅ 10+ types with shape & arrow vocabulary |
| Theme defaults | random per run | ✅ 5 named themes + `skinparam` overrides |
| C4 via cloud | ❌ often broken (`!include` 404s) | ✅ Kroki `c4plantuml` endpoint |
| Common-mistake guard rails | ❌ | ✅ 9-row curated pitfalls table |
| Offline / air-gapped | ❌ | ✅ local Kroki Docker or local jar |

Full comparison + key-advantages summary in [docs/features.md](docs/features.md).

## 🔗 Related Skills

Part of the [Agents365-ai diagram-skill family](https://github.com/Agents365-ai) — pick the right tool for the job:

| Skill | Style | Best for |
|---|---|---|
| [drawio-skill](https://github.com/Agents365-ai/drawio-skill) | Professional / vector | Architecture, ML/DL, ER diagrams with self-check loop |
| [excalidraw-skill](https://github.com/Agents365-ai/excalidraw-skill) | Hand-drawn / sketchy | Whiteboard mockups, informal diagrams |
| [mermaid-skill](https://github.com/Agents365-ai/mermaid-skill) | Text-based, auto-layout | README-embeddable, version-control friendly |
| [tldraw-skill](https://github.com/Agents365-ai/tldraw-skill) | Whiteboard collaboration | Casual sketches, FigJam-style boards |

## 💬 Community

- **Discord:** https://discord.gg/79JF5Atuk
- **WeChat:** scan the QR code below

<p align="center">
  <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/qrcode/agents365ai_wechat_1.png" width="200" alt="WeChat Community Group">
</p>

## ❤️ Support

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

## 👤 Author

**Agents365-ai**

- GitHub: https://github.com/Agents365-ai
- Bilibili: https://space.bilibili.com/441831884

## 📄 License

[MIT](LICENSE)
