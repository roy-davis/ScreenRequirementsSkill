> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Journey: Connect a Cloud Audiobook Library

> File: outputs/journeys/chloe-connects-cloud-library.md
> Persona: Chloe the Cloud Collector
> Trigger: Chloe chooses a cloud provider as her audiobook source
> End state: Cloud source is authorized, folder is selected, and sync is running or complete

## Stages

### Stage 1: Choose trusted provider
Chloe starts setup and looks for the cloud provider she already uses. She needs branded choices for Dropbox, Google Drive, iCloud Drive, and OneDrive, plus plain language about what the app will access. WebDAV should be visible only as an advanced custom option, not as the path for mainstream cloud providers. The risk is making the app feel unofficial or unsafe.

**Data needed:** Supported providers, provider capability summaries, authorization method, privacy/security copy.
**Friction risk:** Generic protocol language can reduce trust before authorization begins.

### Stage 2: Authorize account
Chloe authorizes the provider, likely using a secondary device or provider sign-in flow rather than typing credentials on the TV. She needs to understand what permission is being requested and what happens after approval. The app should show a waiting state that can survive switching devices. The risk is OAuth timeout or unclear account mismatch.

**Data needed:** Provider account state, device code or authorization URL, token status, requested scopes, expiration state.
**Friction risk:** If the TV screen times out without a recovery action, Chloe may abandon setup.

### Stage 3: Pick cloud folder
After authorization, Chloe browses provider folders and selects the audiobook root. She needs friendly folder names, breadcrumbs, and confidence that the app is not uploading or modifying files. Folder listing should use provider APIs and expose source capabilities relevant to playback. The risk is slow folder listing or unsupported file access.

**Data needed:** Provider folder tree, account label, folder IDs, direct/ranged download support, selected folder ID.
**Friction risk:** A missing or empty folder can be interpreted as account failure without clear empty-state copy.

### Stage 4: Sync cloud metadata
Chloe starts indexing and waits for enough results to confirm that the selected folder is correct. The app crawls in the background, storing book and chapter metadata locally. It should allow navigation away once sync is running and make partial results understandable. The risk is a provider rate limit or token refresh issue mid-sync.

**Data needed:** Sync progress, provider rate-limit status, books found, files skipped, token refresh state, last sync time.
**Friction risk:** Provider errors need to be phrased as recoverable account/source states.

### Stage 5: Browse from cache
Chloe arrives in the library and sees her cloud-backed audiobooks displayed as books, not file lists. The app should clearly indicate that browsing works from cache and that playback still depends on cloud availability. She needs a simple path to reauthorize if the provider later expires. The risk is cloud-specific status overwhelming the normal listening path.

**Data needed:** Cached books, provider account label, source status, last sync, resumable playback metadata.
**Friction risk:** If cloud availability is unclear, Chloe may not trust the library to be current.

## Moments that matter
- **Provider selection:** Branded setup communicates legitimacy and avoids the impression of a generic workaround.
- **Authorization waiting:** Chloe needs clear next steps and recovery before switching to another device.
- **Cloud folder to sync:** The app proves it can turn cloud files into a usable audiobook library.

## Screens implied

| Stage | Screen needed |
|---|---|
| Choose trusted provider | Source Setup |
| Authorize account | Provider Auth |
| Pick cloud folder | Folder Picker |
| Sync cloud metadata | Sync Status |
| Browse from cache | Library Home |
