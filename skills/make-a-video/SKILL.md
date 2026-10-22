---
name: make-a-video
description: Make a video with an AI Cast Studio character, from an idea to posted, stopping for the person's yes before anything that costs credits or posts. Use when someone wants a new video, to film a script, fix a shot, finish a video or post it.
---

<!-- Generated from plugins/cast-studio/src/skills/make-a-video.md in the AI Cast Studio repo by `pnpm plugin`. Edit the source, not this copy. -->

# Make a video

**Goal:** a finished video the person likes, posted or scheduled when they say so.

The order and the rules live on the server: its instructions, each tool's description, and the
`next` on every result. Follow `next` unless the person asked for something else.

## The flow and its stop points

`make_video` runs the whole flow one step at a time for a `video_id` and stops at each point below
with a choice. The step tools are there when the person wants control: `write_script`, `plan_shots`,
`film`, `fix_shot`, `finish_video`, `set_video_state`, `post_video`.

1. **Idea → Script** (free). The script is written in the background: the start answers at once
   with `video_id` and a job. `wait_for_job`, then `make_video` with that `video_id`. If the host
   times out, call the same start again: it answers the same video, never a second one. Show the
   hook, the lines and the length against what was asked. Stop: keep it, change a line, or rewrite
   with a note.
2. **Shots.** Drawing a frame for each shot costs credits. Stop at the quote.
3. **Frames.** Show the frame links. Stop: film it, change a shot, or stop here.
4. **Film.** Stop at the quote. Then `wait_for_job` until it is done, saying the stage and the count
   it reports. A film is a few calls, not dozens.
5. **Watch.** Give the watch link, when it expires, and that anyone with the link can watch. Stop:
   Done, fix a shot, or Finish touches.
6. **Finish**: captions, text on screen, end card, music. Fixing a shot is paid and quoted.
7. **Done** locks the video and makes its 1080p file. Say so before you do it.
8. **Post.** Always stop, even if the person said "post it" earlier: show the accounts, the caption
   and the time in the person's time zone and in her city's. If posts wait for the person's OK, say
   where to approve them.

## At a quote

A tool that spends answers `needs_confirmation` with a quote. Say the credits, the dollars and the
balance after, in those words, and ask. Only after a yes, call the same tool again with the same
arguments plus `quote_id`. No yes, no `quote_id`.

## Never

- Never start a second film without the person choosing it.
- Never paste base64: give links. Download the person's finished video only when they ask you to
  review or analyse it, and only for that.
- On an error, say its message as written and offer its `next`. When it lists `also`, tell the
  person all of it at once.
- Never pick a voice for the person, and when `post_intent.confirm_with_person` is true, ask its
  `question` before going on.
