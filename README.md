# plantuml-skill — From Text to Professional UML Diagrams

[中文文档](README_CN.md)

## What it does

- Generates `.puml` PlantUML source files from natural language descriptions
- Exports diagrams to **PNG or SVG** through the **Kroki rendering API** — no Java, no Graphviz, no local install (just `curl`)
- **10+ diagram types**: sequence, component, class, ER, activity, use case, state, C4, mind map, gantt — each with idiomatic syntax templates
- **Themes built in**: `plain`, `cerulean`, `blueprint`, `aws-orange`, `vibrant` — plus full `skinparam` overrides for custom styling
- **Three rendering modes** — public Kroki API (zero-install), local Kroki via Docker (offline), or traditional PlantUML jar + Java
- **C4 diagram support** via Kroki's `c4plantuml` endpoint (the public PlantUML server's `!include` directives are pre-resolved)
- Triggers automatically when diagrams would help explain APIs, class hierarchies, state machines, or systems with 3+ components

## Comparison

### vs No Skill (native agent)

| Feature | Native agent | This skill |
|---------|--------------|------------|
| Generate PlantUML source | Yes — LLMs know the syntax | Yes |
| Export to PNG/SVG | No — outputs text only | Yes — one `curl` POST to Kroki |
| Renderer choice | None | Public Kroki / local Kroki / `plantuml.jar` |
| Proactive triggers | No — only when explicitly asked | Yes — auto-suggests when 3+ components, APIs, class hierarchies, state machines |
| Diagram-type catalog | Implicit | 10+ types with shape vocabulary, arrow vocabulary, multiplicity notation |
| Theme defaults | Random per run | `!theme plain` baseline + curated palette of pastel colors |
| C4 diagrams via cloud | Often broken (`!include` 404s) | Uses Kroki `c4plantuml` endpoint with stitched includes |
| Common-mistake table | None | 9 known pitfalls (arrow direction, layout overflow, label escaping, ordering, etc.) |
| Offline support | No | Yes — local Kroki Docker container or local jar |

### vs Other PlantUML approaches

| Approach | Install footprint | Offline | LLM-aware | Notes |
|----------|-------------------|---------|-----------|-------|
| **This skill (Kroki API)** | `curl` only | Optional via Docker | ✅ | Default; zero-install |
| **This skill (local Kroki)** | Docker | ✅ | ✅ | One container, no Java needed |
| **This skill (`plantuml.jar`)** | Java + Graphviz + jar | ✅ | ✅ | Heaviest, but offline by default |
| Public PlantUML web server | Browser only | ❌ | Manual | Not scriptable from agents |
| `node-plantuml` / npm wrappers | Node + Java | ✅ | ❌ | Wrappers around the same jar |
| VS Code PlantUML extension | Editor + Java | ✅ | ❌ | Author-time, not agent-time |

### Key advantages

1. **Zero-install default** — `curl` is everywhere; the skill works out of the box on macOS, Linux, and Windows (Git Bash / WSL)
2. **Three rendering paths** — pick public Kroki for speed, local Kroki for offline/privacy, or the jar for air-gapped deployments
3. **Diagram-type playbook** — each of 10+ diagram types has a copy-paste syntax template, shape vocabulary, and arrow conventions
4. **Mistake-aware** — the SKILL.md ships with a curated "Common Mistakes" table covering arrow direction, layout overflow, label escaping, participant ordering, and the C4-include trap
5. **Multi-agent, single file** — one `SKILL.md` runs across Claude Code, Opencode, OpenClaw, Hermes, Codex, and SkillsMP

## Supported diagram types

- **Sequence**: API calls, OAuth flows, protocol traces — with lifelines, activation boxes, async arrows
- **Component / Architecture**: services, modules, queues, databases, clouds — with package/rectangle grouping
- **Class**: OOP models — inheritance, composition, aggregation, multiplicities (`"1" --> "*"`)
- **ER / Entity**: database schemas — `<<PK>>` / `<<FK>>` notation, crow's-foot relationships
- **Activity**: workflows, business processes, decision branches — `if/then/else/endif` syntax
- **Use Case**: actors, system boundaries, user stories
- **State**: state machines, lifecycle flows, `[*] -->` start/end markers
- **C4**: Context, Container, Component diagrams via Kroki's `c4plantuml` endpoint
- **Other**: mind maps (`@startmindmap`), gantt (`@startgantt`)

