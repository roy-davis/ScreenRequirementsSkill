> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Flow: Connect a Home SMB Library

> File: outputs/flows/harvey-connects-home-library.md
> Persona: Harvey the Home Server Hobbyist
> Journey: harvey-connects-home-library.md
> Implements: Choose source type, Enter network details, Pick library folder, Index and verify, Land in library

- Trigger: Harvey launches the app with no sources or selects Add source from settings.
- Entry point: Source Setup.
- Preconditions: Android TV has network connectivity; user has access to SMB host and share credentials; encrypted storage is available.
- Steps:
  1. Harvey focuses SMB on Source Setup and opens it ← Choose source type.
  2. The app shows SMB capability summary, setup requirements, and secure storage copy.
  3. Harvey enters host, share path, optional domain/workgroup, username, and password, or enables anonymous login ← Enter network details.
  4. The app validates required fields and attempts an SMB2/SMB3 connection.
  5. If connection succeeds, the app stores credentials in encrypted storage and opens Folder Picker.
  6. Harvey navigates breadcrumbs and folder rows with the D-pad ← Pick library folder.
  7. Harvey selects the audiobook root folder and confirms Use this folder.
  8. The app saves the source, starts WorkManager background indexing, and opens Sync Status ← Index and verify.
  9. The app displays scanned files, books found, skipped files, source health, and estimated completion.
  10. Harvey selects Open library once partial or complete results are available ← Land in library.
- Decision points: Choose SMB vs another source; anonymous login vs credentials; select root folder; open library now vs stay on sync status.
- Branches:
  - Host unreachable: show Connection Recovery with host/network guidance and retry.
  - Auth fails: keep Provider Auth open with credentials highlighted and password re-entry focused.
  - Share connects but folder listing fails: show folder-specific permission error with Back to credentials and Retry.
  - No audio files found: show Sync Status empty result with Change folder action.
  - Sync partially succeeds: allow Library Home with warning indicator and diagnostics link.
- Error paths: Host unreachable, SMB protocol unsupported, credentials rejected, share path not found, folder permission denied, no supported audio files, encrypted storage unavailable.
- Exit states: Source saved and indexing; source saved with partial sync; setup abandoned; setup blocked by connection failure.
- Completion criteria: The SMB source is saved securely, a root folder is selected, indexing has begun or completed, and Harvey can browse cached books from Library Home.
