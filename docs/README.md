# Qubro Next-Gen Spatial Command & Control Center (CCC): documentation (short name: Qubro)

Live app: https://lordarris2025.github.io/-qubro-spatial-ccc-/app/ · Demo video: https://youtu.be/LlkcGd_Jt8k · Source: private repository · For judges: [../JUDGES.md](../JUDGES.md)

| Document | What it covers |
|---|---|
| [../COMPLIANCE.public.md](../COMPLIANCE.public.md) | Contest rule to evidence table, with the items that are still pending |
| [../PRIVACY.md](../PRIVACY.md) | What data the app handles, what it keeps and what it does not do |
| [../JUDGES.md](../JUDGES.md) | One page for reviewers: each rule and criterion, and where the evidence is |
| [USER_GUIDE.md](USER_GUIDE.md) | How to use Qubro in VR (Quest 3 or desktop): the room, controls, orders, a three-minute tour, troubleshooting |
| [ARCHITECTURE.md](ARCHITECTURE.md) | System design: components, diagrams, runtime flows, how the three AI models are used, safety rules, failure modes, known limitations |
| [API.md](API.md) | REST endpoints, the `/ws` and `/voice` WebSocket protocols, message formats |
| [../eval/RESULTS.md](../eval/RESULTS.md) | Offline evaluation of the order router |

Diagram images (generated from the Mermaid sources inside `ARCHITECTURE.md`) are in [images/](images/).

Suggested reading order for a reviewer: the README overview, then the [user guide](USER_GUIDE.md), then [ARCHITECTURE.md](ARCHITECTURE.md) sections 1 to 5, then [API.md](API.md) if you want to drive the backend yourself.
