# Personal trainer product

Status: ready-for-agent

Source: platform grill (shared understanding) plus `CONTEXT.md` and ADRs 0001–0004. The POC harness (`docs/specs/personal-trainer-harness.md`) is existing code to evolve, not a blank folder.

Vocabulary: `CONTEXT.md`. Do not say **trainer** for the person using the app; that person is a **user**.

## Problem Statement

Two people share a home gym and need to generate sessions, look at a board, and log scores from a phone or a gym screen. The POC lets a single browser chat the harness into a narrative workout or a plan, with no sign-in and with everything stuck in that browser. Agreed work is not saved. Scores are not logged. Old WODs cannot be retested. Strength plans do not become a next session. Partner work does not account for the room. The other person has no identity, and the gym display cannot be trusted as an unlocked full app.

## Solution

One web app that Craig and Gillian both use in full. Phones sign in (lock plus persona). A paired floor display skips sign-in, runs the full app with no me, and logs results into the participant slots of the session on screen. Canonical records live in the app backend. Home defaults to me and can switch to the other client or both. A WOD is one click once client context is known, shown as a glanceable whiteboard; coaching notes stay collapsed. An agreed WOD and its results go to workout history and can be retested. A workout plan is still agreed in conversation, then saved with a cycle length; only the next plan session exists. If the backend is unreachable, a session already on screen can still take results, which queue and sync later.

## User Stories

