> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Journey: Connect a Home SMB Library

> File: outputs/journeys/harvey-connects-home-library.md
> Persona: Harvey the Home Server Hobbyist
> Trigger: Harvey launches the app for the first time or adds a new source
> End state: SMB source is saved, indexed, and available in the library

## Stages

### Stage 1: Choose source type
Harvey opens the app and sees that no audiobook sources are connected yet. He wants to know whether the app understands a local network share without forcing him into a generic file browser. The screen needs to separate SMB from branded cloud providers and advanced WebDAV so he can choose the right path quickly. The main risk is making setup feel like a mobile form transplanted onto a TV.

**Data needed:** Available source types, capability summaries, setup requirements, current connection status.
**Friction risk:** If source options are visually similar or terminology is vague, Harvey may pick WebDAV or cloud by mistake.

### Stage 2: Enter network details
Harvey enters the server address, share path, and credentials with a remote. He needs confidence that credentials are stored securely and that anonymous login is supported when appropriate. The app should validate only enough to catch obvious mistakes before trying the connection. The biggest risk is long text entry and unclear authentication failure messaging.

**Data needed:** SMB host, share path, username, password, anonymous login toggle, secure storage notice.
**Friction risk:** Remote text entry is slow, especially for passwords and IP addresses.

### Stage 3: Pick library folder
After the share connects, Harvey browses the remote folder tree to choose the audiobook root. He needs a focused folder picker that shows where he is, which folders are selectable, and what will happen after selection. The app should not require a full library crawl just to navigate. If a folder is empty or unreachable, the reason must be specific.

**Data needed:** Remote folder names, path breadcrumbs, folder accessibility, selected root path.
**Friction risk:** Slow network listing or missing permission can make the app feel broken.

### Stage 4: Index and verify
Harvey starts the background sync and watches enough progress to know that the app is building a local cache. He wants evidence that the app found books and chapters, not just raw audio files. The screen should explain that browsing will come from local metadata after indexing. The risk is leaving him unsure whether playback can begin while sync is incomplete.

**Data needed:** Sync status, files scanned, books found, chapters found, errors, last updated time.
**Friction risk:** A long initial crawl can reduce trust if there is no visible progress or partial result.

### Stage 5: Land in library
When sync completes or has partial results, Harvey lands in the library grid. He needs the app to feel instant and TV-native, with cover/title/progress cards driven by the local database. He should see whether the source is healthy without source diagnostics dominating the browse experience. The risk is exposing implementation details instead of letting him start a book.

**Data needed:** Cached books, source health, sync timestamp, cover/title/author/progress metadata.
**Friction risk:** If the library is empty after sync, the app must distinguish no audiobooks found from sync failed.

## Moments that matter
- **Source type to setup:** Trust is built when SMB is treated as a first-class source, not a hidden advanced option.
- **Credential submission:** A failed connection must say whether the host, share, credentials, or network appears to be the issue.
- **Sync completion to library:** The app proves its core promise when the library opens instantly from cache.

## Screens implied

| Stage | Screen needed |
|---|---|
| Choose source type | Source Setup |
| Enter network details | Provider Auth |
| Pick library folder | Folder Picker |
| Index and verify | Sync Status |
| Land in library | Library Home |
