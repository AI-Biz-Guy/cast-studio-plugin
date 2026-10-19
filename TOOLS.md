# AI Cast Studio tools

The tools the AI Cast Studio server (`https://app.aicaststudio.com/mcp`) lists, as of plugin 2.0.0. Generated from the live server by `pnpm plugin`: do not edit by hand.

Every tool that spends credits answers first with a quote and spends only when called again with its `quote_id`; every tool that posts stops for the person. Errors carry `next`, the call that fixes them.

| Tool                | What it does                                              | What it may do                                                      |
| ------------------- | --------------------------------------------------------- | ------------------------------------------------------------------- |
| `get_studio`        | The studio at a glance                                    | Reads only                                                          |
| `get_checkout_link` | Pay in the browser                                        | Reads only                                                          |
| `create_character`  | Create a character (46 credits)                           | Changes or removes things                                           |
| `update_character`  | Change a character                                        | Changes or removes things                                           |
| `make_video`        | Make a video, start to finish                             | Changes or removes things                                           |
| `write_script`      | Write and edit a script                                   | Makes or changes things                                             |
| `plan_shots`        | Plan the shots                                            | Changes or removes things                                           |
| `film`              | Film a video from its shots                               | Changes or removes things                                           |
| `fix_shot`          | Fix a shot of a filmed video                              | Changes or removes things                                           |
| `finish_video`      | Finish a video: captions, text, end card, music, versions | Makes or changes things                                             |
| `make_music`        | Make music for a video                                    | Makes or changes things                                             |
| `set_video_state`   | Done, reopen, archive or unarchive a video                | Changes or removes things, safe to repeat                           |
| `list_videos`       | A character’s videos                                      | Reads only                                                          |
| `get_video`         | One video                                                 | Reads only                                                          |
| `get_downloads`     | Download a video                                          | Makes or changes things, safe to repeat                             |
| `connect_account`   | Connect Instagram, TikTok, YouTube or Facebook            | Makes or changes things, reaches outside the studio, safe to repeat |
| `post_video`        | Post a video to social accounts                           | Changes or removes things, reaches outside the studio               |
| `get_schedule`      | What is going out                                         | Reads only                                                          |
| `manage_post`       | Move, cancel or retry a post                              | Changes or removes things, reaches outside the studio               |
| `ideas`             | Ideas: accounts to learn from                             | Makes or changes things, reaches outside the studio                 |
| `wait_for_job`      | Wait for a job                                            | Reads only                                                          |
| `cancel_job`        | Cancel a job                                              | Changes or removes things, safe to repeat                           |

## get_studio

**The studio at a glance.** Free, read-only. The account: credits (and held for work still running), plan and about how many dollars a credit is, every character with the line the app shows ("Pick a voice", "Filmed · not done yet"), what needs the person now, recent spending, and the daily limit for AI apps with what is left today and whether a new character and a typical video fit it (plan_ahead). Pass character_id for the whole card: look and voice links to see and hear, voices to pick from, personality, direction for the next videos, and the character’s city. Start here; payment is get_checkout_link. A new character is 46 credits; a typical 20-second video of 3 shots about 107 credits in all (drawing its shots 8 plus 3 a shot, filming 4.5 credits a second); scripts, voices and posting are free.

- `character_id`: One character, for the whole card.
- `history`: How many recent credit entries to include (default 10).
- `include_archived`: Archived characters too.

## get_checkout_link

**Pay in the browser.** Free; never charges. Paying always happens on the website: this gives the page to open, to choose or change a plan, or to top up (400 credits for $29). Give the person the link; once paid, get_studio shows the credits.

- `what`: top_up (default when there is a plan): buy credits. plan: choose or change a plan. One of: `top_up`, `plan`.
- `plan`: With what plan: the plan to open on. One of: `starter`, `creator`, `studio`.
- `interval`: With what plan: monthly (default) or yearly. One of: `month`, `year`.

## create_character

**Create a character (46 credits).** One sentence (or a full description) becomes a whole character in one job: a story and handle, a portrait, character sheets and three voices to hear. 46 credits: the first call answers needs_confirmation with the price in credits and about $; show it, ask, and only after a yes call again with quote_id. Needs a plan. Starts a job and returns at once: wait_for_job, then get_studio with character_id to see the character and hear the voices, then update_character pick_voice. Nothing is written for the character to say: each video starts from an idea.