1. As a user, I want to sign in as myself (Craig or Gillian), so that the app knows who me is and a random browser cannot open it.
2. As someone with the app URL and no pairing, I want to be blocked until I sign in, so that API keys and client records are not on an unlocked public page.
3. As a signed-in user, I want home, next plan session, profile, and a new WOD to start on me, so that I am not picking myself every time.
4. As a signed-in user, I want to switch to the other client or both, so that I can still coach or log the other person.
5. As a signed-in user, I want full access to settings, API keys, the other profile, the other plan, and workout history, so that neither of us is a client portal.
6. As a user, I want a person to be both a user and a client, so that signing in does not replace the coaching record.
7. As a user, I want to pair a floor display, so that the gym screen can skip sign-in.
8. As someone on a paired floor display, I want the full app with no me, so that we can generate, plan, and log without a phone and without pretending someone is signed in.
9. As someone on a paired floor display, I want to pick one or more clients before a new WOD or prompt when nothing is on screen, so that client context is still known.
10. As someone on a paired floor display, I want a result to fill a participant slot of the session on screen, so that I am not asked “who am I?” after a partner WOD is already up.
11. As a user, I want the same client profiles, agreed WODs, workout plans, plan sessions, results, equipment, gym space, and API keys on my phone, the other user’s phone, and the floor display, so that we are not copying files.
12. As a user, I want those records to survive reload on another signed-in session, so that the backend—not this browser’s storage—is the source of truth.
13. As someone on a floor display, I want generation to use the saved API keys from the backend, so that the gym screen can call the model without re-entering keys.
14. As a user, I want to enter, save, and change API keys and harness configuration in the UI, so that nothing for this app lives in environment files.
15. As a user, I want to save and edit each client profile as sentence text, so that prompts receive the same words I saved.
16. As a user, I want to save and edit equipment as sentence text, so that WOD and plan prompts can include what we have.
17. As a user, I want to save and edit gym space as sentence text, so that partner work can respect the room.
18. As a signed-in user, I want one-click WOD generation once me (or my switched selection) is known, so that I do not write a prompt on the gym floor.
19. As someone on a floor display, I want one-click WOD generation after clients are selected (or a session is already up), so that one click still means generate, not skip picking who is training.
20. As a user, I want the generated WOD as a whiteboard with warmup, workout, and cooldown, so that I am not reading a narrative.
21. As a user, I want that whiteboard simple enough to read once and remember, so that I am not glancing at the board the whole time.
22. As a user, I want WOD formats inspired by Girl / Hero / CrossFit.com-style sessions, so that variety is familiar CrossFit programming rather than random circuits.
23. As a user, I want per-client scaling on the board to be at most one line each, so that a partner WOD stays one session.
24. As a user, I want how-to, things to note, and other explanation as coaching notes collapsed behind the whiteboard, so that the board stays glanceable.
25. As a user, I want to expand those coaching notes when I choose, so that the explanation is available without living on the board.
26. As a user, I want a one-client WOD prompt payload to include that client profile sentence text and the saved equipment sentence text, so that the session fits that person and what we have.
27. As a user, I want a partner/group WOD prompt payload to include every selected client profile, equipment, and gym space, so that two people can work in the same room.
28. As a user, I want WOD generation to refuse to start when no client context is known, so that a conversation never runs without profiles.
29. As a user, I want to regenerate or send a short follow-up before I agree, so that I can tweak the whiteboard without writing a brief.
30. As a user, I want generating or regenerating not to create workout history, so that drafts do not pollute agreed work.
31. As a user, I want to save a WOD only via an explicit agree/save, so that an agreed WOD is a deliberate record.
32. As a user, I want an agreed WOD tagged with date and participating clients, so that I can find who did what and when.
33. As a user, I want workout history to list agreed WODs, so that I can open one and see the whiteboard, participants, and date.
34. As a user, I want to log scores, times, weights, and notes against an agreed WOD, so that the session has a result.
35. As a user, I want logging to succeed even if I dismiss a stale-profile prompt, so that a three-month-old profile does not block scores.
36. As a user, I want to be prompted to review a participating client profile if it has not been saved for three months after I log a result, so that stale profiles do not keep driving programming.
37. As a user, I want a later one-click WOD for those clients to attach recent results, so that scaling follows demonstrated capability.
38. As a user, I want to retest an agreed WOD from history, so that the whiteboard is the same session again.
39. As a user, I want a retest to keep the earlier result and show previous vs new, so that I can compare.
40. As a user, I want a WOD with a result older than about two months surfaced as due for retest, so that I do not hunt history by hand.
41. As a user, I want a retest to be a new result on the same agreed WOD, so that I do not get a duplicate history row.
42. As a user, I want a planning conversation for a workout plan (focus, duration, cycle length, gym space), so that the program is agreed before it is saved.
43. As a user, I want that conversation to keep selected client profiles, equipment, and gym space in the prompt payload, so that unknown-plan chat stays grounded.
44. As a user, I want to save a workout plan only via an explicit agree/save, so that chatting does not create a plan.
45. As a user, I want a saved workout plan to store a configurable cycle length, so that the program has a stated horizon.
46. As a user, I want saving a workout plan to create exactly one plan session (the next immediate session), so that future weeks do not exist yet.
47. As a signed-in user, I want one click from home to open the next plan session for me, so that I am not hunting a calendar.
48. As a signed-in user, I want one click from home to open the next plan session for the other client after I switch, so that full access includes their next session.
49. As a user, I want completing a plan session to create only the following session, so that the board stays one session in front of me.
50. As a user, I want home not to list a full calendar of unused future sessions, so that peek-ahead is not a calendar with a curtain.
51. As a user, I want cycle length to stop minting the next plan session when the horizon is done, so that a finished plan does not keep producing sessions.
52. As a user, I want to log weights, reps, and notes on the current plan session, so that the next session has something to progress from.
53. As a user, I want completing a session whose prescribed reps were hit to create the next session with loads increased by 2kg (or the documented default), so that progression is automatic.
54. As a user, I want to override that bump when logging, so that I can hold or drop load on purpose.
55. As a user, I want the next session not to add load automatically if the logged result did not complete the prescribed work, so that failed sessions are not rewarded.
56. As a user, I want Fitness Q&A to remain, so that I can still ask a client-specific question without generating a WOD.
57. As a user, I want a Fitness Q&A prompt payload to include the selected client profile sentence text and my question, so that answers are not abstract.
58. As a user, I want Q&A follow-up turns to keep that profile attached, so that I do not restate the client.
59. As a user, I want Fitness Q&A not to require equipment text in this spec, so that Q&A stays profile-plus-question as in the POC.
60. As a user, I want a typed brief to stay optional for WODs, so that one-click remains the path and conversation is a tweak, not the default.
61. As someone on a floor display with a session already loaded, I want to keep viewing that whiteboard or plan session if the backend becomes unreachable, so that a wifi blip does not blank the board.
62. As someone on a floor display in that state, I want to type results that queue and sync when the network returns, so that we do not lose Saturday scores.
63. As a user, I want generate and mint-next-session to require the backend, so that we do not invent a WOD or the next plan session with no store and no model.
64. As a user, I want the other user’s signed-in session to see a result I logged once it has synced, so that history is shared.
65. As a user, I want partner WODs that cannot share one format to be generated as two one-click singles instead, so that we do not put two full tracks on one board.
66. As a user, I want only these two humans as users and clients for now, so that a third person who cannot sign in is not in this spec.

