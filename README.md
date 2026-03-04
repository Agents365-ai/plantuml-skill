# plantuml-skill

Claude Code skill for generating PlantUML diagrams and exporting to PNG/SVG via Kroki — no Java or local install required.

[中文文档](README_CN.md)

## What it does

- Generates `.puml` PlantUML source files from natural language descriptions
- Exports diagrams to PNG or SVG using the **Kroki** rendering API
- Supports sequence, component, class, ER, activity, state, and more diagram types
- Triggers automatically when diagrams would help explain complex systems

## Dependencies

| Tool | Purpose |
|------|---------|
| `curl` | Send `.puml` to Kroki API for rendering |
| Kroki (`kroki.io`) | Cloud/local PlantUML renderer — no Java needed |

`curl` is pre-installed on macOS, Linux, and Windows (Git Bash / WSL).

## Install

### Option A: Kroki API (recommended — zero install)

```bash
# curl is already available — just verify:
curl --version
```

No additional setup. Diagrams are rendered via `https://kroki.io`.

### Option B: Local Kroki with Docker (offline use)

```bash
docker run -d -p 8000:8000 yuzutech/kroki

# Use http://localhost:8000 instead of https://kroki.io
curl -s -X POST http://localhost:8000/plantuml/png \
  -H "Content-Type: text/plain" \
  --data-binary "@diagram.puml" \
  -o diagram.png
```

### Option C: Traditional PlantUML + Java

```bash
# macOS
brew install graphviz
# Download plantuml.jar from https://plantuml.com/download
java -jar plantuml.jar diagram.puml
```

## Usage

Just describe what you want:

```
Create a sequence diagram showing the OAuth 2.0 authorization code flow with
Client, Authorization Server, Resource Server, and User
```

Claude will generate the `.puml` file and export it to PNG automatically.

## Example

**Prompt:**
> Create a microservices e-commerce architecture with Mobile/Web/Admin clients, API Gateway,
> User/Order/Product/Payment services, Kafka event bus, Notification service,
> and User DB / Order DB / Product DB / Redis Cache / Stripe API

**Output:**

![Microservices Architecture](assets/example.png)

## Files

- `SKILL.md` — skill instructions loaded by Claude Code
- `README.md` — this file (English)
- `README_CN.md` — Chinese documentation
- `assets/` — example diagrams

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
  </tr>
</table>

## Author

**Agents365-ai**

- Bilibili: https://space.bilibili.com/441831884
- GitHub: https://github.com/Agents365-ai