- `description` (required): Who the character is, in a sentence or more.
- `quote_id`: Leave out at first: the tool answers needs_confirmation with a quote. After the person says yes, call again with the same arguments and quote.quote_id. Single use and also the idempotency key: retrying with it never charges twice.

## update_character

**Change a character.** Changes one character. pick_voice: without voice_id answers the three voices with links to hear each (nothing changes); with voice_id makes it the character’s (free; required before the first video). new_voices: three new voices to pick from. change_look: draws the look again (with an optional note). Both are free while free changes are left, then 12 credits, quoted first (needs_confirmation: show the price, ask, call again with quote_id); both lock once the first video films. personality: the story in a sentence ("funnier, from Palermo"; free), and/or the name, age, hometown or handle. for_her_next_videos: direction every new video of the character starts with ("more energy, talk slower"; empty clears it). archive: the character leaves the studio and their scheduled posts are cancelled (say so first); unarchive brings them back.

- `action` (required): pick_voice: Hear the voices, or pick one (free). new_voices: Three new voices to pick from (free change, then quoted). change_look: Draw the look again (free change, then quoted). personality: The story, name, age, hometown or handle (free). for_her_next_videos: Direction the new videos start with (free). archive: Archive the character; cancels their scheduled posts. unarchive: Bring the character back. One of: `pick_voice`, `new_voices`, `change_look`, `personality`, `for_her_next_videos`, `archive`, `unarchive`.
- `character_id` (required): The character (from get_studio).
- `voice_id`: A voice_id from the character’s voices. (action pick_voice)
- `note`: What should be different. (action new_voices, change_look)
- `story`: A change to the story, in a sentence. (action personality)
- `name`: (action personality)
- `age`: (action personality)
- `hometown`: City plus region: post times follow it. (action personality)
- `handle`: (action personality)
- `direction`: Empty clears it. (action for_her_next_videos)
- `quote_id`: Leave out at first: the tool answers needs_confirmation with a quote. After the person says yes, call again with the same arguments and quote.quote_id. Single use and also the idempotency key: retrying with it never charges twice.

## make_video

**Make a video, start to finish.** The whole flow in one tool: idea → script → shots → film → finish → Done → post. Start with character_id and idea (optionally format, words_exactly, and when and where to post it): the script is written in the background, so the start answers at once with stop wait, video_id and job_id (wait_for_job, then make_video with video_id), and the same start again (start_key, or the same idea within 10 minutes) answers the same video, never a second; carry on with video_id (post_when and post_to are saved on the video: every next and resume carries them, and post_video's next says them). Each call does the free work and stops at the next decision, saying it in "stop" with the one call to make in "next". It stops at: draw_frames (drawing a frame per shot costs credits), voice_check (once, before a character's first film: the voices against the character’s voice brief, since the voice locks when the first video films; step voice_check keeps it), film (filming costs credits), refilm (a paid re-film, once the free one is used), wait (something is being made: wait_for_job, then the call in resume), fix_shots (a frame to fix before filming), change_shot (the video service turned a shot down), review (filmed: watch it, edit with finish_video, then Done), post (Done: posting is post_video, never done here), archived. Before a spend it says every spend left on the way to Done and whether they fit today's AI-app limit (ahead). A failed film's first re-film is free and runs on its own. To go past a stop, call again with step set to it: a spend answers needs_confirmation with the price in credits and about $ and charges only with quote_id after the person says yes.

