> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Workflow State Model

> File: outputs/data/states.md

## Objects

| Object | States | Terminal states |
|---|---|---|
| Source | Unconfigured, Configuring, Authorizing, Connected, Syncing, Degraded, Reauthorization Required, Unreachable, Removed | Removed |
| Sync Job | Not Started, Queued, Running, Partial, Complete, Failed, Paused Rate Limited, Cancelled | Complete, Failed, Cancelled |
| Book | Discovered, Indexed, Partially Indexed, Ready, Unavailable, Removed | Removed |
| Chapter | Indexed, Resolving, Playable, Buffered, Failed, Skipped Unsupported | Skipped Unsupported |
| Playback Session | Idle, Loading, Playing, Buffering, Paused, Background Playing, Ended, Error | Ended |
| Playable Asset | Stored Reference, Resolving, Stream Ready, Expired, Missing, Unsupported | Missing, Unsupported |

---

## Object: Source

### States

| ID | Name | Description | Entry | Exit |
|---|---|---|---|---|
| SRC1 | Unconfigured | No source exists or user is adding one | First launch or Add source | Source type selected |
| SRC2 | Configuring | Source details are being entered or folder is being selected | Source type selected | Details validated or cancelled |
| SRC3 | Authorizing | Cloud provider authorization is pending | Provider auth started | Token granted, denied, or expired |
| SRC4 | Connected | Source credentials/tokens are valid and source can be listed | Connection succeeds | Sync starts, token expires, source removed |
| SRC5 | Syncing | Source is connected and metadata crawl is active | Sync starts | Sync completes, partially completes, or fails |
| SRC6 | Degraded | Source has partial results or recoverable listing/playback issues | Partial sync or transient source error | Retry succeeds, reauth required, source removed |
| SRC7 | Reauthorization Required | Cloud token cannot refresh silently | Token refresh fails | Reauthorization succeeds or source removed |
| SRC8 | Unreachable | Network host/provider cannot be reached | Connection attempt fails | Retry succeeds or source removed |
| SRC9 | Removed | Source and related credentials are deleted | Remove source confirmed | Terminal |

### Transitions

| From | Trigger | To | Actor | Guard |
|---|---|---|---|---|
| SRC1 | Select source type | SRC2 | User | Source type supported |
| SRC2 | Start cloud authorization | SRC3 | User | Provider selected |
| SRC3 | Authorization approved | SRC4 | Provider | Token stored securely |
| SRC2 | SMB connection succeeds | SRC4 | System | Credentials valid |
| SRC4 | Start sync | SRC5 | User/System | Folder selected |
| SRC5 | Sync completes | SRC4 | System | No blocking errors |
| SRC5 | Sync partial | SRC6 | System | Some files unavailable or skipped |
| SRC4 | Token refresh fails | SRC7 | System | Cloud provider source |
| SRC4 | Host/provider unreachable | SRC8 | System | Network unavailable or host down |
| SRC6 | Retry succeeds | SRC4 | User/System | Source reachable |
| Any non-terminal | Remove source | SRC9 | User | Confirmation accepted |

### Actions per state

| State | Allowed | Blocked | Hidden |
|---|---|---|---|
| Unconfigured | Choose source type | Play audiobook | Remove source |
| Configuring | Edit details, test connection, cancel | Start sync until required fields valid | Play controls |
| Authorizing | Restart authorization, cancel | Browse provider folders until authorized | SMB credential fields for cloud providers |
| Connected | Browse folder, start sync, play indexed books, edit source | None when healthy | Reauthorize unless cloud token requires it |
| Syncing | Open partial library, pause/cancel sync, view errors | Remove source without confirmation | None |
| Degraded | Retry, view diagnostics, open cached library | Play unavailable assets | None |
| Reauthorization Required | Reauthorize, open cached library | Resolve new cloud streams | SMB credential fields |
| Unreachable | Retry, edit source, open cached library | Start playback from unreachable source | Provider auth controls for SMB |
| Removed | None | All source actions | Source controls |

### Diagram

```text
[SRC1: Unconfigured] --(select source)--> [SRC2: Configuring]
[SRC2: Configuring] --(cloud auth)--> [SRC3: Authorizing]
[SRC2: Configuring] --(SMB valid)--> [SRC4: Connected]
[SRC3: Authorizing] --(approved)--> [SRC4: Connected]
[SRC4: Connected] --(start sync)--> [SRC5: Syncing]
[SRC5: Syncing] --(partial errors)--> [SRC6: Degraded]
[SRC4: Connected] --(token refresh fails)--> [SRC7: Reauthorization Required]
[SRC4: Connected] --(network failure)--> [SRC8: Unreachable]
```

### Terminal states

| State | Description | UI behaviour |
|---|---|---|
| Removed | Source, credentials, tokens, and account metadata have been deleted | Source disappears from settings after confirmation |

---

## Object: Sync Job

### States

| ID | Name | Description | Entry | Exit |
|---|---|---|---|---|
| SYN1 | Not Started | Source exists but no crawl has begun | Source saved | Sync queued |
| SYN2 | Queued | WorkManager job is waiting | Sync requested | Worker starts |
| SYN3 | Running | Folder crawl and metadata indexing are active | Worker starts | Complete, partial, failed, paused |
| SYN4 | Partial | Some books indexed while some files failed | Worker exits with recoverable errors | Retry or complete |
| SYN5 | Complete | Crawl finished and cache is current | Worker finishes successfully | New sync requested |
| SYN6 | Failed | Crawl cannot continue | Blocking error | Retry |
| SYN7 | Paused Rate Limited | Provider throttled or asked app to retry later | Rate limit encountered | Retry window elapsed |
| SYN8 | Cancelled | User cancelled current sync | Cancel action | New sync requested |

