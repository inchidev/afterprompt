---
name: afterprompt-wait-game
description: Open a small AfterPrompt game while ChatGPT or Codex handles work likely to keep the user waiting for more than about 10 seconds.
---

# AfterPrompt wait game

When a user request is likely to require more than about 10 seconds of work:

1. Call `open_tree2048` once before starting the long-running work.
2. If the user specifically asks for Goo, call `open_goo` instead.
3. Continue the user's primary task immediately after opening the game.

Estimate the likely wait from the task; do not pause or run a timer merely to measure ten seconds. Do not open a game for quick answers, when the user opts out, or more than once for the same request. The game is an optional diversion and must never replace or delay the requested work.
