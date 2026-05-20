> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Connection Recovery

> File: outputs/screens/connection-recovery.md
> Screen ID: PG09

| Attribute | Value |
|---|---|
| Route | `/recovery/:sourceId` |
| Access | Source owner, listener |
| Mobile | Not required |
| Scope | In scope |

### Design intent

- Primary purpose: Explain a source, authorization, sync, or playback failure and present the next safe recovery action.
- Primary actors: Harvey the Home Server Hobbyist; Chloe the Cloud Collector.
- Primary job: Recover from an unreachable SMB share, expired cloud token, folder permission issue, sync failure, or playback stream failure.
- Principle served: Specific recovery beats generic error handling.
- Entry points: Provider Auth, Folder Picker, Sync Status, Book Detail, Now Playing, Settings and Sources.
- Success outcome: User retries successfully, edits credentials, reauthorizes, resyncs, or returns to cached library with a clear limitation.
- Content priority: Primary: what failed, why it likely failed, what to do next. Secondary: diagnostics and cached-data availability.
- Critical variants: SMB unreachable; SMB auth failed; cloud token expired; cloud provider unavailable; folder permission denied; playback asset missing; unsupported format.
- Exit paths: Provider Auth, Sync Status, Book Detail, Library Home, Settings and Sources.

### Related flows

- harvey-connects-home-library.md: Handles SMB connection, folder, and sync failures.
- harvey-resumes-a-long-book.md: Handles stream/playback recovery.
- chloe-connects-cloud-library.md: Handles provider authorization and rate-limit recovery.

### Decisions on this screen

| ID | Decision | Stakes | Reversible |
|---|---|---|---|
| D1 | Choose recovery path | High | Yes |

**Choose recovery path — D1**
- What the user is deciding: Which recovery action should be attempted.
- Options: Retry, edit credentials, reauthorize provider, resync, choose another folder, return to cached library.
- Information required: Source type, failure category, last successful sync, whether cached browsing is still available.
- Risk if wrong: User may repeat a failing action or lose context while audio is interrupted.
- Reversible: Yes — Recovery: Back returns to the previous screen or cached library when available.
- Recommended default: Retry for transient network errors; Reauthorize for token errors; Edit credentials for authentication errors.
- Conditions that change this: If cached data exists, Return to library should remain available even when playback is blocked.

### States

| State | Description | Transitions |
|---|---|---|
| `smb_unreachable` | Host/share cannot be reached | Retry or edit credentials |
| `smb_auth_failed` | Credentials rejected | Edit credentials |
| `cloud_token_expired` | Provider token cannot refresh | Reauthorize |
| `provider_unavailable` | Cloud provider/API unavailable | Retry later or return to cache |
| `folder_permission` | Selected folder cannot be listed/read | Choose another folder or edit access |
| `asset_missing` | Saved chapter reference no longer resolves | Resync or choose another chapter |
| `unsupported_format` | File exists but cannot be played | Back to book or view sync details |

### Actions

| Action | Description | Precondition |
|---|---|---|
| Retry | Reattempts failed action | Failure may be transient |
| Edit credentials | Opens Provider Auth | SMB/auth details editable |
| Reauthorize | Opens Provider Auth cloud variant | Cloud token invalid |
| Choose another folder | Opens Folder Picker | Source connection works |
| Resync | Opens Sync Status and queues sync | Source reachable |
| Return to library | Opens cached Library Home | Cached books exist |
| Back | Returns to previous screen | Always |

### Data required

| Data | Source | Offline | Notes |
|---|---|---|---|
| Failure type | System/source resolver | Cached | Drives specific copy |
| Source type/account/host | Local metadata | Cached | No secret values |
| Last successful sync | Local database | Cached | Tells user what remains usable |
| Cached book count | Local database | Cached | Enables Return to library |
| Retry eligibility | System/provider response | Cached/Real-time | Determines primary action |

### Visual indicators

| Indicator | Meaning |
|---|---|
| Error category badge | Network, authorization, permission, asset, format |
| Cached data available badge | Library can still be browsed |
| Primary recovery action focus | Recommended next step |
| Diagnostic detail panel | Technical details available without dominating screen |

### Critical variants

**SMB unreachable**
Explain host/share/network as likely issue and offer Retry plus Edit credentials.

**Cloud token expired**
Explain that cached library remains but new stream URLs need reauthorization; offer Reauthorize as primary.

**Playback asset missing**
Explain that the book record is cached but the source file/path no longer resolves; offer Resync or return to Book Detail.

### Layout regions

| Region | Purpose | Required content |
|---|---|---|
| Problem summary | Explain failure plainly | Source name, failure category, human explanation |
| Recovery actions | Let user act immediately | Recommended primary action and safe secondary actions |
| Cached availability | Preserve trust in cache | Last sync, cached book count, what still works |
| Diagnostic details | Help technical users troubleshoot | Host/provider, last error, retry time |
| Navigation row | Escape without losing context | Back, Return to library, Settings |

### Content

| Element | Copy |
|---|---|
| Page title | Source needs attention |
| Primary action label | Retry |
| Empty state heading | No recovery action available |
| Empty state body | Return to your cached library or adjust this source from settings. |
| Empty state CTA | Return to library |
| Error heading | Playback cannot continue |
| Error body | The current chapter stream could not be resolved from this source. |
| Offline message | Cached library data is still on this device, but playback needs the source to reconnect. |

### Screen generation prompt

> Target tool: other / native Android TV prototype

Create a focused Android TV recovery screen that replaces generic errors with source-specific guidance. The screen must state what failed, why it likely failed, what cached data remains usable, and the recommended next action for SMB unreachable, SMB auth failed, cloud token expired, provider unavailable, folder permission denied, missing asset, and unsupported format. Provide large D-pad actions for Retry, Edit credentials, Reauthorize, Choose another folder, Resync, Return to library, and Back, showing only actions that make sense for the failure state. Keep diagnostics available for Harvey without overwhelming Chloe, never expose secrets, and always preserve a path back to cached browsing when cached books exist.

### Acceptance criteria

- [ ] Each failure category has specific copy and a recommended recovery action.
- [ ] Cached-data availability is visible when cached books exist.
- [ ] Cloud token expiration routes to Reauthorize, not generic Retry only.
- [ ] SMB credential failures route to Edit credentials.
- [ ] Missing asset and unsupported format are distinguished.
- [ ] The user can leave recovery without losing access to cached library browsing.
- [ ] Diagnostic details do not expose credentials or tokens.

### Heuristic check

All core heuristic items are defined for this screen. Heuristic gaps: None.