### Transitions

| From | Trigger | To | Actor | Guard |
|---|---|---|---|---|
| SYN1 | Request sync | SYN2 | User/System | Source connected |
| SYN2 | Worker starts | SYN3 | System | Background execution available |
| SYN3 | All supported files indexed | SYN5 | System | No blocking errors |
| SYN3 | Some files skipped | SYN4 | System | Partial result usable |
| SYN3 | Provider rate limit | SYN7 | System | Retry-after exists |
| SYN3 | Blocking source error | SYN6 | System | Cannot list or resolve files |
| SYN3 | Cancel sync | SYN8 | User | Confirmation accepted |
| SYN4 | Retry failed items | SYN2 | User/System | Source reachable |
| SYN7 | Retry window elapsed | SYN2 | System | Source still authorized |

### Actions per state

| State | Allowed | Blocked | Hidden |
|---|---|---|---|
| Not Started | Start sync | Open library if no cached books | Cancel sync |
| Queued | Cancel sync | Start duplicate sync | None |
| Running | View progress, open partial library, cancel | Edit source root | None |
| Partial | Retry failed, open library, view skipped files | Claim source fully current | None |
| Complete | Open library, resync | Cancel sync | None |
| Failed | Retry, edit source, view diagnostics | Open empty library as if healthy | None |
| Paused Rate Limited | View retry time, open cached library | Force immediate provider calls | None |
| Cancelled | Start sync | Resume cancelled job | None |

### Diagram

```text
[SYN1: Not Started] --(request sync)--> [SYN2: Queued]
[SYN2: Queued] --(worker starts)--> [SYN3: Running]
[SYN3: Running] --(success)--> [SYN5: Complete]
[SYN3: Running] --(recoverable errors)--> [SYN4: Partial]
[SYN3: Running] --(rate limit)--> [SYN7: Paused Rate Limited]
[SYN3: Running] --(blocking error)--> [SYN6: Failed]
```

### Terminal states

| State | Description | UI behaviour |
|---|---|---|
| Complete | Metadata cache is current for the selected folder | Show current timestamp and healthy status |
| Failed | Current sync ended with a blocking error | Keep cached results visible but show recovery state |
| Cancelled | User stopped the current sync | Show cancelled state and Start sync action |

---

## Object: Playback Session

### States

| ID | Name | Description | Entry | Exit |
|---|---|---|---|---|
| PB1 | Idle | No active playback | App launch or stop | Resume selected |
| PB2 | Loading | Asset reference is resolving and buffer is preparing | Resume or chapter selected | Playing, buffering, or error |
| PB3 | Playing | Audio is playing in foreground | Buffer ready | Pause, buffer underrun, background, ended |
| PB4 | Buffering | Player is waiting for enough data | Network delay or seek | Playing or error |
| PB5 | Paused | Playback stopped with position saved | Pause selected | Resume or stop |
| PB6 | Background Playing | Audio continues outside app UI | User leaves app while playing | Foreground return, pause, ended |
| PB7 | Ended | Book or chapter finished | Playback reaches end | Replay or next selection |
| PB8 | Error | Playback cannot continue | Resolution or stream failure | Retry or recovery |

### Transitions

| From | Trigger | To | Actor | Guard |
|---|---|---|---|---|
| PB1 | Resume | PB2 | User | Cached progress exists |
| PB2 | Stream ready | PB3 | System | Asset resolves |
| PB2 | Stream delayed | PB4 | System | Buffer below threshold |
| PB3 | Pause | PB5 | User/System | Position saved |
| PB3 | Leave app | PB6 | User/System | MediaSession active |
| PB6 | Return to app | PB3 | User | Session active |
| PB3 | Buffer underrun | PB4 | System | Network slow |
| PB4 | Buffer recovered | PB3 | System | Buffer threshold reached |
| PB2/PB4 | Resolution fails | PB8 | System | Source or asset unavailable |
| PB3 | Track/book complete | PB7 | System | Duration reached |

### Actions per state

| State | Allowed | Blocked | Hidden |
|---|---|---|---|
| Idle | Resume, choose book | Pause, seek | Speed controls unless a book is selected |
| Loading | Cancel, view source status | Skip, speed change until stream starts | None |
| Playing | Pause, seek, skip, speed, chapter list | Remove source | None |
| Buffering | Pause, cancel, view status | Repeated seek until buffer recovers | None |
| Paused | Resume, seek, speed, exit | Background playback claim | None |
| Background Playing | Pause from system controls, return to app | Source editing | Detailed settings |
| Ended | Replay, next chapter/book, library | Save position beyond duration | None |
| Error | Retry, open recovery, back to book | Continue playback | Speed controls |

### Diagram

```text
[PB1: Idle] --(resume)--> [PB2: Loading]
[PB2: Loading] --(stream ready)--> [PB3: Playing]
[PB3: Playing] --(pause)--> [PB5: Paused]
[PB3: Playing] --(leave app)--> [PB6: Background Playing]
[PB3: Playing] --(buffer underrun)--> [PB4: Buffering]
[PB4: Buffering] --(recovered)--> [PB3: Playing]
[PB2: Loading] --(failure)--> [PB8: Error]
```

### Terminal states

| State | Description | UI behaviour |
|---|---|---|
| Ended | Playback reached chapter or book end | Show completion state and next action |