- `video_id`: Carry on with this video. Leave out to start one.
- `character_id`: To start: the character (from get_studio).
- `idea`: To start: the idea, or the exact words with words_exactly.
- `format`: To start: a format (write_script formats). One of: `Talking to camera`, `POV`, `Talking joke`, `Dance or posing`, `Outfit change`, `Text meme`, `Voiceover how-to`, `Talking + scenes`.
- `words_exactly`: To start: true uses the idea as the exact words the character says.
- `start_key`: To start: any string you choose for this start. The same start_key again answers the same video (a retry after a timeout), never a second one. Without it, the same character and idea within 10 minutes is the same video.
- `step`: Go past this stop (from the last result’s next): draw_frames, film or refilm (spends, quoted first), voice_check (keeps the voice, free), done (marks it Done). One of: `draw_frames`, `voice_check`, `film`, `refilm`, `done`.
- `post_when`: When the person wants it posted, in their words ("Thursday 7pm"); saved on the video and handed to post_video.
- `post_to`: Where to post it (instagram, tiktok, youtube, facebook); saved on the video and handed to post_video.
- `quote_id`: Leave out at first: the tool answers needs_confirmation with a quote. After the person says yes, call again with the same arguments and quote.quote_id. Single use and also the idempotency key: retrying with it never charges twice.

## write_script

**Write and edit a script.** Free. The Script step: writes the character’s script from your idea and edits it until the words are right. action "formats" lists the formats; "write" writes a new video from an idea (optionally in a format, or "words_exactly" to use your words as written), in the background: it answers video_id and job_id at once, and wait_for_job's next shows the script; "rewrite" rewrites it from a note (scope "staging" keeps every word, "words" changes them too); "pick_hook" puts one of the three openers on line 0, "more_hooks" writes three more; "hook_text" turns the hook text over the video on or off (the character’s next videos start the same way); "edit_line" / "delete_line" change one line; "look" decides how this video looks (outfit, eyes, framing, what is in view), which every frame and shot follows. Needs the character’s voice picked first. Then plan_shots (or make_video for the whole flow). A video is filmed from its shots: plan_shots draws a frame per shot (8 plus 3 a shot), then film films them at 4.5 credits a second: about 107 credits in all for a 20-second video of 3 shots.

- `action` (required): formats: List the formats a video can be written in. write: Write a new video from an idea. rewrite: Rewrite the script from a note. pick_hook: Put one of the openers on line 0. more_hooks: Write three more openers. hook_text: Turn the hook text over the video on or off. edit_line: Change one line: its words, its kind, or both. delete_line: Delete one line. look: Set how this video looks. One of: `formats`, `write`, `rewrite`, `pick_hook`, `more_hooks`, `hook_text`, `edit_line`, `delete_line`, `look`.
- `character_id`: The character (from get_studio). (action write)
- `idea`: The idea: a sentence, pasted facts, or the words to say (up to 5000 characters). (action write)
- `format`: One of the formats (action formats says what each one does). Optional. (action write) One of: `Talking to camera`, `POV`, `Talking joke`, `Dance or posing`, `Outfit change`, `Text meme`, `Voiceover how-to`, `Talking + scenes`.
- `words_exactly`: True: the idea is the exact words the character says (they must fit one video). Default false: the script is written from the idea. (action write)
- `from_video`: A video from Ideas (the ideas tool) to follow the shape of, never its words. Optional. (action write)
- `video_id`: The video (video_id from make_video, write_script or list_videos). (action rewrite, pick_hook, more_hooks, hook_text, edit_line, delete_line, look)
- `note`: What to change ("shorter, about garlic"), up to 500 characters. (action rewrite)
- `scope`: "staging" keeps every word (who is on screen, places only); "words" rewrites the words too. Default: staging for words written exactly, else words. (action rewrite) One of: `staging`, `words`.
- `option`: The opener (script.hooks[].option). (action pick_hook)
- `on`: True shows the hook text; the character’s next videos start the same way. (action hook_text)
- `line`: The line, counted from 0 (0 is the opener). (action edit_line, delete_line)
- `text`: The new words. (action edit_line)
- `kind`: talking (said to the camera), narrated (the character on screen, voice over it), scene (the place, voice over it). (action edit_line) One of: `talking`, `narrated`, `scene`.
- `outfit`: (action look)
- `eyes`: (action look)
- `framing`: (action look)
- `in_view`: (action look)
- `direction`: Style, movement and mood for the filming ("walking, selfie style"). (action look)

## plan_shots

