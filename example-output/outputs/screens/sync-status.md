> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Sync Status

> File: outputs/screens/sync-status.md
> Screen ID: PG04

| Attribute | Value |
|---|---|
| Route | `/sync/status` |
| Access | Source owner, listener |
| Mobile | Not required |
| Scope | In scope |

### Design intent

- Primary purpose: Show metadata indexing progress and whether cached library browsing is ready.
- Primary actors: Harvey the Home Server Hobbyist; Chloe the Cloud Collector.
- Primary job: Understand what the background sync found, what failed, and when it is safe to open the library.
- Principle served: Make background work observable without blocking listening.
- Entry points: Folder Picker confirmation; Settings resync; Connection Recovery retry.
- Success outcome: User sees books indexed and opens Library Home from cache.
- Content priority: Primary: sync state, books found, chapters found, Open library. Secondary: skipped files, errors, source diagnostics.
- Critical variants: Queued; running; partial; complete; failed; paused due to rate limit; cancelled.
- Exit paths: Library Home, Settings and Sources, Connection Recovery.

### Related flows

- harvey-connects-home-library.md: Verifies SMB indexing.
- chloe-connects-cloud-library.md: Verifies cloud metadata sync and provider limits.

### Decisions on this screen

| ID | Decision | Stakes | Reversible |
|---|---|---|---|
| D1 | Open partial library before sync completes | Med | Yes |
| D2 | Cancel current sync | Med | Yes |

**Open partial library before sync completes — D1**
- What the user is deciding: Whether to start browsing cached results while indexing continues.
- Options: Open library, stay on sync status.
- Information required: Number of books found, sync progress, current source state, known errors.
- Risk if wrong: User may think missing books are absent rather than not indexed yet.
- Reversible: Yes — Recovery: Return to Sync Status from Settings or status badge.
- Recommended default: Allow Open library once at least one book is indexed.
- Conditions that change this: Block if no cached books exist.

**Cancel current sync — D2**
- What the user is deciding: Whether to stop the background crawl.
- Options: Continue syncing, cancel sync.
- Information required: Current progress, partial results preserved, how to restart later.
- Risk if wrong: Library may remain incomplete until resync.
- Reversible: Yes — Recovery: Start sync again from Settings and Sources.
- Recommended default: Continue syncing.
- Conditions that change this: Cancellation is hidden when sync is complete or failed.

### States

| State | Description | Transitions |
|---|---|---|
| `queued` | Sync requested but not started | Worker starts → running |
| `running` | Files are being crawled and indexed | Complete, partial, failed, rate limited |
| `partial` | Some results available with skipped/errors | Retry failed or Open library |
| `complete` | Metadata cache is current | Open library |
| `failed` | Blocking error stopped sync | Retry or Connection Recovery |
| `rate_limited` | Cloud provider paused requests | Wait or open cached results |

### Actions

| Action | Description | Precondition |
|---|---|---|
| Open library | Opens cached Library Home | At least one book indexed |
| Retry failed items | Queues another sync for failures | Partial or failed state |
| Cancel sync | Stops active sync after confirmation | Running or queued |
| View details | Opens diagnostics in Settings or Recovery | Errors or skipped files exist |
| Back to sources | Opens Settings and Sources | Always |

### Data required

| Data | Source | Offline | Notes |
|---|---|---|---|
| Sync state | WorkManager/local DB | Cached | Main status |
| Files scanned | Local job state | Cached | Running progress |
| Books and chapters found | Local DB | Cached | Enables Open library |
| Skipped files/errors | Local DB/job log | Cached | Source-specific diagnostics |
| Provider rate-limit retry time | Provider API/job state | Cached | Cloud-only |

### Visual indicators

| Indicator | Meaning |
|---|---|
| Progress rail | Sync is active and moving |
| Healthy badge | Complete with no blocking errors |
| Warning badge | Partial or rate limited |
| Error panel | Failed sync requiring action |

### Critical variants

**Running with partial results**
Show live counts and make Open library available once useful cached results exist.

**Partial sync**
Show results as usable, not failed, while separating skipped files and retry actions.

**Failed sync**
Name the source-specific cause and route to Connection Recovery when playback/browsing cannot proceed.

### Layout regions

| Region | Purpose | Required content |
|---|---|---|
| Header | Show source and indexing status | Source name, last updated, status badge |
| Progress summary | Communicate whether cache is usable | Progress rail, counts, running/partial/complete copy |
| Results panel | Show indexed books/chapters | Books found, chapters found, skipped files |
| Diagnostics panel | Explain recoverable problems | Error list, provider or SMB details |
| Action row | Continue or recover | Open library, Retry, Cancel, View details |

### Content

| Element | Copy |
|---|---|
| Page title | Indexing audiobook library |
| Primary action label | Open library |
| Empty state heading | No books indexed yet |
| Empty state body | The app is still scanning this source. Books will appear here as they are cached. |
| Empty state CTA | Keep scanning |
| Error heading | Sync stopped |
| Error body | The app could not finish indexing this source. Cached results remain available if any were found. |
| Offline message | Source is unreachable. Cached results can still be opened if they exist. |

### Screen generation prompt

> Target tool: other / native Android TV prototype

Create a TV-safe sync status dashboard that shows background audiobook metadata indexing after a user selects a source folder. The screen must make the cache promise clear: browsing comes from locally indexed books, and the user may open partial results once books are found. Show sync states for queued, running, partial, complete, failed, and provider rate-limited, with specific counts for files scanned, books found, chapters found, skipped files, and source errors. Include clear D-pad actions for Open library, Retry failed items, Cancel sync, and View details, and use distinct healthy, warning, and error indicators without implying that partial results are unusable.

### Acceptance criteria

- [ ] Harvey or Chloe can tell whether cached library browsing is available.
- [ ] Running, partial, complete, failed, and rate-limited states are visually distinct.
- [ ] Open library appears only when cached book data exists.
- [ ] Skipped files and sync errors are separated from successful indexed counts.
- [ ] Retry and recovery actions are source-specific.
- [ ] The screen explains that sync is background metadata indexing, not local downloading.

### Heuristic check

All core heuristic items are defined for this screen. Heuristic gaps: None.
