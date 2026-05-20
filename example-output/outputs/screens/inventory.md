> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Screen Inventory

> File: outputs/screens/inventory.md

Total: 9 | In scope: 9 | Out of scope: 0

## Screen list

| ID | Name | Type | Priority | Scope | File | Flow | Persona | State |
|---|---|---|---|---|---|---|---|---|
| PG01 | Source Setup | Form | Must-have | In | source-setup.md | harvey-connects-home-library.md; chloe-connects-cloud-library.md | Harvey; Chloe | Source: Unconfigured |
| PG02 | Provider Auth | Auth | Must-have | In | provider-auth.md | harvey-connects-home-library.md; chloe-connects-cloud-library.md | Harvey; Chloe | Source: Configuring/Authorizing |
| PG03 | Folder Picker | Form | Must-have | In | folder-picker.md | harvey-connects-home-library.md; chloe-connects-cloud-library.md | Harvey; Chloe | Source: Configuring |
| PG04 | Sync Status | Dashboard | Must-have | In | sync-status.md | harvey-connects-home-library.md; chloe-connects-cloud-library.md | Harvey; Chloe | Sync Job: Running/Partial/Complete |
| PG05 | Library Home | Dashboard | Must-have | In | library-home.md | harvey-resumes-a-long-book.md | Harvey; Chloe | Source: Connected/Degraded |
| PG06 | Book Detail | Detail | Must-have | In | book-detail.md | harvey-resumes-a-long-book.md | Harvey; Chloe | Book: Ready/Unavailable |
| PG07 | Now Playing | Detail | Must-have | In | now-playing.md | harvey-resumes-a-long-book.md | Harvey; Chloe | Playback Session: Loading/Playing/Buffering/Paused/Error |
| PG08 | Settings and Sources | Settings | Should-have | In | settings-and-sources.md | harvey-connects-home-library.md; chloe-connects-cloud-library.md | Harvey; Chloe | Source: Connected/Degraded/Reauthorization Required |
| PG09 | Connection Recovery | Error | Must-have | In | connection-recovery.md | all flows | Harvey; Chloe | Source: Unreachable/Reauthorization Required; Playback Session: Error |

> The File column has been filled by spec.oneshot because this run generated the screen files in the same pass.

---

## Source Setup — PG01

**Type:** Form | **Priority:** Must-have | **Scope:** In
**Flow:** harvey-connects-home-library.md; chloe-connects-cloud-library.md | **Journey stage:** Choose source type / Choose trusted provider | **Persona:** Harvey the Home Server Hobbyist; Chloe the Cloud Collector | **State shown:** Source: Unconfigured

**Purpose:** Let a TV user choose SMB, a first-class cloud provider, or advanced WebDAV as the source for an audiobook library.
**Entry points:** First launch, Settings and Sources Add source.
**Exit points:** Provider Auth, Settings and Sources, Back/cancel.
**Notes:** WebDAV is advanced/custom and must not be presented as the implementation path for branded cloud providers.

## Provider Auth — PG02

**Type:** Auth | **Priority:** Must-have | **Scope:** In
**Flow:** harvey-connects-home-library.md; chloe-connects-cloud-library.md | **Journey stage:** Enter network details / Authorize account | **Persona:** Harvey; Chloe | **State shown:** Source: Configuring or Authorizing

**Purpose:** Collect SMB credentials or guide provider authorization with clear security and recovery messaging.
**Entry points:** Source Setup provider selection.
**Exit points:** Folder Picker, Connection Recovery, Source Setup.
**Notes:** Remote text entry must be minimized and provider OAuth should favor secondary-device authorization.

## Folder Picker — PG03

**Type:** Form | **Priority:** Must-have | **Scope:** In
**Flow:** harvey-connects-home-library.md; chloe-connects-cloud-library.md | **Journey stage:** Pick library folder / Pick cloud folder | **Persona:** Harvey; Chloe | **State shown:** Source: Configuring