## Implementation Decisions

- Evolve the existing browser harness into this product. Do not start from a blank app. The POC’s localStorage store and “no authentication” are superseded by ADR 0001 and ADR 0002.
- Two users exist (Craig and Gillian). Both have full access. Each user has one corresponding client. Sign-in does not collapse user and client into one thing.
- Sign-in on a phone is a lock for random browsers and a persona for defaults (me). A paired floor display skips sign-in, runs the full app with no me, and logs results into the participant slots of the session on screen (ADR 0001).
- Canonical records live in one app-owned backend: client profiles, equipment, gym space, agreed WODs, workout plans, plan sessions, results, and API keys. Browser localStorage is not the source of truth (ADR 0002). This spec does not pick a vendor or database product.
- API keys and harness configuration stay entered, saved, and updated in the UI. The app does not use environment files for configuration, API keys, or client data.
- Client profiles, equipment, and gym space stay sentence text, included directly in prompt payloads. They are not shaped into database-formatted records for prompting. Results are structured (scores, times, weights, reps, notes).
- A signed-in session defaults home, next plan session, profile, and a new WOD to me. The user can switch to the other client or both. A floor display has no me: pick clients (or use the session already on screen) before a new generate or prompt.
- A WOD is a structured whiteboard (warmup + workout + cooldown), not a narrative chat reply. Girl / Hero / CrossFit.com-style named WODs are format inspiration, not a scraped or licensed catalog. Per-client scaling on the board is at most one line. Coaching notes (how-to, things to note, other explanation) are collapsed until expanded.
- One-click WOD generation runs once client context is known. No prompt is required. Regenerate or short follow-up is allowed before agree/save. A typed brief is optional. Strength workout plans stay a planning conversation, then an explicit save.
- WOD prompt payloads attach selected client profile sentence text and saved equipment sentence text. When more than one client is selected, they also attach gym space. Once results exist, subsequent WOD payloads attach recent results for those clients. Generation still cannot start with no client context.
- Saving an agreed WOD is an explicit action. Generation does not write history. The record includes date, participating clients, the whiteboard content, and later results.
- Profile staleness is measured from when that client’s profile was last saved. After logging a WOD result, if a participating client’s profile is older than three months, prompt the user to review it. Do not block logging.
- Retest uses the saved agreed WOD as the session to repeat. Suggest retests on a roughly two-month cadence when a WOD still has an older result. Comparison shows previous result vs new result. A retest is a new result on the same agreed WOD, not a duplicate history row.
- Workout-plan chat stays conversational for focus, duration, cycle length, and gym space. Saving the agreed plan is explicit. Cycle length is stored on the plan as a horizon. Prompt payloads include selected client profiles, equipment, and gym space.
- A saved plan materializes only the next plan session (ADR 0003). Home has one-click access to that session (default me; switch for the other client). Completing it materializes the following session only. Cycle length stops minting when the horizon is done. Home does not show a full calendar.
- Progression is a simple default from logged results (add 2kg when prescribed reps were completed). The user can override. Incomplete prescribed work does not automatically add load. This is not an RPE or periodization engine.
- If the backend is unreachable, a WOD or plan session already on the device can still be viewed and have results typed; those results queue and sync when the network returns. Generating a new WOD or minting the next plan session requires the backend (ADR 0004).
- Fitness Q&A from the POC stays. Q&A payloads attach selected client profile sentence text and the user’s question, not equipment text in this spec.
- Feature work for WOD history, retest, and next-session flow is blocked on this platform existing: sign-in, pairing, backend, and default-to-me. Do not implement those slices against the POC’s no-auth localStorage model.

