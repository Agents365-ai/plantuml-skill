# plantuml-skill

用于在 Claude Code 中生成 PlantUML 图表并通过 Kroki 导出为 PNG/SVG 的 skill — 无需安装 Java 或本地工具。

[English](README.md)

## 功能说明

- 根据自然语言描述生成 `.puml` PlantUML 源文件
- 通过 **Kroki** 渲染 API 导出为 PNG 或 SVG
- 支持时序图、组件图、类图、ER 图、活动图、状态图等多种类型
- 当图表有助于解释复杂系统时自动触发

## 依赖项

| 工具 | 用途 |
|------|------|
| `curl` | 将 `.puml` 发送到 Kroki API 渲染 |
| Kroki (`kroki.io`) | 云端/本地 PlantUML 渲染器 — 无需 Java |

`curl` 在 macOS、Linux、Windows（Git Bash/WSL）上已预装。

## 安装

### 方式 A：Kroki API（推荐 — 零安装）

```bash
# curl 已预装，验证即可：
curl --version
```

无需额外配置，图表通过 `https://kroki.io` 渲染。

### 方式 B：本地 Kroki（Docker，离线使用）

```bash
docker run -d -p 8000:8000 yuzutech/kroki

# 将 https://kroki.io 替换为 http://localhost:8000
curl -s -X POST http://localhost:8000/plantuml/png \
  -H "Content-Type: text/plain" \
  --data-binary "@diagram.puml" \
  -o diagram.png
```

### 方式 C：传统 PlantUML + Java

```bash
# macOS
brew install graphviz
# 从 https://plantuml.com/download 下载 plantuml.jar
java -jar plantuml.jar diagram.puml
```

## 使用方式

直接描述你想要的图表：

```
画一个 OAuth 2.0 授权码流程的时序图，包含 Client、Authorization Server、Resource Server 和 User
```

Claude 会自动生成 `.puml` 文件并导出为 PNG。

## 示例

**提示词：**
> 画一个微服务电商架构图，包含 Mobile/Web/Admin 客户端，API Gateway，
> User/Order/Product/Payment 微服务，Kafka 事件总线，Notification 服务，
> User DB / Order DB / Product DB / Redis Cache / Stripe API

**输出效果：**

![微服务架构图](assets/example.png)

## 文件说明

- `SKILL.md` — Claude Code 加载的 skill 指令文件
- `README.md` — 英文说明
- `README_CN.md` — 本文件（中文）
- `assets/` — 示例图表

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
  </tr>
</table>

## 作者

**Agents365-ai**

- Bilibili: https://space.bilibili.com/441831884
- GitHub: https://github.com/Agents365-ai
