# Canonical records live in the app backend

Client profiles, equipment, gym space, agreed WODs, workout plans, plan sessions, results, and API keys live in one app-owned store. Phones authenticate into it; the floor display uses its pairing. A browser's localStorage is not the source of truth.

We rejected Notion as the store (gym logging is not a second-brain write), per-device copies synced by export, and splitting settings from history across two places.
