# AfterPrompt for Claude Code and Codex

When your agent has been working for more than a minute, a small game window opens in the corner of your screen. When the agent finishes or needs your permission, the window says so and closes itself after five seconds. It never closes while you're dragging a blob, and "keep playing" keeps it open.

Quick answers never open anything: if the agent is done within the delay, no window appears.

## Install

**Claude Code**

```
/plugin install afterprompt --marketplace inchidev/afterprompt
```

**Codex macOS app**

1. Open **Plugins** in the Codex macOS app and select **Add Marketplace**.
2. Add `inchidev/afterprompt`, then open **AfterPrompt** and select **Install**.
3. When the app shows that setup needs attention, select **Finish**, review the five AfterPrompt hooks, and trust them.
4. Start a new task.

These GitHub marketplace installs work before directory review. A public release appears in the shared Plugins Directory after OpenAI review.

For updates, Claude users can run `/plugin update afterprompt@inchidev` or enable auto-update for the `inchidev` marketplace under **Plugins → Marketplaces**. Codex users can upgrade the `inchidev` marketplace, restart the app, and accept the new plugin version.

Nothing else to install: the hooks are a plain shell script that uses only `sh`, `curl`, and `sed`, which come with macOS and Linux. Windows is not supported yet.

## How it works

| Claude Code hook | Codex hook | What happens |
| --- | --- | --- |
| `UserPromptSubmit` | `UserPromptSubmit` | Starts a background timer (async hook: the agent is never delayed). |
| after the delay | after the delay | Creates a session and opens `…/goo?session=…&popup=1` as a chromeless Chrome, Edge, Brave, or Chromium `--app` window, in the top-right corner. Without a Chromium browser, it opens a normal tab. |
| `Stop`, `Notification` (`permission_prompt`) | `Stop`, `PermissionRequest` | Completes the session. The window shows "claude is ready" or "codex is ready", counts down, and closes. On macOS the dedicated browser profile quits once the window is gone. |
| `SessionEnd` | `SessionEnd`, `Interrupt` | Completes any open session. |

State lives in `~/.afterprompt/`: one small file per agent session, a log, and a separate browser profile that keeps your game progress. Your everyday browser profile is never touched.

**Privacy:** only the selected game ID, agent name (`claude` or `codex`), and timestamps are sent to `afterprompt.inchi.dev`. Prompts, code, file paths, and transcripts never leave your machine.

## Settings

Set these environment variables, for example in the `env` block of `~/.claude/settings.json`, or in your shell profile for Codex:

| Variable | Default | Meaning |
| --- | --- | --- |
| `AFTERPROMPT_DELAY` | `60` | Seconds of work before the window opens |
| `AFTERPROMPT_GAME` | `goo` | ID of the game to open |
| `AFTERPROMPT_DISABLE` | — | `1` turns the plugin off without uninstalling |
| `AFTERPROMPT_BROWSER` | auto | Path to a Chromium-family browser binary |
| `AFTERPROMPT_URL` | `https://afterprompt.inchi.dev` | Game server (for local development) |
| `AFTERPROMPT_HOME` | `~/.afterprompt` | Where state, logs, and the browser profile live |

Ask Codex to “open the Goo AfterPrompt practice game” at any time, or pass another installed game with `--game its-id`.
