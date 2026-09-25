<p align="center">
  <img src="https://aicaststudio.com/showcase/nonna-mimi-queens-1.jpg" alt="Nonna Mimi, an AI influencer made with AI Cast Studio" width="220" style="border-radius:24px">
</p>

<h1 align="center">cast-studio</h1>

<p align="center">
  Meet, cast and film an AI influencer from Claude Code.<br>
  The <a href="https://aicaststudio.com">AI Cast Studio</a> plugin: one sentence in, a character with a face, a voice and 15–20 second talking videos out.
</p>

<p align="center">
  <a href="https://aicaststudio.com">aicaststudio.com</a> ·
  <a href="#install">Install</a> ·
  <a href="#the-flow">The flow</a> ·
  <a href="#tools">Tools</a> ·
  <a href="#pricing">Pricing</a>
</p>

---

## Install

```sh
claude plugin marketplace add AI-Biz-Guy/cast-studio-plugin
claude plugin install cast-studio@cast-studio
```

Then, in Claude Code, run `/mcp`, pick **cast-studio** and authenticate. Sign-in is a 6-digit code
sent to your email, then one consent screen. No password, no API key to paste.

## The flow

Say what you want in plain words and Claude drives the studio's tools in order:

1. **Meet her** — "Make me an 81-year-old nonna from Queens who roasts Gen Z cooking." Free: a
   story, a catchphrase, a handle and a first portrait.
2. **Shape her** — "Make her from Palermo", "funnier", rename her, change her handle. Free.
3. **Pay** — nothing that costs money happens before a plan. Claude hands you a link; you pay on
   the website (Stripe), never in the terminal.
4. **Cast her** — 100 credits. Three looks, three voices lettered A/B/C, her sets, her first scripts.
   Pick a look, then a voice. Three redraws are included.
5. **Film** — your own idea first ("why you should never put cream in carbonara"), or one of her
   scripts. 100 credits a video, about 5 minutes, one video at a time.
6. **Download** — the captioned MP4, the clean MP4, or her whole kit as a ZIP. Free, unlimited.
7. **Redo** — fix the edit (free), a new take (100; free once for her first video), or report a
   broken render (re-filmed free).

Claude always says the price before a paid step (`estimate_only`) and never charges twice on a
retry (every paid tool takes an `idempotency_key`).

## Tools

| Tool | What it does |
| --- | --- |
| `get_account` · `list_transactions` · `get_checkout_link` | Credits, plan, history, and the link to pay on the website |
| `meet_influencer` · `tweak_influencer` · `edit_influencer` | Her story and portrait, changed in a sentence or a field |
| `cast_influencer` · `redraw_looks` · `choose_look` · `redraw_voices` · `choose_voice` | Looks, voices A/B/C, sets, first scripts |
| `write_script` · `list_scripts` · `film_video` · `wait_for_jobs` · `redo_video` | Scripts, filming, progress, redo |
| `list_influencers` · `get_influencer` · `get_downloads` | Your studio, her page, signed download links |

The server is `https://aicaststudio.com/mcp` (MCP Streamable HTTP, OAuth 2.1 with dynamic client
registration). Discovery: `/.well-known/oauth-protected-resource/mcp`. Any MCP client works; this
plugin adds the config and a skill that teaches Claude the flow.

## Pricing

| | Price | Credits a month | Per video |
| --- | --- | --- | --- |
| Starter | $29 | 400 | $7.25 |
| Creator | $79 | 1,150 | $6.87 |
| Studio | $199 | 3,200 | $6.22 |

New influencer 100 credits · video 100 · scripts, tweaks and downloads free · 3 redraws per
influencer · top-up 400 for $29 · yearly is one month free.

## Good to know

- Every influencer is fictional and ships with an AI disclosure in her bio. Turn on your
  platform's AI label when you post.
- Filming keeps going if you close Claude Code; `wait_for_jobs` picks it back up, and you get an email.
- A render that fails on the studio's side refunds its credits automatically.

MIT licensed. Questions: support@aicaststudio.com.
