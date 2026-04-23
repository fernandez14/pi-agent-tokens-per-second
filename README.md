# tokens-per-second.ts — Pi Extension

Real-time token generation speed display for [pi](https://github.com/mariozechner/pi-coding-agent). Shows `tok/s` and total output token count in the footer status bar while streaming.

## What it does

- **During streaming:** Shows current average throughput (e.g., `42 tok/s · 128`) updated every 250ms
- **After completion:** Shows final stats in green (e.g., `38 tok/s · 256 in 6.7s`)
- **Auto-clears** 4 seconds after the agent finishes

## Install

The easiest way is via `pi install`:

```bash
pi install https://github.com/fernandez14/pi-agent-token-stats
```

Or copy manually into your pi extensions directory:

```bash
cp tokens-per-second.ts ~/.pi/agent/extensions/
```

For project-local use:

```bash
mkdir -p .pi/extensions
cp tokens-per-second.ts .pi/extensions/
```

Reload pi with `/reload` (or restart). The status bar will show token speed during the next assistant response.

## How it works

Subscribes to `message_start`, `message_update`, and `message_end` events for assistant messages. Reads `usage.output` from the partial message for accurate token counts — no estimation needed.

## Events used

| Event | Purpose |
|-------|---------|
| `agent_start` | Reset state, clear status |
| `message_start` | Record start time on first assistant message |
| `message_update` | Update TPS display every 250ms during streaming |
| `message_end` | Show final stats with elapsed time |
| `agent_end` | Clear status after 4s delay |
