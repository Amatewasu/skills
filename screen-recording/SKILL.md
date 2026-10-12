---
name: screen-recording
version: 0.0.1
description: Record concise screen recordings for humans, scoped to what the change makes relevant. Use when making a screen recording, demo clip, or walkthrough video for an MR, QA briefing, bug report, or release note - especially when the recording must show only the changed behavior instead of a full session, navigation, or e2e test run.
---

# Screen recording

A recording for humans is evidence of a change, not a session log. Build it from a **shot list**, and let the list do the cutting: if it is not on the list, it is not in the video.

## 1. Derive the shot list from the change

- Source material: the diff, the ticket, or the request. One shot per user-visible behavior the change introduces, fixes, or removes.
- A shot is one interaction that ends with the changed behavior visible on screen (the dialog opening, the validation firing, the layout fixing itself).
- A whole test suite is one shot at most: the seconds where the relevant step or assertion runs, or the single frame of its summary result. The rest of the run is the same evidence watched slower.
- Completion criterion: every user-visible change in the diff has a shot, and every shot traces to a line of the change. Anything else is out.

## 2. Arrive in state before the first frame

Do all setup - login, navigation, seeded data, waiting for the app to load - before recording starts. The clip opens already in the changed state; the viewer never watches the app boot.

## 3. Record the shots

- One capture per shot: press record, perform the interaction, stop.
- Hold the end state 1-2 seconds before stopping. That hold is the emphasis; it is the only pause the video gets.
- When the change lives behind several unrelated steps (e.g. an e2e run reaching the changed feature), record only the relevant segment and trim or restart the capture around it.
- Catch a wrong take by re-recording the shot, then trim; a stray take never survives into the final cut.

## 4. Assemble and trim

Duration budget: 15-30 seconds is the target, 60 seconds the ceiling, each shot under 10. A shot that cannot fit in 10 seconds shows a before/after pair or a 2x speed-up instead of real time.

Trim and join the raw captures with ffmpeg (seek before `-i` for fast, lossless cuts):

```
ffmpeg -ss <start> -to <end> -i in.webm -c copy out.webm
printf "file 'shot1.webm'\nfile 'shot2.webm'\n" > list.txt
ffmpeg -f concat -safe 0 -i list.txt -c copy final.webm
```

Watch the assembled cut once, end to end. Completion criterion: total duration inside the budget and every second shows a listed shot.
