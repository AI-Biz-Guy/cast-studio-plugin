---
name: cast-studio-setup
description: Install, sign in to, check or repair AI Cast Studio in this AI app (Claude Code, Codex, Cursor, Claude.ai, ChatGPT). Use when someone pastes the AI Cast Studio setup prompt, asks to connect AI Cast Studio, or its tools answer 401 or insufficient_scope.
---

<!-- Generated from apps/api/src/domain/agent-setup.ts in the AI Cast Studio repo by `pnpm plugin`. Edit the source, not this copy. -->

# Set up AI Cast Studio

Set up AI Cast Studio in this AI app. Do every step you can yourself; the person does only the one sign-in step, and you tell them exactly what it is.

## 1. Install

Find which app you are (and its version), then follow only its section. If AI Cast Studio is already installed, update it instead of installing twice; keep every other integration as it is.

### In Claude Code

**Install.** Run these yourself (check the client’s --help first if a command is refused):

- Add the AI Cast Studio marketplace and install the plugin (in a terminal, or as /plugin commands inside Claude Code):

  ```
  claude plugin marketplace add AI-Biz-Guy/cast-studio-plugin
  claude plugin install cast-studio@cast-studio
  ```

- Load it into the running session:

  ```
  /reload-plugins
  ```

**Sign in (the person’s one step).** Tell them, word for word: “Type /mcp, pick cast-studio and choose Authenticate. Your browser opens AI Cast Studio: enter the code we email you, then press Allow.” Then wait for them to say it is done.

**Their skills:** `/cast-studio:cast-studio`, `/cast-studio:cast-studio-setup`, `/cast-studio:make-a-video`, `/cast-studio:whats-going-out`.

**If it goes wrong:**

- /reload-plugins says 0 skills: That is normal: it only counts commands. Run /cast-studio:cast-studio to check the skills loaded.
- The skills or the server do not show up: Run /plugin list (bare /plugin opens a panel). If it is missing, install again; if it is there but stale, update it as below and restart Claude Code.
- It says the marketplace already exists (an older install): Run claude plugin marketplace remove cast-studio, then add AI-Biz-Guy/cast-studio-plugin again.
- Tools answer 401, or the server shows as needing sign-in: Sign in again (the sign-in step above). A sign-in lasts until you disconnect the app in Settings → Connected apps.
- A tool answers insufficient_scope: This app was not allowed to spend credits or post. Sign in again and allow it on the consent page.
- The code email does not arrive: Check spam for support@aicaststudio.com, wait a minute, then ask for a new code on the same page.
- A tool answers agent_daily_limit: AI apps have a daily credit limit you set in Settings → Connected apps. The app cannot raise it; you can.

### In Codex

**Install.** Run these yourself (check the client’s --help first if a command is refused):

- Add the AI Cast Studio marketplace and install the plugin:

  ```
  codex plugin marketplace add AI-Biz-Guy/cast-studio-plugin
  codex plugin add cast-studio@cast-studio
  ```

- Restart Codex so it loads the server and the skills.

**Sign in (the person’s one step).** Tell them, word for word: “In a terminal, run codex mcp login cast-studio. Your browser opens AI Cast Studio: enter the code we email you, then press Allow.” Then wait for them to say it is done.

**Their skills:** `$cast-studio`, `$cast-studio-setup`, `$make-a-video`, `$whats-going-out`.

**If it goes wrong:**

- codex mcp login says cast-studio is not found: Restart Codex first: a plugin’s server registers only after a restart. Then run the login again.
- Sign-in fails with a missing issuer: Update Codex to the latest version and sign in again.
- A codex plugin command is not recognised: Run codex plugin --help: the subcommand names have changed between versions.
- Tools answer 401, or the server shows as needing sign-in: Sign in again (the sign-in step above). A sign-in lasts until you disconnect the app in Settings → Connected apps.
- A tool answers insufficient_scope: This app was not allowed to spend credits or post. Sign in again and allow it on the consent page.
- The code email does not arrive: Check spam for support@aicaststudio.com, wait a minute, then ask for a new code on the same page.
- A tool answers agent_daily_limit: AI apps have a daily credit limit you set in Settings → Connected apps. The app cannot raise it; you can.

### In Cursor

**Install.** Run these yourself (check the client’s --help first if a command is refused):

- Add the server to ~/.cursor/mcp.json (keep any servers already there):

  ```
  {
    "mcpServers": {
      "cast-studio": {
        "url": "https://app.aicaststudio.com/mcp"
      }
    }
  }
  ```

- Add the skills:

  ```
  git clone --depth 1 https://github.com/AI-Biz-Guy/cast-studio-plugin ~/.cursor/cast-studio-plugin
  mkdir -p ~/.cursor/skills && cp -R ~/.cursor/cast-studio-plugin/skills/* ~/.cursor/skills/
  ```

**Sign in (the person’s one step).** Tell them, word for word: “Open Cursor Settings → MCP, find cast-studio and press Connect. Your browser opens AI Cast Studio: enter the code we email you, then press Allow.” Then wait for them to say it is done.

