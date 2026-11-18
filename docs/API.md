# API reference

The backend (`backend/main.py`, FastAPI) exposes REST endpoints, one state WebSocket (`/ws`) and one audio WebSocket (`/voice`). CORS is open (`*`) because the frontend is served from another origin during development. Run it with `uvicorn main:app --host 0.0.0.0 --port 8000`.

Back to [architecture](ARCHITECTURE.md) · [README](../README.md)

## REST endpoints

| Method | Path | Purpose |
|---|---|---|
| GET | `/api/state` | Current snapshot (same object as the `state` WebSocket message) |
| GET | `/api/ai-status` | Online or offline state of the AI model, the vision model, the voice model. Read only, never calls the model provider |
| POST | `/api/ai-check` | **Admin (v270).** One explicit probe of the three links. Rate limited to one round per 60 s. Returns `{probed, ai}` |
| POST | `/api/simulate-attack` | Start the simulated intrusion. `{ok}` is false unless the phase is UNLOCKED |
| POST | `/api/authorize?gesture=button` | Hand authorization. `{ok}` is false unless the phase is STANDBY_AUTH |
| POST | `/api/reset` | Back to UNLOCKED, clears detection, plan, report and history |
| GET | `/api/report` | Latest report as JSON, or `{"error": "no report yet"}` |
| GET | `/api/report.md` | Latest report as a Markdown download (404 if none) |
| POST | `/api/report/generate` | **Admin (v270).** Ask the AI model for a status report now |
| GET | `/report/` | Printable report page |
| GET | `/monitor/`, `/monitor/tx` | Live model-call monitor and transaction list pages |
| GET | `/api/monitor/summary` | Counts, latency, tokens and health of every link. Public; since v270 it carries no order text and no model answers (admin callers still get them) |
| GET | `/api/monitor/tx` | **Admin (v270).** Transactions. Query: `kind`, `status`, `q`, `op`, `limit` (default 400) |
| GET | `/api/monitor/tx/{id}` | **Admin (v270).** One transaction with its redacted request and response |
| POST | `/api/monitor/clear` | **Admin (v270).** Delete stored transactions |
| GET | `/api/monitor/report` | **Admin (v270).** Markdown usage report of all model calls |
| GET | `/api/sysinfo` | v188: which machine runs this backend: platform (Render / own machine), CPU, cores, RAM, GPU, OS, Python. No hostname, user or address. Shown in DEBUG LOG |
| GET | `/api/diag` | Why a link is down: uptime, clients, which keys are present, config error. Never returns key values |

**Admin endpoints (v270).** Marked *Admin* above. They answer 403 unless the request carries the header `X-Admin-Token: <value of the ADMIN_TOKEN environment variable>` (or `?token=`), or comes straight from the same machine (local development, `run.sh`). If ADMIN_TOKEN is not set on the host, remote callers always get 403. The VR scene and the live demo never call them.

## WebSocket `/ws`

**Sessions (v145).** Connect as `/ws?sid=<id>` (and `/voice?sid=<id>`, `GET /api/monitor/summary?sid=<id>`, `GET /api/state?sid=<id>`). The `sid` is 8 to 64 characters of `A-Z a-z 0-9 _ -`; the web page makes a random one per page load. Each sid gets its **own simulation** (phase, log, report, history), made on first use and forgotten after 300 s (`SESSION_IDLE_S`) with nobody connected; at most 60 are kept (`MAX_SESSIONS`). A connection **without** a valid sid uses the one shared session (curl, tests, the REST calls below). The monitor's call history (`/api/monitor/...`) is shared by all sessions.

**AI order limit (v146).** One session may send at most `AI_PER_MIN` (default 30) AI orders per minute (`say`, `report`, `sweep`, `check_ai`); beyond that the server answers `{"type":"ack","ok":false,"reason":"rate_limited"}` and adds a log line, so one visitor or agent cannot drain the shared model quota. `simulate_attack`, `authorize` and `reset` are never limited.

On connect the server sends one `state` message, then pushes updates to every client of that session.

### Client to server

Send JSON with a `cmd` field. The server answers each known command with `{"type": "ack", "cmd": "<cmd>", "ok": true|false}`.

