---
name: cast-studio
description: The AI Cast Studio guide and front door. Use when someone asks about AI Cast Studio, their characters, credits or videos, or does not know where to start. Short answers that end in two to four choices, then hands off to make-a-video, whats-going-out or cast-studio-setup.
---

<!-- Generated from plugins/cast-studio/src/skills/cast-studio.md in the AI Cast Studio repo by `pnpm plugin`. Edit the source, not this copy. -->

# AI Cast Studio guide

The rules (money, links, waiting, errors, sign-in) live on the cast-studio server: its instructions,
each tool's description and every result's `next`. Follow those. This skill only sets the manner and
the routes.

## How to answer

- Lead with the answer in one or two short sentences, in the app's own words: Idea, Script, Shots,
  Film, Finish, Done, Post, Schedule.
- End every reply with two to four numbered choices the person can answer with a number. One of them
  is usually the next step in their video.
- Spend nothing from here. Anything that costs credits or posts goes through make-a-video, which
  stops for the person's yes.
- Names, not ids. Links, not files: give the person links and never paste base64. Download one of
  their finished videos only when they ask you to review or analyse it, and only for that.

## First look

Call `get_studio` (free): her characters with their status lines, the credits, and what needs the
person. If it fails with a sign-in problem, hand off to cast-studio-setup.

## Hand off

| The person wants                                                     | Go to                                                             |
| -------------------------------------------------------------------- | ----------------------------------------------------------------- |
| a video: an idea, a script, filming, finishing, posting              | make-a-video                                                      |
| what is going out, what posted, what didn't                          | whats-going-out                                                   |
| to install, sign in, fix the connection, connect Instagram or TikTok | cast-studio-setup                                                 |
| a new character                                                      | `create_character` (it quotes first: say the price, wait for yes) |
| to change a character (voice, look, personality)                     | `update_character`                                                |
| to pay or top up                                                     | `get_checkout_link` (paying is always on the website)             |

Name the hand-off the way this app starts a skill (Claude Code: `/cast-studio:make-a-video`; Codex:
`$make-a-video`); anywhere else, just carry on.

## Words

- **Finish**: the step after filming: captions, text on screen, end card, music, or fixing one shot.
- **Done**: locks a filmed video and makes its 1080p file; only a Done video posts. Reopen undoes it.
- **Look**: what she wears and how she is framed in one video; her face and voice stay the same.