**Their skills:** `/cast-studio`, `/cast-studio-setup`, `/make-a-video`, `/whats-going-out`.

**If it goes wrong:**

- cast-studio shows red or no tools in Cursor Settings → MCP: Check the URL is exactly the one above, toggle the server off and on, then press Connect again.
- Tools answer 401, or the server shows as needing sign-in: Sign in again (the sign-in step above). A sign-in lasts until you disconnect the app in Settings → Connected apps.
- A tool answers insufficient_scope: This app was not allowed to spend credits or post. Sign in again and allow it on the consent page.
- The code email does not arrive: Check spam for support@aicaststudio.com, wait a minute, then ask for a new code on the same page.
- A tool answers agent_daily_limit: AI apps have a daily credit limit you set in Settings → Connected apps. The app cannot raise it; you can.

### In Claude.ai and Claude Desktop

**Install.** The person does this part in the app; you cannot. If tools are not available in this chat yet, give them these steps and stop:

- Open Customize → Connectors (Settings → Connectors in older versions), press + and choose Add custom connector. Name it AI Cast Studio and paste this URL:

  ```
  https://app.aicaststudio.com/mcp
  ```

- On a Team or Enterprise plan, an owner adds it once under Organization settings → Connectors.

**Sign in (the person’s one step).** Tell them, word for word: “Press Connect on AI Cast Studio. Your browser opens AI Cast Studio: enter the code we email you, then press Allow. Then turn it on for the chat from the + menu.” Then wait for them to say it is done.

**Their skills:** just ask in the chat.

**If it goes wrong:**

- Claude does not use the tools: Turn AI Cast Studio on for this chat from the + menu (Connectors).
- Tools answer 401, or the server shows as needing sign-in: Sign in again (the sign-in step above). A sign-in lasts until you disconnect the app in Settings → Connected apps.
- A tool answers insufficient_scope: This app was not allowed to spend credits or post. Sign in again and allow it on the consent page.
- The code email does not arrive: Check spam for support@aicaststudio.com, wait a minute, then ask for a new code on the same page.
- A tool answers agent_daily_limit: AI apps have a daily credit limit you set in Settings → Connected apps. The app cannot raise it; you can.

### In ChatGPT

**Install.** The person does this part in the app; you cannot. If tools are not available in this chat yet, give them these steps and stop:

- Open Settings → Apps (Connectors in some versions) → Advanced settings and turn on Developer mode. Then press Create app, name it AI Cast Studio, choose OAuth and paste this URL:

  ```
  https://app.aicaststudio.com/mcp
  ```

- It needs a Plus, Pro, Business or Enterprise plan, on the web.

**Sign in (the person’s one step).** Tell them, word for word: “Press Connect when ChatGPT asks. Your browser opens AI Cast Studio: enter the code we email you, then press Allow. In a chat, pick AI Cast Studio from the + menu.” Then wait for them to say it is done.

**Their skills:** just ask in the chat.

**If it goes wrong:**

- ChatGPT does not offer the tools: Pick AI Cast Studio from the + menu in the chat; Developer mode must stay on.
- Tools answer 401, or the server shows as needing sign-in: Sign in again (the sign-in step above). A sign-in lasts until you disconnect the app in Settings → Connected apps.
- A tool answers insufficient_scope: This app was not allowed to spend credits or post. Sign in again and allow it on the consent page.
- The code email does not arrive: Check spam for support@aicaststudio.com, wait a minute, then ask for a new code on the same page.
- A tool answers agent_daily_limit: AI apps have a daily credit limit you set in Settings → Connected apps. The app cannot raise it; you can.

Any other MCP client: add the server `https://app.aicaststudio.com/mcp` (Streamable HTTP, OAuth sign-in) the way that client adds remote servers.

## 2. Check it works (free)

Once the tools load, call `get_studio` and `list_videos`. Both are free. Never spend credits, start a film or post during setup. Never say it works before both calls succeed; if one answers 401, give the sign-in step again. An API key is only for scripts and CI: do not make one or ask for one here.

## 3. Reply in 140 words or fewer

Use this shape and nothing more:

- **Ready** (or what blocked it and the one fix).
- Her characters by name with each one’s status line, and the credits left (from `get_studio`). No characters yet: say the first thing to try is making one.
- Four things to try, in this app’s own way of starting a skill: make a video (`make-a-video`), what is going out this week (`whats-going-out`), ask the studio guide anything (`cast-studio`), and connect her Instagram or TikTok now so posting works later.
- One line each: Finish: the step after filming: captions, text on screen, end card, music, or fixing one shot. Done: locks a filmed video and makes its 1080p file; only a Done video posts. Reopen undoes it. Look: what she wears and how she is framed in one video; her face and voice stay the same.
- “Skip the API key unless you run scripts.”

If the person says yes to connecting Instagram or TikTok, call `connect_account` and give them the link it returns to open in their browser.
