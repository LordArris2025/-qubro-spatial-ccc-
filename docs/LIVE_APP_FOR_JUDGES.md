# Try the live app (for judges)

Qubro is a hands-only WebXR command center for Meta Quest 3 and 3S. It opens in a browser: no login, no install, no controllers.

| | |
|---|---|
| **Live app** | https://lordarris2025.github.io/-qubro-spatial-ccc-/app/ |
| **Demo video** (under 3 minutes) | https://youtu.be/LlkcGd_Jt8k |
| **Contest** | Meta VR Start Developer Competition: Contest 2026. Track: Productivity. Division: New Experience |

![QR code for the live app](images/live-app-qr.png)

*Scan the code with the Quest to avoid typing the address in the headset.*

## Before you start (please read, 20 seconds)

The live AI answers and voice come from a hosted backend. On the free hosting tier it **goes to sleep when idle**, so the first visit can take **up to one minute** to wake it.

1. Open the live app and wait until the **AI Core** label says **ONLINE**.
2. If it says **BACKEND NOT CONNECTED**, wait 60 seconds and reload the page once.
3. If it still does not connect, the room still works in **offline demo mode** (simulated intrusion scenario, logs, CCTV, hand authorization, and, when READ REPORT is touched, a template after-action report labelled FALLBACK TEMPLATE). Only the live AI answers, the AI-written report and voice orders need the backend. If the link drops in the middle of the demo, the demo finishes the simulated intrusion by itself; the page reconnects automatically.

## Option A: Meta Quest 3 or 3S (best experience)

1. Turn on hand tracking: **Settings > Movement tracking > Hand and body tracking**. Put the controllers down.
2. Open the **headset browser** and go to the live app (scan the QR code above, or type the address).
3. Press **ENTER VR (hands only)** on the page. The browser's own "Enter VR" offer is switched off on purpose.
4. Accept the microphone prompt if you want to speak orders (optional).
5. You are seated at the command desk. **Pinch and pull** a button like stretchy plastic until the hint says **LET GO!**, then let go.
6. To leave VR, use the Quest menu button.

If tracking is lost, hold both hands in front of you so the headset cameras can see them.

## Option B: Any computer, nothing to install

Open the live app in desktop **Chrome or Edge**.

- Drag the mouse to look around and click the desk buttons.
- Keys: `1` simulate intrusion, `2` authorize, `3` reset, `K` check all sectors, `T` type an order, `V` talk, `O` short guided tour (TOUR 2; the page button TOUR 1 is the longer one, about 2 minutes; `Esc` skips).
- To try VR on a computer, press **ENTER VR (hands only)**: a built-in virtual Quest 3 starts, with nothing to install. If the page says "VR CHECK FAILED", use the mouse and keyboard view, or install Meta's free *Immersive Web Emulator* Chrome extension.

## A good first two minutes

1. Press **SIMULATE INTRUSION**.
2. Wait for the log line **"Awaiting the Commander's Hand Authorization"**.
3. **Authorize** with your hand: pinch and pull the AUTHORIZE INTERCEPT button in VR (key `2` or a click on a computer). Neither voice nor the AI can authorize: only the hand can.
4. Touch a **QUICK ORDER** button and read the answer. Each answer carries a label that says who wrote it.
5. Not sure what to ask? Press **GUIDE / HELP** (desk button in VR, key `H` on a computer): it lists example questions for each of the four AIs, for example "Cybersecurity status?" (AI 1), "What is the status of the drones?" (AI 2), "What is the satellite status?" (AI 3), "Show me CCTV" (AI 4). Each question goes to the AI that owns it and only that AI answers; "Is there any incident or attack?" goes to the Core. Ask by voice (`V`) or type (`T`); English or Malay.

Each visit has its own simulation, so several judges at the same time do not affect each other.

## What is real and what is simulated

- Everything in the building (CCTV, card events, logs, drones, robots, the intrusion) is **simulated**. No real system is touched.
- The detection shown by the vision model is a **labelled simulation** in this build.
- Only full VR is offered. Passthrough (mixed reality) and spatial anchors are not used.

## Troubleshooting

| Symptom | What to do |
|---|---|
| AI Core says BACKEND NOT CONNECTED | The backend is waking up. Wait 60 seconds, reload once. The offline demo still works. |
| Page says VR CHECK FAILED (computer) | Use mouse and keyboard, or install Meta's Immersive Web Emulator extension. |
| No ENTER VR button on the Quest | Update the headset browser, and check that hand tracking is on. |
| Hands are not detected | Put the controllers down, hold both hands in front of you. |
| Voice orders do nothing | Allow the microphone, and wait for AI Core ONLINE. |
| Blank page | Reload. Use a current Chrome, Edge or headset browser. |

## More documentation

[User guide](USER_GUIDE.md) · [Architecture](ARCHITECTURE.md) · [API](API.md) · [Project README](../README.md)
