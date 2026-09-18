# Personal trainer

A browser app two users share to generate whiteboard WODs, run strength plans, and keep results. A signed-in user is defaulted to themselves; a paired gym screen runs the same app with nobody signed in.

## People

**User**:
A signed-in person with full access to the app. There are two: Craig and Gillian.
_Avoid_: Trainer (as the person who uses the app), account, role

**Client**:
A person being coached, held as a record of profile, WODs, plans, and results. Each user has one corresponding client; signing in does not make the user the client.
_Avoid_: User, athlete, member

**Me**:
The signed-in user's corresponding client. Home, next plan session, profile, and a new WOD start on this client; the user can switch to the other client or both.
_Avoid_: Current user, session (as the person)

**Floor display**:
A paired gym device that skips sign-in, runs the full app, and has no me.
_Avoid_: Kiosk, guest, TV mode, anonymous user

## Records

**Client profile**:
Saved sentence text about one client. That text is what prompts receive.
_Avoid_: Client object, database record, intake form (the profile is the saved sentences, not a structured questionnaire)

**Equipment**:
Saved sentence text describing what is available to train with.
_Avoid_: Inventory, gear list (as a structured catalog)

**Gym space**:
Saved sentence text describing gym dimensions and floor plan, used when more than one client trains together.
_Avoid_: Floor-plan editor, CAD, map

## Sessions and programs

**WOD**:
A single CrossFit-style session shown as a whiteboard: warmup, the workout, and cooldown. Short enough to run without staring at the board.
_Avoid_: Narrative workout, prose session, chipper (as the default). Named Girl / Hero / CrossFit.com WODs are inspiration for format, not a licensed catalog to copy.

**Whiteboard**:
The glanceable layout of a WOD (title, format, time cap or rounds, movements), not a paragraph of coaching copy. Per-client scaling on the board is at most one line.
_Avoid_: Chat reply, markdown essay

**Coaching note**:
How to perform a movement, things to note, and other explanation that sits behind the whiteboard, collapsed until opened.
_Avoid_: Narrative workout, scaling paragraph

**Workout**:
Any single training session. A WOD is the CrossFit whiteboard form; a plan session is the strength-programming form.
_Avoid_: Using "workout" and "WOD" as if they were always the same thing

**Agreed WOD**:
A generated WOD the user explicitly saves. Saving tags it with a date and the participating clients. Generation alone does not save.
_Avoid_: Autocomplete, draft WOD (as the history record)

**Workout history**:
The list of agreed WODs, each with date, participants, and any logged results.
_Avoid_: Chat log, conversation history

**Result**:
Scores, times, weights, reps, and notes logged against an agreed WOD or a plan session. On a floor display, a result fills a participant slot of the session on screen.
_Avoid_: Analytics, metrics dashboard

**Retest**:
Running an agreed WOD again later so the new result can be compared with an earlier one. The working cadence is: suggest a retest about every two months for a WOD that still has an older result (the stakeholder example was a WOD last tested six months ago).
_Avoid_: PR attempt, benchmark (unless the user named it that)

**Workout plan**:
A multi-session, goal-oriented, or progressive strength program the user agrees and saves. Cycle length is part of the saved plan.
_Avoid_: WOD, single session, mesocycle (unless the user used that word)

**Cycle length**:
How many weeks the saved workout plan covers. A horizon, not a list of unpublished sessions.
_Avoid_: Periodization block (as a required term), calendar

**Plan session**:
One session inside a saved workout plan. Only the next immediate plan session exists. Completing it creates the following one.
_Avoid_: Full calendar, week view of every session

**Progression**:
An automatic update to the next plan session from the logged result (for example adding 2kg after a completed set of prescribed reps).
_Avoid_: Full RPE engine, periodization model

## Harness

**Harness**:
This agentic application.
_Avoid_: Bot, GPT wrapper

**Prompt payload**:
The text sent to the model, including attached sentence text (profiles, equipment, gym space, and, when they exist, results).
_Avoid_: API request internals as the thing the user cares about
