# cast-studio

The [AI Cast Studio](https://aicaststudio.com) plugin for Claude Code: meet an AI influencer from
one sentence, cast her look and voice, write scripts and film 15–20 second talking videos, without
leaving the terminal. Payment stays on the website.

```sh
claude plugin marketplace add AI-Biz-Guy/cast-studio
claude plugin install cast-studio
```

Then `/mcp` → `cast-studio` → authenticate (sign in with your email code in the browser). The
`cast-studio` skill teaches the flow: meet → shape → pay → cast → pick look → pick voice → first
video → download. Every paid tool takes `estimate_only` and an `idempotency_key`.

MCP server: `https://aicaststudio.com/mcp` (Streamable HTTP, OAuth 2.1 with dynamic client
registration). Discovery: `/.well-known/oauth-protected-resource/mcp`.