## Prerequisites

### Option A — Kroki API (recommended, zero install)

```bash
# curl is pre-installed on macOS, Linux, and Windows (Git Bash / WSL)
curl --version
```

That's it. Diagrams render via `https://kroki.io`.

### Option B — Local Kroki via Docker (offline use)

```bash
docker run -d -p 8000:8000 yuzutech/kroki

# Then point the skill at the local endpoint
curl -s -X POST http://localhost:8000/plantuml/png \
  -H "Content-Type: text/plain" \
  --data-binary "@diagram.puml" \
  -o diagram.png
```

### Option C — Local PlantUML jar (air-gapped)

```bash
# macOS
brew install graphviz openjdk
# Ubuntu/Debian
sudo apt install graphviz default-jre

# Download plantuml.jar from https://plantuml.com/download
java -jar plantuml.jar diagram.puml
# → diagram.png is written next to the source
```

| Platform | Notes |
|----------|-------|
| **macOS** | `curl` ships with the OS; Homebrew handles `graphviz` / `openjdk` for Option C |
| **Windows** | Use Git Bash or WSL so `curl` is available; install Java + Graphviz for Option C |
| **Linux** | `curl` ships with most distros; `apt`/`yum`/`pacman` install `graphviz` + `default-jre` |

## Installation

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

## Updates

The skill auto-checks for updates once per 24 hours on first use in a conversation. The check runs `git pull --ff-only` against the install directory and writes a `.last_update` timestamp; if the pull fails (offline, conflict, not a git checkout), the error is swallowed and the workflow continues normally — no user-visible noise.

To pull manually:

```bash
cd <your-install-path>/plantuml-skill && git pull
```

Package-manager installs handle updates themselves:

```bash
# SkillsMP
skills update plantuml-skill
```

## Usage

Just describe what you want:

```
Create a sequence diagram showing the OAuth 2.0 authorization code flow with
Client, Authorization Server, Resource Server, and User
```

The agent will generate the `.puml` file and export it to PNG via Kroki automatically.

## Example

**Prompt:**
> Create a microservices e-commerce architecture with Mobile/Web/Admin clients, API Gateway,
> User/Order/Product/Payment services, Kafka event bus, Notification service,
> and User DB / Order DB / Product DB / Redis Cache / Stripe API

**Output:**

![Microservices Architecture](assets/example.png)

## Files

- `SKILL.md` — **the only required file**. Loaded by all platforms as the skill instructions.
- `agents/openai.yaml` — OpenAI Codex-specific configuration (display name, policy, capabilities, prerequisites)
- Auto-update is inline in `SKILL.md` (step 0): a 24h-throttled `git pull --ff-only` on first use per conversation, recorded in `.last_update`
- `README.md` — this file (English, displayed on GitHub homepage)
- `README_CN.md` — Chinese documentation
- `assets/` — example `.puml` source and rendered PNG

> **Note:** Only `SKILL.md` is needed for the skill to work. `agents/openai.yaml` is only needed for Codex. The `assets/` and README files are documentation only and can be safely deleted to save space.

## Known Limitations

- **Public Kroki rate limits**: The hosted `kroki.io` endpoint is shared and best-effort; for heavy workloads run your own Kroki container (Option B)
- **Network required by default**: Option A needs egress to `kroki.io`. Use Option B (local Docker) or Option C (jar) for offline / air-gapped environments
- **C4 `!include` directives**: The public PlantUML web server resolves `!include` URLs at render time; Kroki may not. Use Kroki's dedicated `c4plantuml` endpoint instead (`https://kroki.io/c4plantuml/png`)
- **Graphviz needed for some diagram types**: Activity and class diagrams use Graphviz internally — Option C requires `graphviz` to be installed alongside Java
- **No vision-based self-check**: Unlike `drawio-skill`, this skill does not read back the rendered PNG to auto-fix layout issues

## License

MIT

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
