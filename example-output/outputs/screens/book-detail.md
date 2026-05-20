> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Book Detail

> File: outputs/screens/book-detail.md
> Screen ID: PG06

| Attribute | Value |
|---|---|
| Route | `/library/book/:bookId` |
| Access | Listener |
| Mobile | Not required |
| Scope | In scope |

### Design intent

- Primary purpose: Show audiobook context, chapter order, progress, and the next playback action.
- Primary actors: Harvey the Home Server Hobbyist; Chloe the Cloud Collector.
- Primary job: Confirm the right book/chapter and resume or start playback confidently.
- Principle served: Long-form context; the user must understand progress across a long book, not just a single track.
- Entry points: Library Home book card; Back from Now Playing.
- Success outcome: User starts playback from the current position or a chosen chapter.
- Content priority: Primary: title, author, progress, resume action, current chapter. Secondary: chapter list, source status, metadata warnings.
- Critical variants: Ready; no progress; partially indexed; unavailable source; missing chapter asset.
- Exit paths: Now Playing, Library Home, Connection Recovery.

### Related flows

- harvey-resumes-a-long-book.md: Allows Harvey to inspect the book before resuming.

### Decisions on this screen

| ID | Decision | Stakes | Reversible |
|---|---|---|---|
| D1 | Resume or choose chapter | Med | Yes |

**Resume or choose chapter — D1**
- What the user is deciding: Where playback should start.
- Options: Resume current position, start from beginning, play focused chapter, return to library.
- Information required: Current chapter, exact time position, chapter durations, source/playable status.
- Risk if wrong: User may lose place or start the wrong chapter.
- Reversible: Yes — Recovery: Pause and choose another chapter; progress persists.
- Recommended default: Resume current position when progress exists.
- Conditions that change this: If the asset is unavailable, playback actions are blocked and recovery is shown.

### States

| State | Description | Transitions |
|---|---|---|
| `ready_with_progress` | Book indexed and progress exists | Resume → Now Playing |
| `ready_new` | Book indexed with no progress | Start → Now Playing |
| `partial_index` | Some chapters are missing/skipped | Play available; view sync details |
| `unavailable_source` | Source cannot resolve playable assets | Open recovery |
| `missing_asset` | Specific chapter file no longer exists | Resync or choose another chapter |

### Actions

| Action | Description | Precondition |
|---|---|---|
| Resume | Starts playback at stored position | Progress exists and asset resolvable |
| Start book | Starts first playable chapter | No progress or user chooses restart |
| Play chapter | Starts focused chapter | Chapter playable |
| View source issue | Opens Connection Recovery | Source degraded or asset unavailable |
| Back to library | Returns to Library Home | Always |

### Data required

| Data | Source | Offline | Notes |
|---|---|---|---|
| Book title/author/id | Local database | Cached | Inferred from folders/tags |
| Chapter list/order | Local database | Cached | Track number and file order |
| Current position | Local database | Cached | Millisecond precision |
| Playable asset status | Local DB/source probe | Cached/Real-time | Used to block playback |
| Source label/health | Local database | Cached | Shows SMB/provider origin |

### Visual indicators

| Indicator | Meaning |
|---|---|
| Resume button emphasis | Recommended next action |
| Chapter progress marker | Current or completed chapter |
| Warning badge on chapter | Chapter unavailable or unsupported |
| Source badge | Source connected, degraded, or unreachable |

### Critical variants

**Ready with progress**
Focus Resume and show exact chapter/time position plus total progress.

**Partial index**
Show playable chapters normally and mark skipped/unavailable chapters with a specific reason and sync details action.

**Unavailable source**
Keep cached book metadata visible but block playback and offer recovery.

### Layout regions

| Region | Purpose | Required content |
|---|---|---|
| Book summary | Confirm identity and progress | Title, author, duration/progress, source |
| Primary action | Start or resume | Resume/Start, current position |
| Chapter list | Navigate long-form structure | Chapter title, order, duration, progress, status |
| Detail/status panel | Explain focused chapter/source | File/source status, warnings, next action |
| Navigation row | Exit/recover | Back, Source issue, Sync details |

### Content

| Element | Copy |
|---|---|
| Page title | Book details |
| Primary action label | Resume |
| Empty state heading | No playable chapters |
| Empty state body | This book was found, but no supported audio chapters are available yet. |
| Empty state CTA | View sync details |
| Error heading | Chapter unavailable |
| Error body | The saved chapter reference cannot be resolved from this source. |
| Offline message | Book details are cached. Playback needs the source to reconnect. |

### Screen generation prompt

> Target tool: other / native Android TV prototype

Create a TV-readable audiobook detail screen for a cached book selected from Library Home. The user must immediately see title, author, source, total progress, current chapter, exact resume position, and a primary Resume or Start action, followed by an ordered chapter list with status for each chapter. The screen must support ready-with-progress, ready-new, partial-index, unavailable-source, and missing-asset variants, keeping cached metadata visible even when playback is blocked. Make chapter navigation D-pad friendly with large rows, visible focus, current-position indicators, realistic audiobook chapter names, and source-specific recovery actions when an asset cannot be resolved.

### Acceptance criteria

- [ ] The user can identify the book and resume position from the first screenful.
- [ ] Resume is primary when progress exists; Start is primary when it does not.
- [ ] Chapter rows show order, title, duration/progress, and playable status.
- [ ] Unavailable source keeps metadata visible but blocks playback with recovery.
- [ ] Partial index state marks skipped or missing chapters without hiding playable ones.
- [ ] All chapter and action focus states are clear for D-pad navigation.

### Heuristic check

All core heuristic items are defined for this screen. Heuristic gaps: None.
