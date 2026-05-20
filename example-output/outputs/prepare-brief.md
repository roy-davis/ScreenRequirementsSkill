> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Prototype Build Brief

> File: outputs/prepare-brief.md
> Self-contained generation brief for v0, Lovable, Bolt, Claude Design, Google Stitch. Use alongside outputs/design.md.

## Overview

**Product:** Local & Cloud Network Audiobook Player for Android TV
**Goal:** Let a TV user connect a remote audiobook source, index it into a local metadata cache, browse cached books instantly, and resume playback at the exact saved position.
**Fidelity:** Mid-fi to hi-fi interaction prototype.
**Target tool:** Other / native Android TV prototype (assumed because no generation target was supplied).
**Primary flow:** harvey-resumes-a-long-book.md
**Screens:** PG01, PG02, PG03, PG04, PG05, PG06, PG07, PG08, PG09

## Design system summary

- Framework: Native Android TV using Jetpack Compose for TV.
- Component library + version: androidx.tv:tv-material, current stable compatible with target Android TV build.
- Styling: Kotlin theme tokens with Material-style semantic colors.
- Dark mode: Dark-first.
- Navigation: D-pad focus with 1.06 scale, primary outline, and overscan-safe layout.
- Primary color: #38C7A1 on dark background.
- Border radius: 8 dp default.
- Font: Roboto / system sans.

(Full detail in outputs/design.md)

## Screens to build

| Order | ID | Name | Spec file | Entry? | Notes |
|---|---|---|---|---|---|
| 1 | PG01 | Source Setup | outputs/screens/source-setup.md | Yes | First-run entry; source choice |
| 2 | PG02 | Provider Auth | outputs/screens/provider-auth.md | No | SMB credentials or cloud auth |
| 3 | PG03 | Folder Picker | outputs/screens/folder-picker.md | No | Select source root |
| 4 | PG04 | Sync Status | outputs/screens/sync-status.md | No | Background metadata crawl |
| 5 | PG05 | Library Home | outputs/screens/library-home.md | Yes | Cached browse and resume |
| 6 | PG06 | Book Detail | outputs/screens/book-detail.md | No | Chapters and progress |
| 7 | PG07 | Now Playing | outputs/screens/now-playing.md | Yes | Restored playback session |
| 8 | PG08 | Settings and Sources | outputs/screens/settings-and-sources.md | No | Source management |
| 9 | PG09 | Connection Recovery | outputs/screens/connection-recovery.md | No | Source and playback recovery |

## Core flow

1. User at PG05 Library Home sees "Continue listening" for The Long Way to a Small Angry Planet, Chapter 18, 12:43:09 elapsed.
2. Resume → PG07 Now Playing.
3. Playback enters Loading while the app resolves the playable asset reference to an SMB URI.
4. Stream ready → PG07 Playing with progress rail, skip controls, speed 1.25x, and "Position saved" status.
5. User opens Book detail → PG06 Book Detail with current chapter highlighted.
6. User returns to Library Home → PG05 with cached grid still available.
7. If source resolution fails → PG09 Connection Recovery with source-specific retry or credential action.

## Mock data

### Sources

| Field | Value |
|---|---|
| SMB source name | Living Room NAS |
| SMB host | 192.168.1.42 |
| SMB share | /Audiobooks |
| SMB status | Connected |
| Cloud source name | Chloe's Google Drive |
| Cloud account label | chloe.reader@example.com |
| Cloud folder | /Audiobooks/Finished Series |
| Cloud status | Reauthorization required |
| Last sync | Today, 7:42 PM |

### Books

| Field | Value |
|---|---|
| Current book | The Long Way to a Small Angry Planet |
| Author | Becky Chambers |
| Current chapter | Chapter 18 — Port Coriol |
| Position | 12:43:09 of 14:12:55 |
| Progress | 89% |
| Source | Living Room NAS |
| Second book | Project Hail Mary |
| Second author | Andy Weir |
| Second progress | 34% |
| Third book | The Fifth Season |
| Third author | N. K. Jemisin |
| Third progress | Not started |

### Sync

| Field | Value |
|---|---|
| Files scanned | 1,248 |
| Books found | 86 |
| Chapters found | 1,104 |
| Skipped files | 7 |
| Current sync state | Running with partial results |
| Provider rate limit retry | 18 minutes |

## States to show

| Screen | State | How |
|---|---|---|
| PG01 | Offline cloud setup | Cloud providers disabled; SMB available |
| PG02 | Cloud pending auth | Provider code, countdown, waiting state |
| PG03 | Permission denied | Folder row disabled with recovery copy |
| PG04 | Partial sync | Indexed counts plus skipped file warning |
| PG05 | Source unreachable with cache | Cached books visible; playback recovery needed |
| PG06 | Ready with progress | Resume focused, current chapter highlighted |
| PG07 | Buffering | Controls visible, buffer/source status shown |
| PG08 | Reauthorization required | Cloud source calls for reauthorize |
| PG09 | Cloud token expired | Reauthorize primary; Return to library secondary |

## Implement

| From | Trigger | To |
|---|---|---|
| PG01 | Select SMB or provider | PG02 |
| PG02 | Connection/auth success | PG03 |
| PG02 | Failure details | PG09 |
| PG03 | Use this folder | PG04 |
| PG04 | Open library | PG05 |
| PG05 | Resume | PG07 |
| PG05 | Open book | PG06 |
| PG06 | Resume/start/chapter | PG07 |
| PG07 | Playback failure | PG09 |
| PG05 | Settings | PG08 |
| PG08 | Add source | PG01 |
| PG08 | Resync | PG04 |
| PG09 | Reauthorize/edit credentials | PG02 |
| PG09 | Return to library | PG05 |

## Stub (appear but not function)
- Provider OAuth completion on PG02: simulate approved, denied, and expired states.
- Actual SMB connection on PG02: simulate success/failure.
- Real background WorkManager sync on PG04: animate counts and state changes.
- Real Media3 playback on PG07: simulate buffering/playing/paused/error.
- Remove source on PG08: show confirmation only.

## Edge cases

| Case | Screen | How |
|---|---|---|
| No internet during cloud setup | PG01/PG02 | Disable cloud continuation and explain internet requirement |
| SMB auth failure | PG02/PG09 | Show Edit credentials as primary recovery |
| Folder permission denied | PG03/PG09 | Explain current folder cannot be listed; choose another folder |
| Provider rate limit | PG04 | Show retry estimate and cached/partial results |
| Source unreachable but cache exists | PG05 | Allow browse, block playback, show recovery |
| Missing chapter asset | PG06/PG09 | Mark chapter unavailable and offer resync |
| Buffer underrun | PG07 | Show buffering without hiding transport controls |
| Cloud token expired | PG08/PG09 | Reauthorize primary; cached library remains available |

## Do not
- Generate screens not in the inventory.
- Use components not in outputs/design.md.
- Use lorem ipsum.
- Add navigation not in the design context.
- Treat WebDAV as the implementation path for Dropbox, Google Drive, iCloud Drive, or OneDrive.
- Hide cached library browsing just because a source is currently unreachable.
- Show raw credentials, OAuth tokens, refresh tokens, or app passwords.

## Acceptance
- [ ] All nine screens render.
- [ ] Primary resume flow completes end to end from Library Home to Now Playing.
- [ ] Setup flow completes from Source Setup to Sync Status.
- [ ] Visual style is consistent with outputs/design.md requirements.
- [ ] Empty, loading, degraded, and error states are present on each relevant screen.
- [ ] TV layout stays within overscan-safe margins and supports D-pad focus.
- [ ] Realistic mock data appears throughout.