**Plan the shots.** The Shots step: a frame for each shot before anything is filmed. action "plan" plans the shots and draws a frame each (8 credits plus 3 a shot; again after the words change). Free: "edit_shot" (its words, length, place, framing, camera, what the character is doing), "remove_shot", "use_anyway" (keep a frame our check rejected). Paid (quoted first, then call again with quote_id after the person says yes): "add_shot" (3), "redraw_frame" (3, free after a rejection), "redraw_out_of_date" (every frame that no longer shows what its shot says). Drawing starts a job: wait_for_job, then get_video for the frames. Then film.

- `action` (required): plan: Plan the shots and draw a frame each (quoted). edit_shot: Change one shot (free). add_shot: Add a shot and draw its frame (3 credits, quoted). remove_shot: Remove a shot (free). redraw_frame: Draw one frame again (3 credits, free after a rejection; quoted). use_anyway: Keep a frame our check rejected (free). redraw_out_of_date: Draw every out-of-date frame again (quoted) One of: `plan`, `edit_shot`, `add_shot`, `remove_shot`, `redraw_frame`, `use_anyway`, `redraw_out_of_date`.
- `video_id` (required): The video (video_id from make_video, write_script or list_videos).
- `shot`: The shot number, counted from 1 as the app shows it (shots[].shot). (action edit_shot, remove_shot, redraw_frame, use_anyway)
- `words`: What is said in it. (action edit_shot, add_shot)
- `seconds`: How long it runs. (action edit_shot)
- `place`: Another place from its shot list (shots[].place). (action edit_shot)
- `place_looks`: How its place looks in this video, in your words. (action edit_shot)
- `framing`: How close: selfie, close, waist, full. (action edit_shot) One of: `selfie`, `close`, `waist`, `full`.
- `camera`: still, push, pan or handheld. (action edit_shot) One of: `still`, `push`, `pan`, `handheld`.
- `doing`: What the character is doing in it. (action edit_shot)
- `at`: The new shot’s number, from 1: 1 puts it first. (action add_shot)
- `kind`: How it plays: talking (said to the camera), narrated or scene. (action add_shot) One of: `talking`, `narrated`, `scene`.
- `note`: What to change ("closer, holding the map"). Optional. (action redraw_frame)
- `quote_id`: Leave out at first: the tool answers needs_confirmation with a quote. After the person says yes, call again with the same arguments and quote.quote_id. Single use and also the idempotency key: retrying with it never charges twice.

## film

**Film a video from its shots.** Films a video from its planned shots (plan_shots first) and cuts it. Spends credits: the first call answers needs_confirmation with the price in credits and about $; show it to the person and ask, then call again with the same arguments plus quote_id only after a yes (a client that can ask in the chat asks there instead). action "film": film the whole video. action "refilm_failed": after a film that failed or stopped partway, films only the shots that did not film: free the first time, quoted after. A shot the video service turned down is changed with plan_shots, not filmed again. Starts a job and returns at once: then wait_for_job.

- `action` (required): film: Film the whole video from its shots (quoted). refilm_failed: Film again only the shots that didn't film (free the first time) One of: `film`, `refilm_failed`.
- `video_id` (required): The video (from list_videos or write_script).
- `quote_id`: Leave out at first: the tool answers needs_confirmation with a quote. After the person says yes, call again with the same arguments and quote.quote_id. Single use and also the idempotency key: retrying with it never charges twice.

## fix_shot

