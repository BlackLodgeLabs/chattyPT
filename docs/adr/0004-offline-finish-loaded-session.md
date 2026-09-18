# Offline gym can finish the loaded session only

If the backend is unreachable, a WOD or plan session already on the device can still be viewed and have results typed; those results queue and sync when the network returns. Generating a new WOD or minting the next plan session requires the backend.

We rejected full offline-first (conflict, replica, generate without the model) and online-only (losing scores when wifi dies mid-session).
