---
name: cast-studio
description: Use when the user wants to create or run an AI influencer with AI Cast Studio — meet a character from one sentence, cast her look and voice, write scripts, film 15–20 second talking videos, redo or download them, or check credits. Drives the cast-studio MCP server tools in the right order and never spends credits without saying the price first.
---

# AI Cast Studio, from Claude Code

The `cast-studio` MCP server (https://aicaststudio.com/mcp) is the same studio as the website: one
tool per screen. Sign-in is OAuth in the browser the first time (`/mcp` in Claude Code → authenticate).

## The flow, in order

1. **Meet** — `meet_influencer { brief }`. Free. One sentence or a pasted brief becomes a character
   with a story and a first portrait, saved as *met*. Show the user her name, hometown, catchphrase
   and the portrait link (`get_influencer` → `portraitUrl` is relative to https://aicaststudio.com).
2. **Shape her** (free, before casting) — `tweak_influencer { text }` for "make her from Palermo" or
   "funnier"; `edit_influencer` for the name, age, hometown or handle. Only what was asked changes.
3. **Pay** — nothing that costs money happens before a plan. If `get_account` says `canSpend: false`,
   give the user `get_checkout_link` and stop until they've paid (Starter $29 = 400 credits).
4. **Cast** — `cast_influencer` costs **100 credits**. Always call it with `estimate_only: true`
   first and tell the user the cost and balance before the real call. Then `wait_for_jobs` (it
   long-polls 15 s; call it again while `done` is false). Casting draws three looks, auditions three
   voices (A/B/C), builds her sets and writes her first scripts.
5. **Pick her look, then her voice** — `choose_look { look_id }` (from `get_influencer` → `looks`),
   then `choose_voice { voice_id }` (`voices`, lettered A/B/C). `redraw_looks` / `redraw_voices`
   with the user's notes use one of her 3 included redraws each. Look and voice lock when her first
   video films.
6. **Her first video** — the user's own idea first: `write_script { idea }` (free). Her own scripts
   are in `list_scripts`. `film_video { post_id }` costs **100 credits**: estimate first, say the
   price, then film. Filming takes about 5 minutes: `wait_for_jobs` until `done`. One video at a
   time; never film several without the user choosing each one.
7. **Download** — `get_downloads` gives signed links (24 h): the captioned MP4, the clean MP4, and
   her whole kit as a ZIP. Downloads are free and unlimited.
8. **Redo** — `redo_video { kind }`: `edit` (captions on/off) is free and instant; `take` and
   `words` film again for 100 (free once for her first video); `broken` re-films free. Estimate first.

## Rules

- Say the price on every paid step before doing it; `estimate_only: true` exists for that.
- Pass a fresh `idempotency_key` on `cast_influencer`, `film_video` and `redo_video`; if a call
  times out, retry with the same key — it never charges twice.
- Tool errors come back as `{ error: { code, message } }`. `no_plan`, `past_due` and
  `insufficient_credits` mean the user has to act on the website (`get_checkout_link`). `internal`
  is a snag on the studio's side: try again later, or email support@aicaststudio.com.
- Every influencer is fictional and ships with an AI disclosure in her bio; remind the user to turn
  on their platform's AI label when they post.