## Testing Decisions

- Good tests observe external behavior only: what a signed-in user or a floor display can do, what is on the whiteboard, what got saved, which sentence text and results were attached to a prompt payload, and what another authenticated session sees after reload. Tests do not lock onto internal module structure, storage internals, pairing tokens, prompt-assembly helpers, or a second API/DB suite.
- There is one test seam: the running web application, in as many browsers as the product has surfaces (unsigned window, signed-in user window, paired floor display window), including outbound prompt payloads. This is the same height as the POC Playwright suite, which intercepts the chat-completions request. Do not add a lower seam unless that single seam cannot observe payload contents, shared records, pairing, or queued offline results.
- At that seam, tests cover: an unsigned window must sign in; a signed-in user defaults to me and can switch client; records saved in one signed-in session are visible after reload in another; a paired floor display skips sign-in, has no me, and logs results into participant slots; one-click WOD after client context is known, shown as a whiteboard with collapsed coaching notes; WOD payloads include profile and equipment (and gym space when more than one client is selected); explicit agree/save writes history and generation does not; results persist; a later WOD payload includes recent results; planning chat then explicit plan save; only the next plan session exists; completing it creates the following one; progression from logged results; a loaded session remains viewable and results remain typeable when the network is cut, while generate and mint-next-session do not; Q&A still attaches profile sentence text.
- Prior art: the POC Playwright tests against the running app and intercepted model requests. New tests extend that suite at the same seam. Each slice’s acceptance criteria must fail on the current POC before that slice is implemented.

## Out of Scope

- A third client who is not a user.
- Clients as a reduced-permission portal (only-me access, no settings).
- Persona-only access (anyone with the URL can use the full app).
- Lock-everywhere (no floor display without an account).
- Notion as the system of record.
- Per-device export/sync as the sharing model.
- Splitting API keys into the browser and history into a server.
- Full offline-first generate, agree, and mint-next-session.
- Replacing client profiles with a structured intake form.
- A licensed or scraped CrossFit.com WOD database.
- A visual floor-plan editor.
- Materializing a full plan calendar or peek-ahead weeks.
- Notifications outside the app.
- A full RPE / periodization engine.
- Environment files for API keys, harness configuration, or client data.

## Further Notes

- ADRs in force: 0001 (sign-in and paired floor display), 0002 (app backend is source of truth), 0003 (next plan session is storage), 0004 (offline finish loaded session only).
- Domain terms live in `CONTEXT.md`. Use **user**, **client**, **me**, **floor display**, **WOD**, **whiteboard**, **coaching note**, **agreed WOD**, **result**, **retest**, **workout plan**, **cycle length**, **plan session**, **gym space**, **prompt payload**.
- Fitness Q&A attaches selected client profile(s) and the user’s prompt. It does not, in this spec, attach equipment text.
- Harness configuration is UI-accessible; this spec does not enumerate its fields or choose a model vendor.
- Pairing mechanism, sign-in provider, and backend vendor are not chosen here; the behaviour those choices must satisfy is.
