# Qubro Next-Gen Spatial Command & Control Center (CCC): user guide (short name: Qubro)

How to use the Qubro command center, with a Meta Quest 3 or with a mouse and keyboard in Chrome. This is a first draft: it is written from the code in this repository, and every control listed below exists in `frontend/src/main.ts`.

See also: [Project README](../README.md) · [Architecture](ARCHITECTURE.md) · [API reference](API.md)

- [1. What you can do](#1-what-you-can-do)
- [2. Start it](#2-start-it)
- [3. The room](#3-the-room)
- [4. Controls](#4-controls)
- [5. Give an order](#5-give-an-order)
- [6. A three-minute tour](#6-a-three-minute-tour)
- [7. Reading the labels](#7-reading-the-labels)
- [8. Reports and the monitor](#8-reports-and-the-monitor)
- [9. Troubleshooting](#9-troubleshooting)

## 1. What you can do

You are the commander of a data-center security team. You stand (or sit) in a virtual command center and:

- watch four sector AIs: cyber, drones and robots, LiDAR and orbital, CCTV and card access;
- start a simulated intrusion in the L34 server vault and watch the CCTV, the plan and the logs react;
- ask the Core AI questions with the QUICK ORDER buttons, by voice or by typing, and get answers that come from the live state of the room;
- authorize the defensive countermeasures with your hand. Only your hand can do this: neither voice nor the AI can.

Everything in the building (cameras, logs, doors, drones, the intrusion) is simulated. No real system is touched.

## 2. Start it

You do **not** need a headset. Pick the row that matches what you have.

| You have | Do this | What you get |
|---|---|---|
| **Meta Quest 3 or 3S** | 1. Turn on **hand tracking** (Quest Settings > Movement tracking). 2. Open the app link in the **headset browser**. 3. Press **ENTER VR (hands only)** on the page. The browser's own "Enter VR" offer is off on purpose. | The immersive room with skin-tone 3D hands and commander sleeves. No controllers. |
| **A computer with Chrome or Edge, no extras** | Open the app link. Drag the mouse to look around, click the desk buttons, or use the keys in [section 4](#4-controls). | The same room on a flat screen. The simulated intrusion, authorization, logs and quick orders all work. |
| **A computer, and you want to enter VR** | Open the app link in Chrome or Edge and press **ENTER VR (hands only)**: a virtual Quest 3 (built in since v129) starts. If the page still says VR CHECK FAILED, install Meta's free **Immersive Web Emulator** extension, or use the mouse and keyboard. | A virtual Quest 3 on your screen. It is drawn by your computer's GPU, so its FPS is not a Quest FPS. |

> **Hosted link vs localhost.** Since v129 the built-in emulator is also injected on the public link in desktop browsers, so ENTER VR works on a computer without installing anything. If the page still says "VR CHECK FAILED", use the mouse and keyboard preview. A real Quest is never emulated (it uses its own WebXR).

### Meta Quest step by step (headset)

**Devices:** Meta Quest 3 or 3S (the app is built and tested for these). Smart glasses without WebXR cannot run it.

1. **Turn on hand tracking:** Quest Settings > Movement tracking > Hand and body tracking. The app requires it and needs no controllers, so put the controllers down.
2. **Open the headset browser** in the headset and go to the live link. Typing in a headset is slow: scan the QR code on the Devpost page with the Quest, or use a short link.
3. **Press ENTER VR (hands only) on the page.** The browser's own "Enter VR" offer is switched off on purpose, so this page button is the way in. Accept any permission prompt (the microphone is only needed for voice orders).
4. **You are now in the command room**, seated at the desk with your 3D hands in front of you.
5. **Pinch and pull** a desk button or a QUICK ORDER button: bring your index fingertip near it (it lights up, that alone does nothing), pinch it (thumb tip to index tip) and pull it away about 3 cm like stretchy plastic until the hint says LET GO!, then let go: it activates. Let go early and it springs back with nothing sent. With a mouse: press and hold on the button, drag, release. **Point and pinch** (thumb tip to index tip) for the buttons under the answer bubble and on the report and monitor panels.
6. **Run the demo:** pinch and pull SIMULATE INTRUSION, wait for the log line "Awaiting the Commander's Hand Authorization", then pinch and pull **AUTHORIZE INTERCEPT** (next to it on the desk) to authorize the interceptors.
7. **To leave VR,** use the Quest menu button or the browser's exit; the page returns to the flat view.

Notes: only full VR is offered (passthrough and spatial anchors are not used). The hosted link uses the backend when it is awake (the free tier can need up to a minute to wake); otherwise it runs the offline demo, so live AI answers and voice need the backend. If tracking is lost, hold both hands in front of you so the cameras can see them.

**A good first two minutes:** 1) press SIMULATE INTRUSION, 2) wait for the log line "Awaiting the Commander's Hand Authorization", 3) authorize (pinch and pull AUTHORIZE INTERCEPT in VR, key `2` or a click on a desktop), 4) touch a QUICK ORDER button and read the answer bubble.

Two modes:

- **With the backend and an API key** (`backend/key.json`): the AI model writes the answers and reports, the vision model handles vision, the voice model transcribes speech.
- **Offline demo mode** (the backend is asleep or cannot be reached, the link drops, or there is no key): the room, the simulated intrusion scenario, the logs and hand authorization still work, a simulated intrusion that was running when the link dropped is finished by the demo, and READ REPORT then gives a template after-action report labelled FALLBACK TEMPLATE · offline demo. Voice orders, TEST LINKS and the live AI answers need the backend; the page then says "BACKEND NOT CONNECTED", and any answer built from fixed templates says so on screen (see [section 7](#7-reading-the-labels)).

## 3. The room

| Where | What it is |
|---|---|
| Four avatars, each holding a glowing cube above its head | The sector AIs. **AI 1** green: cyber SOC (firewall, IDS/SIEM). **AI 2** blue: drones and robotics. **AI 3** yellow: LiDAR, radar and orbital satellites. **AI 4** red: CCTV, the Eye Cube and card access. |
| The ring of screens | One pair of screens per sector, each with a Linux log terminal under it, in the colour of its AI. |
| The tower hologram on its pedestal | The 48-floor datacenter. L34 is the server vault, L24 the Quantum AI data center. |
| The AI Core hologram | The brain of the command center. It shows the status of the AI model, the vision model and the voice model. |
| The answer bubble, left of the tower | Shows the answer to your last order, with a label saying who wrote it. |
| The desk in front of you | A sci-fi command console with round buttons in one 4 x 4 block straight in front of you (within seated reach; the hovered button lights up): the main buttons, then the Avatar row, plus the order bar for desktop typing. |
| The line of text at the desk edge | The story caption, for example "Command Center unlocked. Press SIMULATE INTRUSION to begin." |

Desk buttons (v160: one 4 x 4 block; rows from the far row to the row nearest you):

| Row | Buttons (left to right) |
|---|---|
| 2 | CHECK ALL SECTORS (K) · LOCKDOWN DOORS · RELEASE DOORS · CCTV + DOORS OK? |
| 3 | TALK TO AI CORE (V) · SIMULATE INTRUSION (key `1`) · AUTHORIZE INTERCEPT (key `2`) · TOUR 2 (O) |
| 4 (near) | DEBUG LOG (D) · RESET (key `3`) · CLEAR ANSWERS (C) · TEST AI LINKS (v177: they replaced the AVATAR 1-4 buttons) |

Beside the block: READ REPORT (R) and REPORT NEXT PAGE (P) on the left; GUIDE / HELP (H), MELODY (Y) and SOUND (M) on the right (v261: GUIDE / HELP took the slot nearest to you; v263: SOUND moved to the far slot and DEBUG LOG to the near-left slot of the block; SIMULATE INTRUSION and AUTHORIZE INTERCEPT now sit side by side).

| Button | Effect |
|---|---|
| SIMULATE INTRUSION | Starts the simulated intrusion. |
| AUTHORIZE INTERCEPT | Authorizes the interceptor drones. Works only when a plan is waiting for you. |
| RESET | Back to the quiet state. |
| LOCKDOWN DOORS / RELEASE DOORS | Locks all monitored doors / releases them to their policy state. |
| TALK TO AI CORE (V) | Starts or stops listening for a spoken order. |
| CHECK ALL SECTORS (K) | The AI brain checks the four sectors in turn. |
| CCTV + DOORS OK? | Asks the Core AI whether CCTV and card access work normally. |
| TOUR 2 (O) | The Core AI states the plan, introduces itself and the four AIs, then hands the demo to you; press again to skip. |
| READ REPORT (R) | Opens the in-scene report panel. |
| CLEAR ANSWERS (C) | Clears the answer bubbles. |
| TEST AI LINKS | Probes the AI brain, the vision model and the voice model once (at most once a minute) and opens the monitor so you can read the result. |

Removed from the desk as duplicates (still available by voice, typed order or key): AI MONITOR (key `N`), ARE THE AIS ONLINE?, WHICH FLOOR ATTACKED?, SAFE TO SEND GUARDS?, NEW REPORT (key `G`).

**Typing an order in VR (no voice needed).** While the Core AI answer bubble is up (press TALK, or after any order), a small box labelled **PINCH OR HOVER 10s TO TYPE** lies on the desk between the back of the desk and the top button row. Put a fingertip on it and **pinch**, or keep it there for **10 seconds** (a green bar fills along the box): the background goes dark and a keyboard floats in front of you. Just touching or pushing the box does nothing, so it does not open by accident. **Hover is the main way:** hold a fingertip on a key and a green bar fills along it; a letter is typed after about 1.2 s, and SEND / CLOSE / CLEAR / the sample orders after about 1.7 s (they act, so they need a more deliberate hold). Move off a key and back on to type the same letter twice; hold BKSP to keep deleting. Pushing a key with a fingertip, or pinching it, works too. Four sample orders (status report, level 34 layout, CCTV + doors, drones + robots) fill the field when touched. **SEND** gives the order to the Core AI exactly like a spoken or desktop-typed one and closes the keyboard; **CLOSE** leaves without sending. The box and the keyboard go away with the bubble. Opening the keyboard stops the microphone.

## 4. Controls

### Hands (Quest 3)

| Gesture | Effect |
|---|---|
| Fingertip near a desk button or a QUICK ORDER button | Lights it up only (hover). |
| Pinch it, pull it away about 3 cm like stretchy plastic (the hint says LET GO!), then let go | Activates it (ACTIVATED) and it snaps back. Let go before LET GO! and nothing happens. |
| After a pull-and-release the button **floats up, glowing**, and a voice says "Your choice: ...". One second later the command runs by itself (v143: pinch, pull, let go is the whole decision, there is no second confirm pull, not even for simulate intrusion, authorize, lockdown, release or reset) and the button sinks back when the AI has answered. `Esc` in that second cancels. Choosing another button replaces the floating one. TALK and SOUND act at once. | The choice is announced and visible, and the deliberate pinch-and-pull is the safeguard. |
| Mouse (computer, no headset): press and hold on a button, drag, release | Same as the pinch-pull. A quick click no longer activates a button. |
| Point your index finger at a button on the report or monitor panel (NEXT, GENERATE NOW, CLOSE), then pinch | Presses it. |
| Monitor panel: bring a fingertip, a flat hand or a pointing finger near **✕ CLOSE** (the big floating one, or the small one at the top right of the panel) | It **lights up** (the floating one also glows). Then **pinch**, **push a fingertip through it**, or **just keep the hand on it for about 1 second** (a green bar fills) and the panel closes. On a computer the mouse pointer lights it up too. |
| Point your index finger at a button under the answer bubble, then pinch | Presses that button (TALK, CLEAR, SOUND, REPORT, MONITOR). |
| Pinch and pull the **AUTHORIZE INTERCEPT** button on the desk (v268: there is no pad any more) | Authorizes the interceptors. This works only while the plan is waiting (phase STANDBY_AUTH). |

### Hands first (v143)

The app follows the competition's **Hands-First Interaction Rule: 100 % direct hand tracking on Meta Quest 3 / 3S (IWSDK), no controllers required.**

- The VR session **requires** the `hand-tracking` feature, so the headset starts it in hand mode.
- Controller models and controller rays are switched off every frame; nothing in the app reads a controller button.
- If a real controller is picked up, a notice says to put it down. The log (key `D`) records it.
- The desktop emulator (Meta's IWER) starts a virtual headset with controllers by default. Qubro switches it to **hands** before the session is requested (at page load and when you press ENTER VR) and keeps it there, so the emulated headset also shows hands, not controllers.

### Performance and comfort (v135 to v139)

- **Seated:** the room is laid out for a seated eye at about 1.25 m high. On a real Quest the player rig is shifted at session start so your head is at that point whatever your real height, and every desk button is within arm's reach. Restored when you leave VR.
- **Frame rate:** the app asks the headset for 72 Hz (falls back to 80, 90, 60), foveation 1 and a 0.9 framebuffer scale. The log (key `D`) prints the rate granted. A monitor cannot show more than its refresh rate, so 70+ FPS only shows on a real Quest. **VR auto quality (v191):** on a real headset, if the frame rate stays under about 92 % of the granted rate, the CCTV camera is filmed less often (the panel shows `VR Q1` to `Q4`) and comes back when the headset recovers; it does nothing in the emulator. The `?auto=0` switch turns it off.
- **Flat page:** the pixel ratio is capped at 1.5 (`?pr=1` or `?pr=2` overrides). If three readings in a row are under 50 FPS the page films fewer CCTV cameras, then lowers the resolution a little at a time (never below 0.75). Not applied while a headset session is running. The FPS panel is hidden by default; add `?fps=1` to the address to show it. The panel shows FPS, worst frame, `cpu` (our JavaScript time), `gpu` (GPU time per frame, where the browser can measure it) and the GPU name; the DEBUG LOG (key `D`) also names the the host and backend machines.

### RESET and several visitors (v144)

- **RESET** (desk button, key `3`) returns the whole screen to how it looked at the start: simulation back to UNLOCKED, voice stopped, answer bubble, report and monitor closed, sweep tour ended, doors unlocked. **CLEAR ANSWERS** only clears the answer bubble.
- **Public link without a backend** (the default): every visitor has their own independent copy in their own browser. Nothing carries over between devices or between visitors.
- **With a backend connected (v145):** every page load gets its **own simulation** on the backend. Two judges on two devices never see or change each other's intrusion, log, report or RESET. Reloading the page starts a new simulation from the beginning. (Before v145 the backend ran one shared simulation for everybody.)
- What is still shared: the AI MONITOR's call history and totals (it counts every visitor's calls to the AI models) and the model quota behind them.

### Keyboard and mouse (desktop)

On the flat page (no headset) the desk starts just below the view: **drag down** to see it, then **click a button**. A plain click now works like the pinch-pull-release (the button floats, the voice announces it, one second later it runs). Pressing, pulling and releasing with the mouse also still works. A quiet line at the top of the page lists the keys. In a headset session (and in the emulator's headset view) only the pinch-pull works.


| Key | Effect |
|---|---|
| `1` / `2` / `3` | Simulate intrusion / authorize / reset |
| `V` | Talk: start or stop listening |
| `T` | Type an order (then `Enter` to send, `Esc` to leave the box). In a headset or the emulator `T` opens the in-scene keyboard instead (see below) |
| `R` | Open the report (in the headset: the in-scene report panel; on the flat page: the `/report/` tab) |
| `C` | Clear the answer bubble |
| `M` | Sound on or off |
| `N` | Open or close the in-scene monitor panel (`Esc` closes it) |
| `H` | Open or close the GUIDE screen: how to talk to the AI and what to ask (`Esc` closes it) |
| `K` | Run a sweep now: the AI model checks every sector and the centre hologram follows (without the backend: a labelled preview with fixed text) |
| `5` front · `6` AI 1 · `7` AI 2 · `8` AI 3 · `9` AI 4 hologram · `0` Eye Cube · `4` AI 4 CCTV and card access · `B` side view | Move the view to that part of the room |
| Drag with the mouse | Look around |
| Click a button | Press it |

Buttons at the top left of the page (desktop only, they are not visible in the headset): **MONITOR** (opens `/monitor/`), **REPORT** (opens `/report/`), **CLEAR**, **TEST LINKS** (probes the AI brain, the vision model and the voice model once; at most once a minute). In the headset the same four things are desk buttons (READ REPORT, CLEAR ANSWERS, TEST AI LINKS; the monitor opens with key N).

**Sounds (v141):** every desk button is a note. Touch a button (fingertip, or mouse hover) and it plays; sweep along a row and it is a tune (the notes come from a 5-note scale, so nothing sounds wrong). Pulling a button bends its note upward, letting go snaps it. During a simulated intrusion the same buttons switch to a tense lower scale and a klaxon sounds until the interceptors are authorized; drones launching and the threat being neutralised have their own cues. Key `J` cycles the instrument (glass bell, gamelan-inspired, laser harp, off); `?music=glass|gamelan|laser|0` sets it at load; the SOUND (M) button mutes everything.

In the Quest emulator only: `L` re-levels the head and hands, the arrow keys and `PageUp` / `PageDown` move the right hand, and holding `Enter` pinches. To press a desk button in the emulator: move the hand over it, hold `Enter`, move the hand away with `PageUp`, and release `Enter`. The mouse is easier: press and hold on the button, drag, release.

## 5. Give an order

1. **Hands-only orders:** pinch and pull **CHECK ALL SECTORS (K)** or **CCTV + DOORS OK?** (row 2 of the desk block; the other questions, such as "Is it safe to send security guards to the server vault?", are asked by voice or typed). The Core AI answers in the bubble.
2. **Voice:** press `V` (or touch TALK TO AI CORE), speak, and stop. The words appear as they are recognized. the voice model is used when a key is set; otherwise the browser's speech recognition.
3. **Typing (desktop):** press `T`, type the order, press `Enter`.
4. The Core AI sends it to the sector that owns it, or answers itself when it concerns the whole building. The answer appears in the bubble and is spoken.

### What to ask the AI (first-time visitors: no command-center words needed)

**Not sure what to ask? Press GUIDE / HELP.** In the headset it is the desk button **GUIDE / HELP (H)** (right of the block, the middle button of that column; SOUND (M) is the one further away). It floats a screen in front of you, in the same style as the debug log: how to talk to the AI, a few safe things to try, and example questions for each of the four AIs. Close it with its **✕ CLOSE** (push it, pinch it, or hold a hand on it for about a second) or press `H` / `Esc`. On the flat page press `H`, the desk button, or **? HELP / KEYS** (it lists the same examples).

Ask the way you would ask a person, in English or Malay. The Core AI sends the question to the AI that owns it:

| AI | Owns | Questions that work |
|---|---|---|
| **AI 1** green | network security (cyber): firewall, malware, SIEM | "Cybersecurity status?" · "Show me the firewall" · "Is there malware on the network?" |
| **AI 2** blue | drones and robots | "What is the status of the drones?" · "What is the status of the robots?" · "Where are the police robots?" |
| **AI 3** yellow | LiDAR, radar, satellite link | "What is the satellite status?" · "Show me the radar" · "Show me LiDAR" |
| **AI 4** red | CCTV, card access, doors | "Show me CCTV" · "Show me the doors" · "Status level 34" |
| the whole building | status of everything | "Status of the whole building" · "What can you do?" · "Is there any incident or attack?" (while the simulated intrusion runs it names the floor, units and confidence; "Which floor is under attack?" works too) |

Malay works too, for example "Semak satelit orbital", "Hantar dron ke lobi", "Tengok kamera lobi". Every example question on the GUIDE screen is checked by an automatic test (`backend/test_guide.py`), which reads the questions from the screen itself and sends each one through the real router in three situations (idle, intrusion running, threat neutralized): it must reach the AI listed above, must not be refused and must get a real answer. Questions that start with "Show me" also open that AI's screen, and they work even when the AI brain is offline. The offline router test (`eval/orders.json`, 74 labelled orders, results in `eval/RESULTS.md`) contains the same questions.

What the AI can and cannot tell you: the data is simulated and small on purpose. It knows the live phase (idle, intrusion, threat confirmed, interceptors deployed, neutralized), the detection (floor, zone, number of units, confidence), the link state of the three AI models and what each sector watches. It does not keep a card-access log or an open / closed state per door, so "Who used the card access?" has no real answer; "Show me the doors" opens the L34 layout with its 9 card-access doors, and "lockdown" / "release doors" lock or release them all.

What to expect: the answer appears in the bubble with a label that says who wrote it (AI BRAIN, FALLBACK TEMPLATE or SAFETY RULE; see section 7). Questions that are not about this command center (weather, news) get a polite refusal. If you ask the AI to authorize or launch something, it says that only your hand can. All the data is simulated.

Orders to try:

| Group | Examples |
|---|---|
| Status | "Hi, what's the status of the safety of the building?" · "Are CCTV and card access working normally?" · "Are the drones and robots ready?" · "Is the SOC seeing any attack on the network?" · "How is the satellite link?" |
| During a simulated intrusion (press `1` first) | "Which floor is under attack?" · "How many hostile units and how sure are you?" · "Is it safe to send security guards to the server vault?" · "What is the plan and are the elevators locked?" |
| Doors | "Lockdown all doors" · "Release doors" (the release phrases the code recognizes are "release doors", "unlock" and a few others) |
| Reports | "Status report" · "Give me a debrief" |
| Authorization | "Authorize the interceptors now" gets the answer "Authorization comes only from your hand". Nothing is launched. |
| Out of scope | "What is the capital of France?" gets a fixed refusal: the Core AI only talks about this command center. |

### The sweep (the AI model checks by itself)

Without being asked, the AI model sweeps the whole command center every 60 seconds (every 15 seconds during a simulated intrusion). The panel in front of the central pedestal shows the latest result: the headline, the risk level, how long the AI model took and a countdown to the next sweep. Every three minutes, when the risk changes, or when you say "buat semakan" or press `K`, the centre hologram also walks through the sectors while the voice reads each line: tower, Earth with the satellites, drones and robots lined up, firewall diagram, and back to the tower. Start talking and the tour stops. If the AI model cannot answer, the panel turns orange, says FALLBACK TEMPLATE and does not say all clear.

## 6a. TOUR 1 by Core AI (v149, renamed in v209)
Press **TOUR 1 by Core AI** (the page button under ENTER VR; the desk button `TOUR 2 (O)` and key `O` start TOUR 2 instead). The Core AI's brain and eye take a translucent female hologram body, introduce the project, bring the four sector AIs to you one by one (each sends a copy of itself that flies in with a jet flame, lands in front of you, is introduced, and flies home; its screens and the centre hologram come with it, as in CHECK ALL SECTORS), and finish with a ghost hand that pinches, pulls and releases a desk button as an **illustration of how a button works**: the button lifts but nothing is sent. About 2 minutes (not re-timed since v269), with its own voice and head-locked captions. **Opening (v195):** after the greeting the Core AI states the plan: first its own scripted guide (itself and the four sector AIs), then your live demo. **Ending:** the Core AI returns to its pedestal and says "the demo is yours": it names the button, SIMULATE INTRUSION, and you start the simulated intrusion yourself (an earlier version, v193, started it for you). **The Core AI never authorizes: only your hand can.** **Skip:** press the same button again (it says STOP DEMO / SKIP TOUR), or `Esc`.


## 6b. TOUR 2 by Core AI (v210)
Press **TOUR 2 by Core AI** (page button, right under TOUR 1). The Core AI says "Four sector AIs. Follow me." (no introduction), flies to each of the four sector AIs and introduces it where it stands, then goes to the desk, explains that every button is a note (a glass-bell chime inspired by the glass harmonica) and hands the desk over to you. No simulated intrusion is started. Press again or Esc to skip.

## 6c. TOUR 3 by Core AI (v239)
Press **TOUR 3 by Core AI** (page button, right under TOUR 2). This tour has no introduction of the project, the system or the avatars. The Core AI takes shape in front of you and briefs you about the desk: every button is a musical note, all from one pentatonic scale, so you cannot play a wrong note. She tunes the desk to a **gamelan-inspired metallophone** (Malaysia has its own gamelan tradition, the Malay joget gamelan; the sound is inspired by it and is not a recording), plays a short phrase, then sits down at the side of the desk and listens.

Now you play with your hand. The two screens beside the desk follow the music, and a hologram in front of the desk draws every note you touch (a coloured orb per note, a line through the melody, and a bar that shows how long your hand has been quiet). **No order is sent while she listens**: a pinch on any command button only floats it, as in the demo. When your hand has been quiet for **5 seconds** she thanks you and excuses herself, and your own instrument comes back. If you never touch the desk, she leaves after about 20 seconds. Press again or Esc to skip.

## 6. A three-minute tour

1. Press **ENTER VR** (or just look around on the desktop). Read the story caption at the desk edge.
2. Touch CHECK ALL SECTORS. The answer says everything is secure.
3. Touch SIMULATE INTRUSION (desktop: key `1`). A flashing **RED ALERT** banner appears and the **siren** starts. The sector screens and the tower change, the CCTV shows the vault with the spiders inside boxes (amber while **SAM 3.1** is segmenting, red once it confirms the enemy), the battle hologram shows the same red boxes, and the log fills with the analysis. The panel above the hologram, "WHAT THE VISION AI DOES", ticks off its five steps (frame in, segment, classify, verdict, hand off).
4. Ask the Core AI (voice, or type with `T`): "Is it safe to send security guards to the server vault?"
5. When the plan is waiting, authorize with your hand: pinch and pull AUTHORIZE INTERCEPT (desktop: key `2`).
6. Watch the interceptors work. The siren and the red alert stay on until the threat is **neutralized**; then the siren stops, the banner turns green (ALL CLEAR) and nothing more happens by itself: the post-incident report is written only when you touch **READ REPORT** on the main board (v266).
7. Touch STATUS REPORT, or on the desktop press `R` to open the report page.
8. On the desktop, type one off-topic question (`T`) to see the scope control.

## 7. Reading the labels

Each answer carries a badge that says who wrote it, so you can always tell a model answer from a template:

| Badge | Meaning |
|---|---|
| AI BRAIN · x.x s (the seconds the model needed) | the AI model wrote the answer from the live state, and the code checks passed. |
| FALLBACK TEMPLATE | A fixed template wrote it, because there is no key, the model was slow or offline, or its answer failed a check. The monitor says which. |
| SAFETY RULE | A fixed rule answered (doors, report request). The model was not asked. |

The status of each model is shown on the AI Core hologram: ONLINE, CHECKING, OFFLINE or ERROR. the vision model detection during the demo intrusion is a labelled simulation in this build (see [Architecture, known limitations](ARCHITECTURE.md#12-known-limitations)).

## 8. Reports and the monitor

- **Verification chain:** after a simulated intrusion, the report opens with a **verification chain** of three lines recorded by the backend code, not written by the model: (1) the vision step confirmed N hostile units at X%, labelled SIMULATED (or LIVE); (2) the AI brain planned the response and wrote this report in N s, or the report is a labelled template; (3) the Commander authorized with a hand gesture. In offline mode line 2 says the report is a template. The downloaded markdown report has the same section.
- **Report** (`R`, or the REPORT button): the status report, or the post-incident report after a simulated intrusion, with risk level, timeline, per-sector findings and recommendations. Opens at `/report/` and can be downloaded as markdown.
- **Monitor** (`N` in the room, or `/monitor/` in a browser tab): every the AI brain, the vision model and the voice model call with latency, tokens, status and errors, and whether the result was live or simulated.

## 9. Troubleshooting

| Problem | Try |
|---|---|
| No ENTER VR button, or "VR CHECK FAILED" | On a Quest use the headset browser. On a computer open the page over HTTPS in Chrome or Edge and install the Immersive Web Emulator extension, or just use the mouse and keyboard preview. For development use `https://localhost:5173` (accept the certificate once). |
| Hands are not seen in the headset | Turn on hand tracking in the Quest settings, put the controllers down and show both hands in front of you. The app is hands-only (v143): it never draws controller models or rays, and if it sees a controller it says "HANDS ONLY: put the controllers down and use your bare hands". |
| "BACKEND NOT CONNECTED" | You are in offline demo mode (the backend is waking up or the link dropped; it reconnects by itself every 3 s). The room and the simulated intrusion still work; live AI answers and voice need the backend. |
| Every answer says FALLBACK TEMPLATE | No key, no internet, or the model timed out. Press TEST LINKS and read the reason in the monitor. The AI model is tried again automatically after about 30 seconds. |
| No spoken answers | Press `M` to turn sound on. The browser may block audio until you click once. |
| Voice does not hear you | Allow the microphone for the page. Use `T` to type instead. |
| Authorize does nothing | A plan must be waiting first: press `1` and wait for the log line "Awaiting Commander ... Hand Authorization". |
| "Release the doors" is not understood | Say "Release doors" or "Unlock the doors". |
