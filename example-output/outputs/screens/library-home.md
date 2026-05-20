> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Library Home

> File: outputs/screens/library-home.md
> Screen ID: PG05

| Attribute | Value |
|---|---|
| Route | `/library` |
| Access | Listener, source owner |
| Mobile | Not required |
| Scope | In scope |

### Design intent

- Primary purpose: Provide instant cached browsing and a prominent route back to the current audiobook.
- Primary actors: Harvey the Home Server Hobbyist; Chloe the Cloud Collector.
- Primary job: Resume listening or pick a cached audiobook without waiting on network folder reads.
- Principle served: Local cache first; network state should not block normal browsing.
- Entry points: App launch with configured source; Sync Status; Back from Book Detail/Now Playing.
- Success outcome: User resumes current book or opens another book detail screen.
- Content priority: Primary: Continue listening, current progress, library shelves/grid. Secondary: source health, last sync, add source/settings.
- Critical variants: Loaded cache; no books; source degraded; source unreachable with cached data; first sync in progress.
- Exit paths: Book Detail, Now Playing, Source Setup, Settings and Sources.

### Related flows

- harvey-resumes-a-long-book.md: Starts resume path from the cached continue card.
- chloe-connects-cloud-library.md: Confirms cloud-backed books are browsable from cache.

### Decisions on this screen

| ID | Decision | Stakes | Reversible |
|---|---|---|---|
| D1 | Resume current audiobook | Med | Yes |
| D2 | Add another source | Low | Yes |

**Resume current audiobook — D1**
- What the user is deciding: Whether to continue the last audiobook immediately.
- Options: Resume current book, open book detail, browse another book.
- Information required: Book title, chapter, exact progress, source status, last played.
- Risk if wrong: User may resume the wrong book or start playback from an unavailable source.
- Reversible: Yes — Recovery: Pause/back from Now Playing.
- Recommended default: Focus Continue listening when a current book exists.
- Conditions that change this: If source is unreachable, resume is disabled and recovery is offered.

### States

| State | Description | Transitions |
|---|---|---|
| `loaded` | Cached books available | Open book or resume |
| `empty` | No cached books exist | Add source or open Sync Status |
| `syncing_partial` | Some cached results available while sync runs | Open books or view sync |
| `source_degraded` | Cached library exists but source has warnings | Browse cache or view recovery |
| `source_unreachable` | Cache exists but playback may fail | Browse details, recovery for playback |

### Actions

| Action | Description | Precondition |
|---|---|---|
| Resume | Opens Now Playing for current position | Current book playable/source reachable |
| Open book | Opens Book Detail | Book exists in cache |
| Add source | Opens Source Setup | Always |
| Settings | Opens Settings and Sources | Always |
| View sync status | Opens Sync Status | Active or recent sync exists |

### Data required

| Data | Source | Offline | Notes |
|---|---|---|---|
| Book catalog | Local database | Cached | Primary browsing data |
| Current progress | Local database | Cached | Continue listening |
| Source health | Local DB/source probe | Cached/Real-time | Secondary indicator |
| Last sync time | Local database | Cached | Trust signal |
| Cover/title/author metadata | Local database/tags | Cached | Fallback if cover missing |

### Visual indicators

| Indicator | Meaning |
|---|---|
| Continue listening focus card | Current recommended action |
| Progress bar on book cards | Stored listening progress |
| Source health badge | Connected, syncing, degraded, unreachable |
| Sync spinner/badge | Background indexing active |

### Critical variants

**Loaded cache**
Continue listening is first, with shelves or grid below.

**Empty library**
Explain whether no source exists, sync has not run, or no books were found; provide Add source or View sync.

**Source unreachable with cache**
Allow browsing cached metadata while disabling playback actions that require stream resolution.

### Layout regions

| Region | Purpose | Required content |
|---|---|---|
| Top bar | Status and navigation | App title, source health, settings |
| Continue shelf | Fast resume | Book title, author, chapter, time remaining, Resume |
| Library grid | Browse cached books | Cover/placeholder, title, author, progress |
| Source/status rail | Show sync/source health | Last sync, active source, warnings |
| Empty/recovery region | Recover when no usable catalog | Add source, View sync, Retry |

### Content

| Element | Copy |
|---|---|
| Page title | Audiobook library |
| Primary action label | Resume |
| Empty state heading | No audiobooks in the cache |
| Empty state body | Add a source or run sync to build the local library. |
| Empty state CTA | Add source |
| Error heading | Source unavailable |
| Error body | Cached books are still visible, but playback needs the source to reconnect. |
| Offline message | Browsing from local cache. Playback may need the source connection. |

### Screen generation prompt

> Target tool: other / native Android TV prototype

Create an Android TV library home screen that opens instantly from cached metadata and makes continuing the current audiobook the most prominent action. Harvey and Chloe should see a large Continue listening card with title, author, current chapter, exact progress/time remaining, source status, and a Resume action, followed by a TV-readable grid or shelves of cached books. The screen must handle loaded, empty, syncing-partial, source-degraded, and source-unreachable-with-cache states while preserving browsing access from local data. Keep source health, last sync, add source, and settings visible but secondary, and make every card and action work with strong D-pad focus indicators inside overscan-safe margins.

### Acceptance criteria

- [ ] The current book can be resumed from the first focused region on a standard TV viewport.
- [ ] Cached book cards show realistic title, author, and progress data.
- [ ] Source health is visible without blocking cached browsing.
- [ ] Empty state distinguishes no source, no sync, and no books found through copy or action context.
- [ ] Source-unreachable state disables or redirects playback while leaving cache browsing visible.
- [ ] Settings and add-source paths are present but secondary.
- [ ] All focusable cards have stable dimensions and visible focus state.

### Heuristic check

All core heuristic items are defined for this screen. Heuristic gaps: None.