**Purpose:** Allow the user to choose a source folder as the audiobook library root.
**Entry points:** Successful SMB connection or provider authorization.
**Exit points:** Sync Status, Provider Auth, Connection Recovery.
**Notes:** Shows provider/source breadcrumbs and folder accessibility without triggering a full library crawl.

## Sync Status — PG04

**Type:** Dashboard | **Priority:** Must-have | **Scope:** In
**Flow:** harvey-connects-home-library.md; chloe-connects-cloud-library.md | **Journey stage:** Index and verify / Sync cloud metadata | **Persona:** Harvey; Chloe | **State shown:** Sync Job: Running, Partial, Complete, Failed

**Purpose:** Show background indexing progress, partial results, skipped files, and next actions.
**Entry points:** Folder Picker confirmation, Settings resync.
**Exit points:** Library Home, Connection Recovery, Settings and Sources.
**Notes:** Must communicate that cached results can be browsed once available.

## Library Home — PG05

**Type:** Dashboard | **Priority:** Must-have | **Scope:** In
**Flow:** harvey-resumes-a-long-book.md | **Journey stage:** Return to current book / Browse from cache | **Persona:** Harvey; Chloe | **State shown:** Source: Connected or Degraded

**Purpose:** Provide instant cached library browsing with a prominent continue-listening path.
**Entry points:** App launch, Sync Status, Back from Book Detail or Now Playing.
**Exit points:** Book Detail, Now Playing, Source Setup, Settings and Sources.
**Notes:** Browsing is local-cache first; source health is visible but secondary.

## Book Detail — PG06

**Type:** Detail | **Priority:** Must-have | **Scope:** In
**Flow:** harvey-resumes-a-long-book.md | **Journey stage:** Inspect book context | **Persona:** Harvey; Chloe | **State shown:** Book: Ready or Unavailable

**Purpose:** Show book progress, chapters, source status, and a clear resume/start action.
**Entry points:** Library Home book card.
**Exit points:** Now Playing, Library Home, Connection Recovery.
**Notes:** Must make long-form chapter order and current position understandable from across the room.

## Now Playing — PG07

**Type:** Detail | **Priority:** Must-have | **Scope:** In
**Flow:** harvey-resumes-a-long-book.md | **Journey stage:** Start progressive stream / Control long-form listening / Leave and preserve state | **Persona:** Harvey; Chloe | **State shown:** Playback Session: Loading, Playing, Buffering, Paused, Error

**Purpose:** Let users control audiobook playback with TV-scale controls, visible progress, speed, skip, and source-aware recovery.
**Entry points:** Library Home continue card, Book Detail resume/start.
**Exit points:** Book Detail, Library Home, Connection Recovery, Android TV background playback.
**Notes:** Progress persistence is a core system behavior surfaced through save/status indicators.

## Settings and Sources — PG08

**Type:** Settings | **Priority:** Should-have | **Scope:** In
**Flow:** harvey-connects-home-library.md; chloe-connects-cloud-library.md | **Journey stage:** Source management and diagnostics | **Persona:** Harvey; Chloe | **State shown:** Source: Connected, Degraded, Reauthorization Required

**Purpose:** Manage connected sources, sync status, source recovery, and playback preferences.
**Entry points:** Library Home settings action, Source Setup cancel, Connection Recovery details.
**Exit points:** Source Setup, Sync Status, Connection Recovery, Library Home.
**Notes:** Destructive actions require confirmation.

## Connection Recovery — PG09

**Type:** Error | **Priority:** Must-have | **Scope:** In
**Flow:** all flows | **Journey stage:** Any source or playback failure | **Persona:** Harvey; Chloe | **State shown:** Source: Unreachable/Reauthorization Required; Playback Session: Error

**Purpose:** Explain blocking source, authorization, sync, or playback failures and give specific recovery actions.
**Entry points:** Provider Auth failure, Folder Picker failure, Sync Status failure, Book Detail unavailable, Now Playing error.
**Exit points:** Retry previous action, Provider Auth, Settings and Sources, Library Home.
**Notes:** Must never collapse distinct failures into a generic error.
