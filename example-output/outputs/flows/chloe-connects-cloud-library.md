> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Flow: Connect a Cloud Audiobook Library

> File: outputs/flows/chloe-connects-cloud-library.md
> Persona: Chloe the Cloud Collector
> Journey: chloe-connects-cloud-library.md
> Implements: Choose trusted provider, Authorize account, Pick cloud folder, Sync cloud metadata, Browse from cache

- Trigger: Chloe chooses Add source and selects a cloud provider.
- Entry point: Source Setup.
- Preconditions: Android TV has internet access; provider OAuth or account authorization endpoint is available; encrypted token storage is available.
- Steps:
  1. Chloe focuses Dropbox, Google Drive, iCloud Drive, or OneDrive on Source Setup ← Choose trusted provider.
  2. The app shows provider-specific access explanation, capabilities, and Continue.
  3. Chloe starts authorization; the app displays device-code or secondary-device sign-in instructions ← Authorize account.
  4. The app waits for provider confirmation and shows account authorization state.
  5. After authorization, the app stores tokens securely and opens Folder Picker.
  6. Chloe browses provider folders and selects the audiobook root ← Pick cloud folder.
  7. The app records provider folder ID/path and provider capabilities for listing, ranged download, refresh, and resumable streaming.
  8. The app starts background metadata sync and opens Sync Status ← Sync cloud metadata.
  9. Chloe reviews books found, provider account label, skipped files, and sync issues.
  10. Chloe opens Library Home once partial or complete cached results are available ← Browse from cache.
- Decision points: Choose provider; continue or cancel authorization; select account; select folder; open library or wait for sync.
- Branches:
  - Authorization pending: keep waiting state with code, expiry time, and restart action.
  - Authorization denied: return to provider setup with explanation and Try again.
  - Token refresh unsupported or fails: show Connection Recovery with reauthorize action.
  - Provider folder listing fails: show provider-specific error and retry.
  - Provider lacks ranged download: mark capability limitation and warn before saving source.
  - Sync hits rate limit: pause sync and show next retry estimate.
- Error paths: OAuth timeout, account mismatch, permission denied, provider API unavailable, folder empty, file type unsupported, token storage unavailable.
- Exit states: Cloud source authorized and indexing; authorization pending; setup cancelled; recovery required.
- Completion criteria: Chloe has authorized a branded cloud provider, selected an audiobook folder, and can browse locally cached library results after sync begins.
