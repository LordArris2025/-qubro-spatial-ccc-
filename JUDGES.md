# Qubro Next-Gen Spatial Command & Control Center (CCC): one page for judges and reviewers

| | |
|---|---|
| Demo video (YouTube, under 3 minutes) | https://youtu.be/LlkcGd_Jt8k |
| Source code | kept in a private repository (available to the organisers on request) |
| Competition | Meta VR Start Developer Competition: Contest 2026. Track: Productivity. Division: New Experience (Conceived of and built within the competition window, after 24 September 2026). Build path: IWSDK (WebXR) |

Qubro is a hands-only WebXR command center for a data-center security team, built with IWSDK for Meta Quest 3 and 3S. You stand in a virtual command room. Four sector AIs (cyber, drones and robots, LiDAR and radar, CCTV and doors) answer your spoken or typed orders from the live state of a simulated building. The AI can never authorize an action: only the operator's hand gesture can.

## 30-second way to try it

- **Headset (best):** open the live link in the headset browser, turn hand tracking on, press **ENTER VR (hands only)**. Pinch a desk button, pull it like stretchy plastic, let go. The hint on the button guides you: PINCH + PULL, then PULL [###--], then LET GO!, then ACTIVATED.
- **Computer:** open the link, drag down to see the desk, click a button. Keys: `1` simulate intrusion, `2` authorize, `3` reset, `K` check all sectors, `T` type an order, `O` the short guided tour TOUR 2 (and the page button **TOUR 1 by Core AI** under ENTER VR, the longer one, about 2 minutes: the Core AI states the plan, introduces itself and the four sector AIs, then hands the demo to you; `Esc` skips). A quiet line at the top of the page lists them.
- **What to ask the AI:** press **GUIDE / HELP** (desk button in VR, key `H` on a computer). It lists example questions for each of the four AIs (firewall, drones and robots, satellite link and radar, CCTV and doors), in English or Malay. No command-center knowledge is needed.
- Each visit has its own simulation, so several reviewers do not affect each other.
- Step-by-step: [docs/USER_GUIDE.md](docs/USER_GUIDE.md).

## What the vision AI (SAM 3.1) does in the simulated intrusion demo

Press SIMULATE INTRUSION (key `1`). A flashing **RED ALERT** banner and a siren start and **stay on until the threat is NEUTRALIZED**. The L34 vault CCTV feed and the battle hologram then show the vision AI at work: amber dashed boxes ("SEGMENTING") turn into solid **red boxes** with "SAM 3.1 ENEMY CONFIRMED" on every spider. The panel above the hologram, "WHAT THE VISION AI DOES", ticks off its five steps: frame in, segment, classify, verdict, hand off. The vision AI only reports; **only the Commander's hand can authorize** the response. The after-action report (written by the AI brain when READ REPORT is touched) opens with a three-line verification chain recorded by code: vision confirmation (labelled SIMULATED), brain plan and report, Commander hand authorization. Honest limit: in this build the detection numbers are a labelled simulation (the vault frame is not yet sent to the model; see [ARCHITECTURE.md section 12](docs/ARCHITECTURE.md#12-known-limitations)).

## Stage 1 rules and where the evidence is

| Rule | Evidence |
|---|---|
| Hands-first: 100 % direct hand tracking, no controllers | The VR session requires the `hand-tracking` feature; every interaction reads tracked hand joints; controller models and rays are hidden and a notice asks you to put real controllers down. See [docs/USER_GUIDE.md](docs/USER_GUIDE.md) "Hands first" and [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) section 6.1 |
| Video link | The table above and the Devpost video field |
| Text description in English | [README.md](README.md) |
| Original work, no third-party brands or real people | The scene, robots, textures and sounds are generated in code (no imported 3D models, images or audio files); the commander is fictional and no real person or brand is shown; see [README.md](README.md) |
| No secrets in the repository | `backend/key.json` is git-ignored; keys are environment variables on the host |
| Documentation | [docs/](docs/README.md): user guide, architecture, API |

## The four criteria and where to look

- **Innovation and creativity:** the pinch-and-pull "stretchy button" interaction with a floating choice and a spoken confirmation; four sector AIs reasoning over one shared live state; an AI that is deliberately unable to authorize. See [README.md](README.md) and [docs/USER_GUIDE.md](docs/USER_GUIDE.md).
- **Experience design:** a seated room built around arm's reach; hover music and synthesised sounds on every button; a desktop fallback (mouse, keyboard) and the Immersive Web Emulator for reviewers without a headset. See "Performance and comfort" in the user guide.
- **Technical implementation:** IWSDK + three.js client in TypeScript; FastAPI backend with a phase state machine, order router, guard rails and per-visitor sessions; the AI model for answers, the voice model for speech input; an in-scene call monitor. See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) and [docs/API.md](docs/API.md).
- **Polish and presentation:** the documentation above, a QR code for the headset, and a live link that needs no install or login.

