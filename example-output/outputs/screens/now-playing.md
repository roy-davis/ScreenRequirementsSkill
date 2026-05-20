> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Now Playing

> File: outputs/screens/now-playing.md
> Screen ID: PG07

| Attribute | Value |
|---|---|
| Route | `/player` |
| Access | Listener |
| Mobile | Not required |
| Scope | In scope |

### Design intent

- Primary purpose: Provide audiobook-specific playback controls with exact progress and source-aware buffering/recovery.
- Primary actors: Harvey the Home Server Hobbyist; Chloe the Cloud Collector.
- Primary job: Listen, pause, seek, skip, change speed, and trust progress persistence during long-form playback.
- Principle served: Playback is the product; every control must be usable at TV distance with a remote.
- Entry points: Library Home Resume, Book Detail Resume/Start/chapter, restored background session.
- Success outcome: Audio plays or pauses while exact position is saved and recoverable.
- Content priority: Primary: book/chapter, progress, transport controls, speed, source/buffer status. Secondary: chapter list, persistence indicator, source recovery.
- Critical variants: Loading; playing; buffering; paused; background session returned; playback error; source unreachable.
- Exit paths: Book Detail, Library Home, Connection Recovery, Android TV dashboard.

### Related flows

- harvey-resumes-a-long-book.md: Core resume and playback control flow.

### Decisions on this screen

| ID | Decision | Stakes | Reversible |
|---|---|---|---|
| D1 | Change playback position or speed | Med | Yes |
| D2 | Stop or leave playback running | Med | Yes |

**Change playback position or speed — D1**
- What the user is deciding: Whether to alter listening speed or position within the book.
- Options: Skip back 15 seconds, skip forward, scrub/seek, chapter previous/next, speed 0.5x to 2.5x.
- Information required: Current chapter, elapsed/remaining time, speed, buffer status.
- Risk if wrong: User may lose context in a long audiobook or create a buffering delay.
- Reversible: Yes — Recovery: Skip back, choose previous chapter, or return to current saved position.
- Recommended default: Keep speed at last-used value; focus Play/Pause first.
- Conditions that change this: Seeking is blocked while stream is still resolving.

**Stop or leave playback running — D2**
- What the user is deciding: Whether to pause/stop or let background playback continue.
- Options: Pause, Back to book, Home/dashboard, keep playing.
- Information required: Save status and background playback indicator.
- Risk if wrong: User may think audio stopped or progress was not saved.
- Reversible: Yes — Recovery: Return to app and pause/resume.
- Recommended default: Leaving app keeps playback running while MediaSession is active.
- Conditions that change this: Playback error or source unavailable stops background playback.

### States

| State | Description | Transitions |
|---|---|---|
| `loading` | Asset resolving and initial buffer preparing | Ready → playing; failure → error |
| `playing` | Audio active in foreground | Pause, seek, buffer, background, ended |
| `buffering` | Waiting for network/buffer | Recover → playing; timeout → error |
| `paused` | Playback paused and position saved | Resume → playing |
| `background_returned` | User returns from dashboard to active session | Foreground controls resume |
| `error` | Stream cannot continue | Retry or recovery |

### Actions

| Action | Description | Precondition |
|---|---|---|
| Play/Pause | Toggles playback | Asset loaded or paused |
| Skip back 15 seconds | Moves position back | Session loaded |
| Skip forward | Moves position forward | Session loaded |
| Change speed | Sets speed between 0.5x and 2.5x | Session loaded |
| Choose chapter | Opens chapter selector or Book Detail | Book has chapters |
| Retry stream | Attempts to resolve/play again | Error or buffering timeout |
| Open recovery | Opens Connection Recovery | Source/asset failure |

### Data required

| Data | Source | Offline | Notes |
|---|---|---|---|
| Current book/chapter | Local database | Cached | Always visible |
| Playback position/duration | Player/local DB | Local | Saved every 5 seconds and lifecycle events |
| Stream/buffer status | Player/source | Real-time | Drives loading/buffering indicators |
| Playback speed | Local preference/player | Local | Persisted preference |
| Source health | Source resolver | Cached/Real-time | Used for recovery |

### Visual indicators

| Indicator | Meaning |
|---|---|
| Large progress rail | Position in current chapter/book |
| Saved check/status | Progress persisted locally |
| Buffer badge | Waiting for stream data |
| Source badge | SMB/cloud source status |
| Focus scale on transport controls | Active remote target |

### Critical variants

**Loading**
Show book and chapter immediately from cache while stream resolution and buffering occur.

**Buffering**
Keep controls visible, show what is happening, and avoid replacing the whole screen with a spinner.

**Playback error**
Explain whether the source is unreachable, token expired, asset missing, or format unsupported; route to recovery.

### Layout regions

| Region | Purpose | Required content |
|---|---|---|
| Playback identity | Keep listener oriented | Book title, author, chapter, source |
| Progress area | Show exact position | Elapsed, remaining, chapter/book progress |
| Transport controls | Core listening actions | Back 15, play/pause, forward, previous/next, speed |
| Status strip | Show buffer/save/source state | Buffer, saved position, source health |
| Secondary actions | Navigate/recover | Chapters, Book detail, Retry, Recovery |

### Content

| Element | Copy |
|---|---|
| Page title | Now playing |
| Primary action label | Pause |
| Empty state heading | Nothing playing |
| Empty state body | Choose a book from your library to start listening. |
| Empty state CTA | Open library |
| Error heading | Playback stopped |
| Error body | The app could not resolve the current chapter stream from this source. |
| Offline message | Source connection lost. Cached progress is saved, but playback needs the source to reconnect. |

### Screen generation prompt

> Target tool: other / native Android TV prototype

Create a full-screen Android TV audiobook player for long-form listening. The screen must show cached book and chapter identity immediately, exact elapsed and remaining time, a large progress rail, transport controls for skip back 15 seconds, play/pause, skip forward, chapter previous/next, and speed from 0.5x to 2.5x, plus source, buffer, and progress-saved indicators. Handle loading, playing, buffering, paused, background-returned, and playback-error states without hiding controls during buffering. Recovery must be specific to source unreachable, token expired, missing asset, or unsupported format, and all controls must be large, stable, overscan-safe, and obvious under D-pad focus.

### Acceptance criteria

- [ ] The listener can pause, resume, skip, and change speed with D-pad focus on a TV viewport.
- [ ] Exact progress and saved-position status are visible during playback and pause.
- [ ] Loading and buffering keep book/chapter context visible.
- [ ] Playback error states distinguish source, authorization, missing asset, and unsupported format failures.
- [ ] Speed control clearly supports 0.5x through 2.5x.
- [ ] Leaving and returning to playback is represented through background session status.
- [ ] All transport controls have stable size and focus states.

### Heuristic check

All core heuristic items are defined for this screen. Heuristic gaps: None.
