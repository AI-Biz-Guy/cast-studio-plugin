# Changelog

Each version of the AI Cast Studio plugin. `scripts/publish-plugin.sh` uses the entry for the version
it publishes as the release note.

## 2.0.1

- `make_video` takes the look in the same call: outfit, eyes, framing, in view, direction and whether
  the hook text shows. Exact words stay exact through the storyboard and the recording.
- The script's seconds match the shots it plans.
- `wait_for_job` waits up to 50 s and says when to call again; a track says it is already on, at
  what volume and in which version; `make_music` takes a volume.
- Every watch link says which version it plays and whether a newer one is still being made.
- Shots say what she is doing, how the frame starts and what the judge checked.
- `post_video` saves an edited caption, keeps a time zone named in the words, and points a refused
  choice at the drafts.
- A posted video says Posted in every tool; `get_studio` for one character points at `make_video`
  and starts no background work.

## 2.0.0

- One plugin for Claude Code, Codex and Cursor, plus setup steps for Claude.ai, Claude Desktop and
  ChatGPT.
- Four skills: `cast-studio` (the guide), `cast-studio-setup`, `make-a-video` and `whats-going-out`.
- The tools follow the app as it is today: Idea, Script, Shots, Film, Finish, Done, Post and Schedule.
  Every spend answers with a quote first and runs only after your yes; posting always stops for you.
- Media comes back as links you can open, with when they expire.
- `TOOLS.md` lists every tool, generated from the live server.
- Every settled job says the call to make next (a new character comes back with its id and its
  voices to pick), and `wait_for_job` counts every job's progress.
- When and where to post a video is saved on it and carried to `post_video` by every step.
- A rejected frame names every shot that needs one and says when to stop redrawing and use it anyway.
- Each voice says how it sounds; the script's seconds add up; `get_studio` says which daily limit
  applies; an edit in `finish_video` comes back with a link to watch the new version.
- A refusal while something is being made says when to try again and which job to wait for.
- "Tomorrow 8:00 am" is read as a day and time when it is said, and `post_video` draft → choose →
  post hands on the accounts and that day and time.
- A `finish_video` edit that changes nothing says so (`changed: false`) instead of "Saved".
- The script says its length is "about" until the shots are drawn, gives the film price as an
  estimate until then, and each line names the shot that carries it.
- An agent cannot approve a waiting post and is told so, with the Schedule link; it can move or
  cancel the request with `manage_post`.
- A video waiting for the person's OK is not "Ready to post" anywhere.
- Drawing frames counts frames drawn with a measured wait; filming counts only shots that came back.
- The frame check never turns a frame down for a background prop nobody asked for; a kept frame
  lists no problems.
- Covers are checked against the ones offered; `plan_shots` hands a make_video video back to it.
- Shots are numbered from 1 everywhere, as the app shows them: `shot` in every argument, `shots[].shot`,
  each script line's shot, quotes and a film's kept and failed shots.
- A rejected frame gets one recommendation: once it is advised to use it anyway, nothing else offers
  a free redraw; a shot's attempts count every frame drawn for it, redraws included.
- Every refusal names a next that works now: Finish or posting a video not filmed yet points to
  `make_video`; an unknown account, post or Ideas account points to the list it is in; a character
  still being made says which job to wait for.
- Every quote says whether it fits today's AI-app limit; one that doesn't is refused before anyone
  says yes, saying when it frees up (in the person's time zone) and to raise the limit in Settings.
  `make_video` and `get_studio` say a whole request up front against what is left.
- Each voice's description is written from its own audio (pitch, pace, texture, energy), so the
  three can be told apart without listening; none is cut short. One not described yet says so
  ("Not described yet: listen to choose") and names the job describing it (`voices_job_id`).
- `post_video choose` hands over a call that works: connect the platform first when it is not
  connected, the full post call from the saved plan when it is; a call with fields missing names
  each one and hands back the call that fixes it.
- A video with no posting plan says so plainly, and `make_video` or `post_video` draft/choose take
  where and when at any point.
- When an API key's own daily limit is the one that applies, every limit line and refusal names it
  beside what the account allows, and points to the key's setting.
- Before a character's first film, `make_video` stops once at a voice check: the three voices, the
  current one marked, her brief, and that the voice locks when the first video films.
- A character with a video in progress reads its line ("Shots · not filmed yet"), never "No videos yet".
- Restoring a version that has its file returns its watch link at once, nothing made again.
- After filming, `wait_for_job` and `make_video`'s resume give the same next.
- Draft captions are written in the character's own first-person voice.
- A script is written in the background: `make_video` and `write_script` write answer at once with
  the video's id and the job to wait for, and a retry of the same start (`start_key`, or the same
  idea within 10 minutes) answers the same video, never a second one.
- A quote says whether the rest of the request still fits what it leaves of today's limit (after a
  character, a typical first video; after the frames, filming).
- `update_character pick_voice` never fills in a voice: its next lists the three with what each
  sounds like, to choose with the person.
- "Tomorrow" in the character's city when it is another day for the person comes back with both
  readings, `confirm_with_person` and one question; the post call says the time in both zones.
- When the idea asks for a length, the script says what it runs against what was asked, and why.
- A refusal that stops the flow (the daily limit, credits) also lists everything else the person has
  to do before it can post, such as connecting TikTok.
