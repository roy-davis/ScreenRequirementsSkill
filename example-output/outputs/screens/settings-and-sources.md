> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Settings and Sources

> File: outputs/screens/settings-and-sources.md
> Screen ID: PG08

| Attribute | Value |
|---|---|
| Route | `/settings/sources` |
| Access | Source owner, listener |
| Mobile | Not required |
| Scope | In scope |

### Design intent

- Primary purpose: Manage connected sources, sync health, recovery actions, and playback preferences.
- Primary actors: Harvey the Home Server Hobbyist; Chloe the Cloud Collector.
- Primary job: Understand source health, add or recover sources, run sync, and adjust audiobook playback defaults.
- Principle served: Diagnostics without clutter; technical detail belongs here, not on the listening path.
- Entry points: Library Home settings, Sync Status, Connection Recovery details.
- Success outcome: User can add, resync, reauthorize, edit, or remove a source safely.
- Content priority: Primary: source list and health. Secondary: sync controls, playback defaults, diagnostics.
- Critical variants: Healthy sources; degraded source; reauthorization required; no sources; remove source confirmation.
- Exit paths: Source Setup, Sync Status, Connection Recovery, Library Home.

### Related flows

- harvey-connects-home-library.md: Lets Harvey manage SMB source settings and sync.
- chloe-connects-cloud-library.md: Lets Chloe reauthorize cloud providers and manage folders.

### Decisions on this screen

| ID | Decision | Stakes | Reversible |
|---|---|---|---|
| D1 | Remove source | High | No |
| D2 | Start resync | Med | Yes |
| D3 | Reauthorize provider or edit credentials | Med | Yes |

**Remove source — D1**
- What the user is deciding: Whether to delete source configuration and related credentials/tokens.
- Options: Cancel, remove source.
- Information required: Source name, provider/host, what will be deleted, whether cached progress remains.
- Risk if wrong: User loses source setup and must reconnect.
- Reversible: No — Recovery: Add source again.
- Recommended default: Cancel.
- Conditions that change this: Removal is unavailable while a modal confirmation is not accepted.

### States

| State | Description | Transitions |
|---|---|---|
| `healthy` | Sources connected with current or completed sync | Resync, edit, remove |
| `degraded` | Source has warnings or partial sync | Retry, recovery, open cache |
| `reauth_required` | Cloud source needs provider reauthorization | Reauthorize → Provider Auth |
| `no_sources` | No source configured | Add source → Source Setup |
| `confirm_remove` | Destructive confirmation visible | Cancel or remove |

### Actions

| Action | Description | Precondition |
|---|---|---|
| Add source | Opens Source Setup | Always |
| Resync | Starts Sync Status for selected source | Source configured |
| Reauthorize | Opens Provider Auth for selected cloud source | Reauthorization required or user chooses |
| Edit credentials | Opens Provider Auth for SMB/source config | Source supports editable details |
| Remove source | Opens confirmation | Source selected |
| Update playback defaults | Adjust speed and skip preferences | Always |

### Data required

| Data | Source | Offline | Notes |
|---|---|---|---|
| Source list | Local database/secure metadata refs | Cached | Do not expose secret values |
| Source health | Local DB/source probe | Cached/Real-time | Main status |
| Last sync and counts | Local database | Cached | Trust signal |
| Playback preferences | Local preferences | Local | Default speed/skip |
| Provider account labels | Secure metadata/provider | Cached | No tokens shown |

### Visual indicators

| Indicator | Meaning |
|---|---|
| Source status badge | Healthy, syncing, degraded, reauth required |
| Focused source panel | Current source being managed |
| Destructive color on remove | Irreversible source deletion |
| Preference value chip | Current default speed or skip setting |

### Critical variants

**Reauthorization required**
Cloud source remains listed with cached data status, but playback/stream resolution actions route to Reauthorize.

**Remove confirmation**
Use a TV-safe modal requiring focus on Cancel or Remove source, with Cancel as the default focus.

**No sources**
Show Add source as primary and avoid empty diagnostics.

### Layout regions

| Region | Purpose | Required content |
|---|---|---|
| Source list | Select a source to manage | Source name, type, status, last sync |
| Source detail | Show health and available actions | Counts, account/host, sync errors, credentials/token status |
| Playback preferences | Manage global listening defaults | Speed, skip interval, progress save explanation |
| Diagnostics | Technical detail when needed | Last error, provider capability, retry details |
| Confirmation modal | Prevent destructive mistakes | Remove source explanation, Cancel, Remove |

### Content

| Element | Copy |
|---|---|
| Page title | Settings and sources |
| Primary action label | Add source |
| Empty state heading | No sources connected |
| Empty state body | Add a network share or cloud folder to start building your audiobook library. |
| Empty state CTA | Add source |
| Error heading | Source needs attention |
| Error body | This source has a recoverable issue. Cached library data remains on this device. |
| Offline message | Some source checks require network access. Cached settings are still available. |

### Screen generation prompt

> Target tool: other / native Android TV prototype

Create a settings and source management screen for an Android TV audiobook player. The screen should use a left source list and a focused detail area to show connected SMB and cloud sources, health, last sync, books/chapters indexed, account or host label, provider capabilities, and actions for Add source, Resync, Reauthorize, Edit credentials, and Remove source. It must also include compact audiobook playback defaults such as speed and skip interval. Handle healthy, degraded, reauthorization-required, no-source, and remove-confirmation states with TV-safe focus, no exposed credential/token values, and a destructive confirmation that defaults to Cancel.

### Acceptance criteria

- [ ] Source health and last sync are visible for each connected source.
- [ ] Reauthorization-required state offers a clear Reauthorize action.
- [ ] Remove source requires confirmation and does not expose secret values.
- [ ] Resync leads to Sync Status and is disabled when source state blocks it.
- [ ] Playback defaults are visible but secondary to source management.
- [ ] No-source state provides Add source as the primary action.

### Heuristic check

All core heuristic items are defined for this screen. Heuristic gaps: None.
