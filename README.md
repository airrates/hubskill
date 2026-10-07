# airrates-api skill

An agent skill for reading [airrates](https://airrates.net) data: perpetual-futures funding rates,
cross-exchange spreads, liquidations and markets. Read-only, no API key.

`SKILL.md` is the skill. It is the same text airrates serves at
<https://airrates.net/.well-known/agent-skills/airrates-api/SKILL.md>, with an examples section and the
metadata ClawHub reads. The site's copy is generated from its endpoint list, so when airrates adds or
changes an endpoint, update `SKILL.md` here to match and publish a new version.

Other ways in: the [OpenAPI description](https://airrates.net/v1/openapi.json),
the [API catalog](https://airrates.net/.well-known/api-catalog), an
[MCP server](https://airrates.net/.well-known/mcp/server-card.json) and an
[A2A agent](https://airrates.net/.well-known/agent-card.json).

Not financial advice. Spreads and backtests are before fees and slippage.

License: MIT-0, as ClawHub publishes all skills.