## Design standards: the four tests

The organisers' requirement and guidelines, each with the test they give and our answer:

| Requirement | Standard | The Test | Qubro's answer |
|---|---|---|---|
| **Requirement** | | | |
| Hands-first | Fully usable with hands end-to-end. Controller support optional. | Can someone complete the entire experience without ever pairing a controller? | **Yes.** The VR session requires hand tracking and no code reads a controller button; controller models and rays are switched off and a notice asks the person to put a real controller down. Every action is a hand pinch-and-pull on a desk button. Typing uses an in-scene touch keyboard; voice is optional. *Tested in an XR emulator; not yet measured on physical Quest hardware.* |
| **Guidelines** | | | |
| Seated-optimized | Designed for seated/stationary use. Limit roomscale or large physical movement. | The "airplane seat test": does every interaction work in a 2 ft radius? | **Yes.** The room is laid out for a seated eye at about 1.25 m. All controls sit on one desk inside a 0.6 m (about 2 ft) reach (farthest button row about 0.53 m). No locomotion or teleport. The tower, report and battle holograms are read-only: nothing is touched at distance. *Distances are from the design and code, not measured on a headset.* |
| Easy to get into/out of | Fast cold start. Clean pause/resume. Satisfying in 10 minutes or less. | The "one bus stop test": can someone have a complete moment in a short ride? | **Yes.** One link, no install, no login. ENTER VR, then simulate an intrusion, authorize and one QUICK ORDER is a complete moment in about a minute. RESET gives a clean start; leaving VR and re-entering restores the desk. An offline demo runs if the backend cannot be reached. *The free backend can need up to a minute to wake on the first visit.* |
| Original, not a wrapper | Core experience is Entrant-built. Thin wrappers around existing platforms or services (YouTube, ChatGPT, etc.) are highly discouraged. | The "take it away test": remove the third-party service. Is there still a Project? | **Yes.** Without the hosted models there are still about 7,300 lines of TypeScript (WebXR client) and about 2,500 lines of Python (phase state machine, order router, guard rails, monitor, per-visitor sessions), a router scored 98.6 % on 74 labelled orders, and a labelled fallback for every model answer. The authorize decision is always a hand gesture. *Free-form answers, written reports and voice orders need the hosted models.* |
| Field of view (judging note) | Evaluators look for FoV-aware design: essential UI inside a comfortable, narrower FoV. | Is essential UI comfortable on Meta VR devices with different fields of view? | **Designed for it, not yet tested on other devices.** The answer bubble sits about 38 degrees left and slightly above eye level; the tower hologram is straight ahead and the report hologram about 36 degrees right, all inside a comfortable central view. The desk controls are below eye level, so you look down at your hands as at a real desk. The four sector screens spread further round the room and are read by turning your head; no control lives on them. *Angles come from the code, not from measurement on a headset.* |

## Meta Code of Conduct for Virtual Experiences

| Code of Conduct area | Qubro's answer |
|---|---|
| Don't deceive users, Meta or developers (fake identity, fraud, deceptive activity) | Nothing pretends to be a real person or organisation. Everything is labelled honestly: all feeds and intrusions are **simulated**, the vision detection is a labelled simulation, and a fallback answer is marked FALLBACK, never passed off as an AI answer. |
| Don't gather or share sensitive personal information; no unauthorised access or malware | No personal data is collected from visitors. Spoken orders are relayed in memory for transcription and no audio is written to disk. No real systems are touched. |
| Nothing illegal, abusive or that could lead to physical harm | No people, no blood, no graphic violence. The simulated intrusion is a scenario on a virtual data center, shown as a small stylised tabletop hologram: red spider-shaped units, defence drones and small robots. On interception the drones hover over the spider units and send electric lightning at them for a few seconds, an EMP ring spreads on the floor, and the units are disabled (they twitch, go still and disappear); the sound is electric (relay clicks, hum, zaps, power-down), with no explosion sound. There is no weapon fire, explosion, fire, debris or injury. The robots are stylised humanoid shapes with a short pulse-emitter tube on the arm; nothing is fired. The authorize decision is always a human hand gesture; voice and the AI can never authorize. |
| Respect intellectual property (no improper use of copyrighted or trademarked material) | The scene is built from procedural shapes (no third-party models); the skyline landmarks are stylised, not replicas. Meta model names appear only in documents; on-screen text is generic so nothing implies endorsement. Third-party libraries are listed in THIRD_PARTY.md. |
| Privacy and user data | See [PRIVACY.md](PRIVACY.md): no account or personal identifiers, no ads or analytics, voice audio not stored, hand data stays in the browser. |
| Public spaces (spam, explicit or excessively violent content) | Not applicable: single-user experience, no multiplayer, no lobby and no user-generated content. Each visitor gets a private simulation. |

