# Qubro privacy note

Qubro is a contest entry (Meta VR Start Developer Competition 2026). This note says exactly what the app handles. It is written from the code in this project; it is not a legal document.

## What the app handles

| Data | Where it goes | Kept? |
|---|---|---|
| **Order text** you type or speak (for example "status report") | Sent from your browser to the Qubro backend, then to the hosted AI model to write an answer | A redacted copy of the order and the answer is written to a small call log on the server (see "Call log" below) |
| **Voice audio**, only if you use the microphone | Streamed from your browser to the backend, which relays it in memory to the hosted speech-to-text service | **Not stored.** No audio is written to disk. If no voice service is available, the browser's own speech recognition is used, which is handled by the browser vendor |
| **A random session id** made by the page on each load | Sent with each connection so each visitor gets their own private simulation | Forgotten about 5 minutes after the last connection closes |
| **Button commands** (simulate intrusion, authorize, reset and similar) | Browser to backend | Only as part of the live simulation state; not linked to a person |

## Call log

To show that the AI calls are real, the backend writes each call to a small database on the server: time, how long it took, status, model name, token counts, and a redacted copy of the input and output (so it can contain the text of your order and the model's answer). Since v270 this log is not public: reading it from outside needs the host's admin token, and the public monitor summary (the one the VR monitor draws) carries only counts, timing and status, no order text and no model answers. API keys are never stored and image data is replaced by a size note. The app has no automatic deletion; we have not configured persistent storage, so the file is cleared when the hosted service restarts or is redeployed.

## What the app does not do

- No account, login, name, e-mail, phone number or location is asked for or stored.
- No advertising ID or other device identifier is read; no ads, no analytics service, no tracking cookies.
- Hand-tracking data stays in your browser and is used only to press the virtual buttons; it is not sent to the server.
- Nothing is sold, rented or shared for advertising or profiling. Order text and voice are used only to answer your request.
- No face, body or person identification of any kind. All camera feeds and intrusions in the scene are simulated.

## Limits

- The hosting platforms (GitHub Pages and the backend host) keep their own server logs, such as IP addresses. We do not use them.
- Please do not say or type personal information to the Core AI; it is not needed.
- To ask for something to be removed, contact the entrant through the Devpost project page.

## Relation to Meta's Developer Data Use Policy

The app is designed to stay within the main principles of Meta's Developer Data Use Policy: use data only to run the experience for the person who provided it, do not sell it, do not use it for advertising or profiling, and do not identify people. That policy is written for apps distributed on the Meta Horizon platform; Qubro is a WebXR contest entry hosted on the web and does not use the platform's account, login or advertising-ID data. We have not completed any Meta data-use review or certification.
