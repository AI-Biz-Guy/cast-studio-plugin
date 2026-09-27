---
name: cast-studio
description: Use when the user wants to create or run an AI influencer with AI Cast Studio — meet a character from one sentence, cast the whole character card in one go, edit any part of it, write scripts, film 15–20 second talking videos, redo or download them, or check credits. Drives the cast-studio MCP server tools in the right order and never spends credits without saying the price first.
---

# AI Cast Studio, from Claude Code

The `cast-studio` MCP server (https://aicaststudio.com/mcp) is the same studio as the website: one
tool per screen. Sign-in is OAuth in the browser the first time (`/mcp` in Claude Code → authenticate).

## The flow, in order

1. **Meet** — `meet_influencer { brief }`. Free. One sentence or a pasted brief becomes a character
   with a story and a first portrait, saved as _met_. Show the user her name, hometown, catchphrase
   and the portrait link (`get_influencer` → `portraitUrl` is relative to https://aicaststudio.com).
2. **Shape the story** (free, any time) — `tweak_influencer { influencer_id, text, idempotency_key }` for "make them from Palermo" or
   "funnier". It returns an accepted job: use `wait_for_jobs` before showing the updated card. Before
   casting a look change redraws the portrait; after casting the drawn look stays (use `redraw_looks`).
   Use `edit_influencer` for the name, age, hometown or handle. Only what was asked changes.
3. **Pay** — nothing that costs money happens before a plan. If `get_account` says `canSpend: false`,
   give the user `get_checkout_link` and stop until they've paid (Starter $29 = 400 credits).
4. **Cast** — `cast_influencer` costs **100 credits**. Always call it with `estimate_only: true`
   first and tell the user the cost and balance before the real call. Then `wait_for_jobs` (it
   long-polls 15 s; call it again while `done` is false). One job makes the whole card: the met
   portrait becomes the look, the character sheet is drawn from it, three voices are auditioned and
   the first saved (B and C stay as alternates), three sets are built, three scripts written. There is
   nothing to pick.
5. **Edit the card** (all free except the 3 included redraws) — `redraw_looks { notes }` draws one
   new look and applies it (sheet redrawn); `redraw_voices { notes }` auditions three new voices and
   saves the first; `choose_voice { voice_id }` switches to an alternate (`get_influencer` → `voices`,
   lettered A/B/C, `voiceId` is the current one); `edit_sets { notes }` rethinks and redraws the three
   sets; `rewrite_script { post_id, notes }` rewrites one unfilmed script. Each of these except
   `rewrite_script` returns a job: `wait_for_jobs`. Look and voice lock when the first video films;
   story, sets and scripts stay editable; filmed videos never change.
6. **Her first video** — the user's own idea first: `write_script { idea }` (free). Her own scripts
   are in `list_scripts`, each with its `credits` (5 a second of its length at their pace, 10–60 s; a 20-second script is 100) and `estimatedSeconds`. `film_video { post_id }` charges that price: estimate first, say the
   price, then film. Filming takes about 5 minutes: `wait_for_jobs` until `done`. One video at a
   time; never film several without the user choosing each one.
7. **Download** — `get_downloads` gives signed links (24 h): the captioned MP4, the clean MP4, and
   her whole kit as a ZIP. Downloads are free and unlimited.
8. **Redo** — `redo_video { kind }`: `edit` (captions on/off) is free and instant; `take` and
   `words` film again at the script's price (free once for her first video); `broken` re-films free. Estimate first.

## Rules

- Say the price on every paid step before doing it; `estimate_only: true` exists for that.
- Pass a fresh `idempotency_key` on `tweak_influencer` (required), `cast_influencer`, `film_video` and `redo_video`; if a call
  times out, retry with the same key — it never charges twice.
- Tool errors come back as `{ error: { code, message } }`. `no_plan`, `past_due` and
  `insufficient_credits` mean the user has to act on the website (`get_checkout_link`). `internal`
  is a snag on the studio's side: try again later, or email support@aicaststudio.com.
- Every influencer is fictional and ships with an AI disclosure in her bio; remind the user to turn
  on their platform's AI label when they post.