| `cmd` | Extra fields | Effect |
|---|---|---|
| `simulate_attack` | | Start the scenario (only from UNLOCKED) |
| `authorize` | `gesture`: `air_pinch`, `palm_thrust` or `button` | Hand authorization (only from STANDBY_AUTH). Since v268 the app sends `button` only; `air_pinch` and `palm_thrust` are still accepted so old clients and tests keep working |
| `reset` | | Reset the scenario |
| `say` | `text` (max 300 chars), `via`: how the order was entered, for example `text` or `voice` | Submit an order to the Core AI |
| `report` | | Generate a report now |
| `check_ai` | | Probe the three model links |
| `sweep` | | Run a sweep now, with the tour and the voice |

### Server to client

| `type` | Fields | When |
|---|---|---|
| `state` | `phase`, `avatar_mode`, `detection`, `plan`, `log` (last 50 lines), `sources`, `ai`, `brain`, `report` | On connect and after every change |
| `log` | `line`, `ts` | A new log line |
| `thinking` | `stage` (`start` or `done`), `id`, plus `source`, `ms`, `model`, `reason` on `done` | The AI model started or finished reasoning |
| `voice` | `text`, `heard_via`, `ai`, `name`, `role`, `reply`, `scope`, `source`, `badge`, `ms`, `model`, `why`, `fallback_reason`, `authorize_blocked` | The Core AI answered an order |
| `report` | `report` | A report was produced |
| `sweep` | `n`, `source` (`muse` or `fallback`), `model`, `ms`, `risk`, `all_clear`, `headline`, `lines` (`building`, `satellite`, `robots`, `cyber`), `tour`, `speak`, `interval`, `fallback_reason` | The AI model swept the center by itself (the last one is also in `state.sweep`) |
| `ack` | `cmd`, `ok` | Reply to a command |

Fields that matter most:

- `phase`: `BOOT`, `UNLOCKED`, `INCURSION`, `STANDBY_AUTH`, `INTERCEPTING` or `NEUTRALIZED`.
- `sources`: `{"sam": "simulated" | "live", "muse": "simulated" | "meta"}`: where the last detection and plan came from.
- `ai`: link state for `muse`, `sam` and `voice`: `ONLINE`, `OFFLINE`, `ERROR` or `CHECKING`, with model name and detail.
- `voice.source`: `muse` (a model wrote the reply), `fallback` (fixed template) or `rule` (deterministic rule). `badge` is its display label: AI BRAIN, FALLBACK TEMPLATE or SAFETY RULE.
- `voice.scope`: `sector`, `core` or `out` (refused as off-topic).

### Example: ask a question

```json
{"cmd": "say", "text": "Is the SOC seeing any attack on the network?", "via": "text"}
```

Typical `voice` message in reply (shortened):

```json
{"type": "voice", "ai": 0, "name": "AI 1", "role": "CYBER SOC", "scope": "sector",
 "source": "muse", "badge": "AI BRAIN", "reply": "No attack on the network. Edge firewall and IDS/SIEM are online.", "authorize_blocked": false}
```

### Real log source (frontend hook)

The terminals under the station screens show simulated Linux logs. The frontend accepts a message of this shape on `/ws` and shows it in the matching terminal instead of a simulated line:

```json
{"type": "syslog", "stream": "auth", "line": "sshd[2635]: Accepted publickey for ops", "lvl": 0}
```

`stream` is one of `card`, `camera`, `l24`, `robot`, `guardian`, `pdrone`, `lidar`, `radar`, `auth`, `siem`; `lvl` is 0 to 3. The handler is in `frontend/src/logs.ts`. The backend does not emit this message today; a deployment that has a real log source would broadcast it from `backend/main.py` through the hub.

## WebSocket `/voice`

Audio relay to the voice model. The API key stays on the server.

1. Server sends `{"type": "ready", "asr": true|false}` (`asr` is false when no voice key is configured).
2. Client sends binary frames of 16 kHz mono PCM16 audio.
3. Server sends `{"type": "transcript", "text": "...", "final": false}` while the person speaks.
4. Client sends `{"cmd": "end"}` when finished.
5. Server sends `{"type": "transcript", "text": "...", "final": true, "via": "meta"}`, or `"error": "<reason>"` on failure.

The final text is then submitted with `{"cmd": "say"}` on `/ws`.
