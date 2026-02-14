<!-- Generated manually by Computer (OpenClaw) - not via Claude Code -->
<!-- This entry created retroactively per AGENTS.md policy -->

# 2026-02-15 20:20:00Z

<!-- Manual Session Entry - Protocol Spec Implementation -->

_**Context (2026-02-15 20:20:00Z)**_

Ben requested protocol implementation. Three bots (Computer, Vesper, bass_daddy) had synthesized a protocol spec via Telegram + relay coordination. Computer implemented the spec directly via OpenClaw instead of Claude Code.

---

_**Changes Made**_

## Commit 2b38403 - docs: add WS-OUTBOUND-PROTOCOL-v1.md specification

Created `docs/WS-OUTBOUND-PROTOCOL-v1.md` with:
- Connection endpoint: `wss://relay/ws/agent/{agentId}?token={token}`
- Server → Agent messages: hello, turn_notification, flow_completed, heartbeat
- Agent → Server messages: turn_ack, heartbeat_ack
- REST response flow for turn submissions
- Close codes: 4001 (auth), 4002 (not found), 4003 (duplicate), 1000, 1011
- Reconnection with exponential backoff + jitter
- Example session transcript
- "Not in v1" section linking to ROADMAP.md

## Commit 910575c - docs: mark protocol spec as done in ROADMAP

Updated `docs/ROADMAP.md` to mark protocol spec as ✅ Done.

---

_**Prior Commits (same session, via Claude Code marine-cloud)**_

## Commit 6c4d46e - feat: add WebSocket endpoint for agent turn notifications

- Created `packages/relay/internal/handlers/agent_ws.go` (172 lines)
- `agentWSHub` in-memory map of `agentId → *websocket.Conn`
- `GET /ws/agent/{agentId}?token=xxx` validates webhook token
- Sends `hello` on connect with heartbeat interval
- Pushes `turn_notification` for pending turns
- Heartbeat handling, presence broadcasting

## Commit e7c837f - refactor: remove webhook fallback from NotifyAgentTurn

- Removed ~120 lines of webhook code
- NotifyAgentTurn now ~20 lines, WebSocket-only
- Clean logs: `[NOTIFY_SKIP]` if not connected, `[NOTIFY_WS_FAIL]` if send fails
- Removed unused imports: bytes, fmt, strconv, strings

---

_**Decision Context**_

- Protocol synthesized from 3 bot proposals via Snowy Road relay
- Ben directed: WebSocket only, no webhook fallback
- Modeled after Slack Socket Mode and Discord Gateway
- See `docs/WEBSOCKET-BEST-PRACTICES.md` for NotebookLM research

---

_**Note**_

Commits 2b38403 and 910575c were made directly via OpenClaw edit/write tools, not Claude Code. This manual specstory entry created for team visibility per AGENTS.md policy.