**Fix a shot of a filmed video.** Films one shot of a filmed video again and cuts it back in; the other shots stay (Finish's "Fix this shot"). Its whole seconds, 4.5 credits a second, 4 seconds at least; free once per video when you say what is off (whats_off). Quoted first: show the price, then call again with quote_id after a yes. Starts a job: wait_for_job, then watch it with get_video.

- `video_id` (required): The video (video_id from make_video, write_script or list_videos).
- `shot` (required): The shot number, counted from 1 as the app shows it (shots[].shot).
- `whats_off`: What is off: Lips off, Voice wrong, Face changed, Wrong set, Words wrong, Other. With one, the first fix is free.
- `note`: Anything to change, in a sentence. Optional.
- `quote_id`: Leave out at first: the tool answers needs_confirmation with a quote. After the person says yes, call again with the same arguments and quote.quote_id. Single use and also the idempotency key: retrying with it never charges twice.

## finish_video

**Finish a video: captions, text, end card, music, versions.** Free. The Finish step, over a filmed video (nothing is filmed again): "show" says what is on it now; "captions" (on/off, style, highlight, where; the character’s next videos start in the look you leave); "add_text" / "remove_text" (text on screen); "end_card" (a closing card with a line and the character’s handle; keeping "Made with AI Cast Studio" on it earns 10 credits back once); "music" puts a track made for this video on it, or takes it off (make_music makes one, paid); "versions" lists every saved edit and "restore" takes one back. Every edit that changes something saves a version (changed: false says it already looked like that). Fixing a shot is fix_shot. When the person is happy: set_video_state done.

- `action` (required): show: What is on the video now. captions: Change the captions. add_text: Add text on screen. remove_text: Remove one text on screen. end_card: Add, change or remove the end card. music: Put a track on it, or take the music off. versions: List every saved edit. restore: Take an earlier version back. One of: `show`, `captions`, `add_text`, `remove_text`, `end_card`, `music`, `versions`, `restore`.
- `video_id` (required): The video (video_id from make_video, write_script or list_videos).
- `on`: False removes it. (action captions, end_card)
- `style`: bold, clean or word (word by word). (action captions) One of: `bold`, `clean`, `word`.
- `highlight`: The spoken word’s colour. (action captions) One of: `yellow`, `white`, `orange`.
- `position`: higher, middle or lower. (action captions) One of: `higher`, `middle`, `lower`.
- `text`: What it says (80 characters). (action add_text)
- `start`: When it shows, in seconds (default 0). (action add_text)
- `end`: When it goes (default 3 seconds after start). (action add_text)
- `look`: bold (default), clean, label or typewriter. (action add_text) One of: `bold`, `clean`, `label`, `typewriter`.
- `text_index`: Its place in edit.texts, from 0. (action remove_text)
- `line`: The line on it (44 characters). Needed to add one. (action end_card)
- `handle`: The character’s handle on it (@name). (action end_card)
- `card_look`: dark (default), light or warm. (action end_card) One of: `dark`, `light`, `warm`.
- `tag`: Keep “Made with AI Cast Studio” (default true; earns 10 credits back once). (action end_card)
- `track`: A track from tracks[] (make_music makes one). Leave out to take the music off. (action music)
- `volume`: How loud, 0 to 1 (default 0.5). (action music)
- `version`: The version number. (action restore)

## make_music

**Make music for a video.** Makes a music track for one filmed video, from a mood (Upbeat, Calm, Driving, Playful, Lo-fi) or your words: 30 credits, quoted first (show the price, then call again with quote_id after a yes). Nothing is filmed again; when it is ready it goes on the video if no other music was chosen (finish_video music picks or removes it). Starts a job: wait_for_job.

- `video_id` (required): The video (video_id from make_video, write_script or list_videos).
- `mood`: The mood. Or describe it. One of: `Upbeat`, `Calm`, `Driving`, `Playful`, `Lo-fi`.
- `describe`: The music in your words.
- `quote_id`: Leave out at first: the tool answers needs_confirmation with a quote. After the person says yes, call again with the same arguments and quote.quote_id. Single use and also the idempotency key: retrying with it never charges twice.

## set_video_state

**Done, reopen, archive or unarchive a video.** Free. "done": the video is Finished and locked (no more edits or fixes until reopened) and its 1080p file starts being made; then post it (post_video) or download it. "reopen": back to In progress and unlocked, and OFF the schedule: its scheduled posts are cancelled (the result says how many, and any already being sent). Tell the person that before reopening. "archive": out of the lists, files and past posts kept (refused while a post of it is scheduled). "unarchive": back as it was. Pressing one again changes nothing.

- `action` (required): done: Mark it Done (locks it, makes the 1080p file). reopen: Unlock it to change it (takes it off the schedule). archive: Take it out of the lists. unarchive: Bring it back. One of: `done`, `reopen`, `archive`, `unarchive`.
- `video_id` (required): The video (video_id from make_video, write_script or list_videos).

## list_videos

**A character’s videos.** One character’s videos, newest first, as the character’s page lists them: show "in_progress" (not Done yet), "finished" (Done), "scheduled" (a post on its way), "posted", "archived", or "all" (default; archived left out). Each comes with the line the app shows ("Filming · 2 of 3 shots", "Scheduled Thu 7:00 pm"). Pages: pass next_cursor back as cursor. Free. get_video gives one video in full, with links.

- `character_id` (required): The character (from get_studio).
- `show`: Which videos (default all). One of: `all`, `in_progress`, `finished`, `scheduled`, `posted`, `archived`.
- `limit`: How many to read for this page (default 50).
- `cursor`: next_cursor from the page before.

## get_video

**One video.** Everything about one video: where it is (lifecycle and the app’s status line), its script, its shots with a link to each frame and its state, its versions, and a fresh link to watch it once filmed (no sign-in; it says when it stops working, and anyone with the link can watch it until then). Free.

- `video_id` (required): The video (video_id from make_video, write_script or list_videos).

## get_downloads

**Download a video.** Links to download one video, no sign-in: the finished 1080p file (its newest edit, with music) and the same without music, the video as filmed, the character’s portrait, and everything of the character’s as one ZIP. The finished file is made after Done; while it is being made, finished is null and file_status says rendering: call again in about 15 seconds. Each link says when it stops working; anyone with a link can open it until then. Free.

- `video_id` (required): The video (video_id from make_video, write_script or list_videos).

## connect_account

**Connect Instagram, TikTok, YouTube or Facebook.** Where a character’s videos can post. action "status" (free): each platform and the accounts connected to the character, and any that need reconnecting. action "connect": a link the person opens in a browser where they are signed in to AI Cast Studio; they sign in to the platform there, then choose, on the character’s page, the account the character posts to. Nothing connects until they choose; then call status. Connecting always happens in the browser: never ask for a platform password.

- `action` (required): status: The character’s connected accounts (free). connect: A browser link to connect one platform. One of: `status`, `connect`.
- `character_id` (required): The character (from get_studio).
- `platform`: instagram, facebook, tiktok or youtube. (action connect) One of: `instagram`, `facebook`, `tiktok`, `youtube`.

## post_video

**Post a video to social accounts.** Posts a filmed video to the character’s connected accounts (connect_account). action "draft" (free): three captions in the character’s voice and hashtags (more: true for three new ones). "choose" (free): the caption, hashtags and cover frame that go out; its next is the post call from the saved posting plan (connect_account first when a platform asked for is not connected). draft and choose also take post_to and post_when (in the person’s words) when the plan was not said before: saved on the video. "tiktok_rules": what TikTok requires the person to choose for one TikTok account. "post": now, or at a day and time (in the character’s city unless time_zone says otherwise); every result says the time in the person’s zone and in that city. Before "post", show the person the accounts, the caption and the time, and ask. When the person has "Posts from AI apps wait for my OK" on (the default), the post waits for their OK on Schedule (they get an email); you cannot approve it. Posting is free.

- `action` (required): draft: Captions and hashtags in the character’s voice (free). choose: The caption, hashtags and cover that go out (free). tiktok_rules: What TikTok needs the person to choose. post: Post now or at a day and time. One of: `draft`, `choose`, `tiktok_rules`, `post`.
- `video_id` (required): The video (from list_videos).
- `more`: Three new captions instead. (action draft)
- `post_when`: (action draft, choose)
- `post_to`: (action draft, choose)
- `caption`: Default: the chosen caption and hashtags. (action choose, post)
- `hashtags`: From the offered ones, without #. (action choose)
- `cover`: One of covers[].cover from draft (one per shot frame, at most five), or null for the opening frame. (action choose)
- `account_id`: The character’s TikTok account_id. (action tiktok_rules)
- `account_ids`: The character’s account_ids (connect_account status). (action post)
- `when`: now, or scheduled at date and time. (action post) One of: `now`, `scheduled`.
- `date`: scheduled: the day, yyyy-mm-dd. (action post)
- `time`: scheduled: the time, 24-hour HH:mm. (action post)
- `time_zone`: The zone date and time are in. Default: the character’s city. (action post)
- `title`: YouTube title. (action post)
- `tiktok`: (action post)
- `your_time_zone`: The person’s IANA zone (America/Denver), to say times in. Default: the one the app knows. (action post)

## get_schedule

**What is going out.** Free, read-only. The Schedule as the app shows it: waiting for your OK (posts from AI apps the person has not approved; only they approve, on Schedule, and manage_post can move or cancel the request), didn’t post (with the platform’s reason), check it (not sure it posted), ready to post (finished videos with nothing scheduled or waiting for the OK), the days ahead, and posted in the last days with links to the live posts. Times are in the person’s zone and the character’s city. Change or cancel with manage_post.

- `days`: How far back “posted” goes (default 7).
- `character_id`: Only this character’s posts.
- `your_time_zone`: The person’s IANA zone (America/Denver), to say times in. Default: the one the app knows.

## manage_post

**Move, cancel or retry a post.** One post on the Schedule (post_id and character_id from get_schedule). change_time: a new day and time while it is still scheduled (in the character’s city unless time_zone says otherwise). cancel: it will not go out (say so first). try_again: a post that didn’t post, or that may not have, goes again now (free). archive: takes a settled post (posted, didn’t post, cancelled) off the Schedule. A post waiting for the person’s OK (its post_id from post_video or get_schedule’s waiting_for_you) can be moved (change_time) or cancelled here; only the person approves it, on Schedule.

- `action` (required): change_time: A new day and time. cancel: It will not go out. try_again: Post it again now (free). archive: Take a settled post off the Schedule. One of: `change_time`, `cancel`, `try_again`, `archive`.
- `post_id` (required): The post (get_schedule → post_id).
- `character_id` (required): The character (from get_studio).
- `date`: yyyy-mm-dd. (action change_time)
- `time`: 24-hour HH:mm. (action change_time)
- `time_zone`: Default: the character’s city. (action change_time)
- `your_time_zone`: The person’s IANA zone (America/Denver), to say times in. Default: the one the app knows. (action change_time)

## ideas

**Ideas: accounts to learn from.** The Ideas place: public TikTok, Instagram or YouTube accounts the person learns from. action "list" (free): the accounts added. "get" (free): one account, what works for them and a page of their videos (each with an id for write_script's start-from-a-video). "add" (paid, quoted: 10 credits for 25 videos, 19 for 50, 36 for 100) reads a new account; "refresh" (paid, the same prices) reads their newest. 10 reads a day. A paid action answers needs_confirmation first: show the price, ask, then call again with quote_id.

- `action` (required): list: The accounts added (free). get: One account and its videos (free). add: Read a new account (quoted). refresh: Read an account’s newest videos (quoted) One of: `list`, `get`, `add`, `refresh`.
- `limit`: How many (default and most 24). (action list, get)
- `cursor`: next_cursor from the page before. (action list, get)
- `account_id`: The account (from list). (action get, refresh)
- `sort`: recent (default), top or discussed. (action get) One of: `recent`, `top`, `discussed`.
- `source`: The account link, or an @handle. (action add)
- `platform`: Only for a bare @handle. (action add) One of: `tiktok`, `instagram`, `youtube`.
- `size`: How many of their newest public videos to read: 25 (10 credits), 50 (19) or 100 (36). Default 25. (action add, refresh)
- `quote_id`: Leave out at first: the tool answers needs_confirmation with a quote. After the person says yes, call again with the same arguments and quote.quote_id. Single use and also the idempotency key: retrying with it never charges twice.

## wait_for_job

**Wait for a job.** Waits up to 25 seconds (wait_seconds, at most 45) for a job from any tool, then says where it is: its stage in the app's words, a real count ("2 of 3 shots filmed" while filming, counting only shots that came back; "2 of 3 frames drawn" while drawing frames; else "1 of 4 steps done"), how long it usually takes, and retry_after_seconds. Call it again while status is queued or running. Once it settles, next is always the call to make: a new character's voices to pick (with its character_id), make_video for a video, the watch link for a film or an edit. When a film fails it says what was kept, what was refunded, and the call that fixes it. Free.

- `job_id` (required): The job, from the tool that started it.
- `wait_seconds`: How long to wait for it to settle (default 25, capped at 45). 0 answers at once.

## cancel_job

**Cancel a job.** Stops a job that is queued or running, and says what came back: everything it held is refunded. A job already settled answers its final state and changes nothing. Free.

- `job_id` (required): The job to stop.