## Meta Horizon OS Content Guidelines

| Content Guidelines area | Qubro's answer |
|---|---|
| Excessively violent content (gore, dismemberment); movie R-rating threshold | Far below the threshold. No people, blood or injury; the simulated intrusion is a small stylised tabletop hologram and the hostile units simply disappear. See the harm row above. |
| Regulated goods (firearms, explosives) and weapons | Nothing is sold or promoted. The small robots carry a short pulse-emitter tube (not a gun shape); the interception is a simulated electric pulse that disables the spider units. |
| Hate, harassment, terrorist or organised violent groups | None. No people or groups are represented; the "enemy" is a fictional spider-shaped unit. |
| Sexual content and nudity | None. |
| Illegal activity (real-money gambling, drugs) | None. No payments, no in-app purchases, no currency. |
| Deceptive content, spam, health or medical claims | None. Everything simulated is labelled simulated; no medical claims. |
| Applications directed to children under 10 | Not directed to children. The audience is adults and security professionals (a data-center security command center). |
| User-generated content | None. Single-user experience with no sharing, chat or multiplayer. |

## Compliance table

See also: [PRIVACY.md](PRIVACY.md) (data handling) and [THIRD_PARTY.md](THIRD_PARTY.md) (libraries and licences).

Every contest rule, the evidence for it, and what is still pending: [COMPLIANCE.public.md](COMPLIANCE.public.md).

## Note for reviewers who record the emulator

**Recording tip (emulator panels).** The desktop emulator draws its own panels, "Controller [L]/[R]" and a "Quest 3" device panel. They are emulator UI, not Qubro UI: Qubro has no controller interface. Hide those panels while recording, or say in the video that they belong to the emulator.

**AI models used (disclosure).** The hosted models are Meta's: Muse Spark 1.3 (answers, plans, reports), SAM 3.1 (vision; the demo detection is a labelled simulation) and Muse Voice (speech-to-text). They were used through normal developer access, with no private invitation. The names appear in documents; the on-screen text and the tour use generic names (AI model, vision model, voice model), with one exception since v228: the vision layer on the battle hologram labels its spider tags "SAM 3.1" (still a labelled simulation; the constant `SHOW_MODEL_NAME` in `frontend/src/battle3d.ts` switches it back to the generic name).

## Numbers we measured

- Backend tests: 87 (they run in the private source repository).
- Order router, offline keyword fallback only: 95.0 % sector routing accuracy on 40 hand-labelled English and Malay orders, 100 % on the authorize-word safety flag ([eval/RESULTS.md](eval/RESULTS.md)). This does **not** measure the live AI answers.
- Frame rate: tested in the IWSDK XR emulator on a development laptop, where the scene held 60 FPS in the latest runs, with no dips. Not yet measured on a physical Quest 3 (the emulator's number is the laptop's, not the headset's). An FPS meter is built in but hidden by default (a small panel on the desk; add `?fps=1` to the address to show it). The app asks the headset for 72 Hz.

## Honest limits (also in [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) section 12)

- the vision model is integrated, but in this build the vault camera frame is not yet sent to it, so the detection shown (units, confidence) is a **labelled simulation**.
- All building data (CCTV footage, card events, logs, drones, robots) is simulated. Door control runs in the frontend.
- Only full VR is offered. Passthrough (mixed reality) and spatial anchors are not used.
- Live AI answers and voice need the backend (Render). If it is asleep, cannot be reached or the link drops mid-demo, the page falls back to an offline demo that still finishes the simulated intrusion and, when READ REPORT is touched, gives a template after-action report labelled FALLBACK TEMPLATE (never passed off as the AI's work).
- Security: API keys stay on the host; the WebSocket accepts only the allowed web origins; each visitor has their own simulation and a limit of 30 AI orders per minute; the call log and usage report need an admin token (v270). No penetration test was done, and there is no per-IP rate limit or Content-Security-Policy yet (details in [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) section 12).
- Model answer quality is not benchmarked beyond the router evaluation above.
