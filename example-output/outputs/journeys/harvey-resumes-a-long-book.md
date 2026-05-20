> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Journey: Resume a Long Book from the Couch

> File: outputs/journeys/harvey-resumes-a-long-book.md
> Persona: Harvey the Home Server Hobbyist
> Trigger: Harvey opens the app after previously listening
> End state: Playback resumes at the exact stored position

## Stages

### Stage 1: Return to current book
Harvey opens the app from the Android TV launcher and expects the most relevant audiobook to be ready without browsing through a file tree. He wants to know exactly where he left off and whether the backing source is reachable. The library home must prioritize "continue listening" over catalog management. The risk is forcing him to remember book, chapter, or folder location.

**Data needed:** Current book, current chapter, stored millisecond position, source health, last played time.
**Friction risk:** If progress is displayed only as a percentage, Harvey may not trust that the exact position was retained.

### Stage 2: Inspect book context
Harvey may open book detail to confirm chapters, progress, or source state before playback. He needs clear chapter ordering and a visible resume action. The app should make the current chapter and remaining time obvious from across the room. The risk is presenting raw file names in a way that obscures the book structure.

**Data needed:** Book title, author, chapter list, track numbers, current chapter, progress per chapter, playable asset status.
**Friction risk:** Bad metadata inferred from folders or ID3 tags can make the book hard to identify.

### Stage 3: Start progressive stream
Harvey presses resume and expects playback to begin quickly. The app resolves the stored playable asset reference into an SMB stream URI and buffers enough to withstand short dropouts. He needs immediate feedback if source resolution fails. The risk is hiding buffering, token, or share errors behind a generic spinner.

**Data needed:** Playable asset reference, resolved URI, buffer status, player state, retry count.
**Friction risk:** Stream start delays longer than a few seconds can make the app feel unreliable.

### Stage 4: Control long-form listening
During playback, Harvey uses skip back, skip forward, speed, and pause without looking closely. The controls must be large, directional, and predictable with a distinct focus indicator. The app saves progress every few seconds and on pause or app termination. The risk is applying music-player assumptions that make chapter and long-book navigation clumsy.

**Data needed:** Position, duration, chapter title, speed, buffer status, skip interval, save state.
**Friction risk:** Tiny controls or unclear focus order can cause accidental skips.

### Stage 5: Leave and preserve state
Harvey exits to the Android TV dashboard or switches apps while audio continues. He needs the MediaSession-style background playback to remain stable and to save state instantly when playback stops. The app should make it clear that resume will work later. The risk is losing progress at a long-book boundary or during app lifecycle changes.

**Data needed:** Media session state, foreground service status, last persisted position, resume target.
**Friction risk:** Infrequent persistence could cause replaying several minutes after interruption.

## Moments that matter
- **App launch to continue card:** The first screen should prove that cached metadata makes the app faster than folder browsing.
- **Resume action to audible playback:** Trust depends on prompt start, visible buffer status, and source-specific recovery if playback fails.
- **Leaving the app:** Background playback and exact progress persistence are core audiobook differentiators.

## Screens implied

| Stage | Screen needed |
|---|---|
| Return to current book | Library Home |
| Inspect book context | Book Detail |
| Start progressive stream | Now Playing |
| Control long-form listening | Now Playing |
| Leave and preserve state | Now Playing |
