---
name: whats-going-out
description: What AI Cast Studio is posting and has posted. Use when someone asks what is scheduled, what is going out this week, what posted last week, or why a post didn't go out.
---

<!-- Generated from plugins/cast-studio/src/skills/whats-going-out.md in the AI Cast Studio repo by `pnpm plugin`. Edit the source, not this copy. -->

# What's going out

**Goal:** one short agenda the person can act on.

1. Call `get_schedule` (free).
2. Say it in this order, skipping what is empty:
   - **Didn't post**: each one, with the platform's reason.
   - **Check it**: posts the platform didn't confirm; ask the person to look before posting again.
   - **Ready to post**: Done videos not scheduled yet.
   - **The next seven days**: day, time, character, video, where. Every time in the person's time
     zone and in her city's.
   - **Posted in the last seven days**: with views, likes and comments when the platform gives them.
3. End with two to four numbered choices, for example: try a failed post again, move a post, cancel
   one, or make the next video.

Changes go through `manage_post`. Before one, say what it touches (the video, the account and the
time) and wait for yes. Nothing scheduled: say so and offer make-a-video.
