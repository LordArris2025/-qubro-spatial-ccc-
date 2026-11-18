# Third-party notices

All code below is used under its open-source licence. No third-party images, 3D models or audio are bundled: every visual in the scene is generated procedurally.

| Component | Use | Licence |
|---|---|---|
| IWSDK (`@iwsdk/core`, `@iwsdk/vite-plugin-iwer`) | WebXR framework, hand tracking, Quest 3 emulator | MIT |
| three.js (`super-three` fork) | 3D rendering | MIT |
| Vite, `@vitejs/plugin-basic-ssl` | dev server / build | MIT |
| TypeScript | type checking | Apache-2.0 |
| FastAPI | backend API + WebSocket | MIT |
| Uvicorn | ASGI server | BSD-3-Clause |
| websockets | WebSocket client (voice) | BSD-3-Clause |
| HTTPX | HTTP client for model calls | BSD-3-Clause |

Please verify exact versions against `frontend/package-lock.json` and `backend/requirements.txt` before release.
