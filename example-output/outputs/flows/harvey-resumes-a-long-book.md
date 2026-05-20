> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Flow: Resume a Long Book from the Couch

> File: outputs/flows/harvey-resumes-a-long-book.md
> Persona: Harvey the Home Server Hobbyist
> Journey: harvey-resumes-a-long-book.md
> Implements: Return to current book, Inspect book context, Start progressive stream, Control long-form listening, Leave and preserve state

- Trigger: Harvey opens the app after previous listening progress exists.
- Entry point: Library Home.
- Preconditions: At least one indexed book exists; playback progress exists; local metadata cache is readable; source may be online or offline.
- Steps:
  1. The app loads Library Home from local metadata and focuses Continue listening ← Return to current book.
  2. Harvey reviews book title, chapter, time remaining, source status, and last played.
  3. Harvey either presses Resume directly or opens Book Detail ← Inspect book context.
  4. On Book Detail, the app shows ordered chapters, current chapter marker, progress, and playable status.
  5. Harvey selects Resume.
  6. The app resolves the playable asset reference into an SMB URI or cloud download URL and opens Now Playing ← Start progressive stream.
  7. Now Playing shows buffering feedback until audio starts.
  8. Harvey uses pause, skip back 15 seconds, skip forward, speed, and chapter navigation with D-pad focus ← Control long-form listening.
  9. The app saves exact position every 5 seconds of active playback and immediately on pause, skip, chapter change, or termination.
  10. Harvey leaves the app; audio continues through MediaSession-style background playback ← Leave and preserve state.
- Decision points: Resume immediately vs open book detail; change speed; skip within chapter; choose another chapter; stop playback.
- Branches:
  - Source online and asset resolves: playback starts after buffering.
  - Source offline: Now Playing shows cached metadata and Connection Recovery action; playback is blocked.
  - Asset no longer exists: Book Detail marks chapter unavailable and offers resync.
  - Buffer underrun: Now Playing shows buffering state and keeps controls visible.
  - Progress save fails: playback continues, but Settings/diagnostics records persistence warning.
- Error paths: Missing asset reference, stream resolution failure, source offline, network timeout, unsupported audio file, database write failure.
- Exit states: Playback active; playback paused with position saved; user returns to Library Home; recovery required.
- Completion criteria: Harvey can resume the current book at the stored millisecond position and leave the app without losing playback or progress.
