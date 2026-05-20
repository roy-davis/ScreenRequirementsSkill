> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Product Brief

## Product name
Local & Cloud Network Audiobook Player for Android TV

## Problem statement
Home server hobbyists and cloud storage users want to listen to long-form audiobooks through a TV or home theater system, but existing Android TV media players are built around video libraries or short music tracks. They often browse remote folders directly, causing slow D-pad navigation, weak focus behavior, limited cloud provider support, and unreliable long-book progress persistence.

## Product purpose
The product is a lean native Android TV audiobook player that indexes remote audiobook libraries into a local metadata cache, then streams playable assets on demand from SMB and first-class cloud providers. It gives TV users fast cached browsing, exact progress resume, background playback, and audiobook-specific controls without requiring local device downloads.

## Target users
- Home server hobbyists with SMB or Windows/Samba network shares.
- Cloud storage users with audiobook libraries in Dropbox, Google Drive, iCloud Drive, or OneDrive.
- Shared household listeners who use a TV remote and expect a 10-foot playback experience.

## Business goal
Deliver a focused Android TV audiobook player prototype that validates whether cached remote-library browsing plus provider-backed streaming creates a meaningfully better audiobook experience than generic TV media players.

## Success criteria
- Cached library screens render perceptibly instantly after the first sync, targeting under 100 ms from local metadata.
- Selecting a playable chapter starts audio within 2 seconds on a typical 5 GHz home Wi-Fi network.
- Playback position is restored to the exact book and millisecond after leaving and returning to the app.
- All primary tasks can be completed with a D-pad remote and visible focus states.
- Users can connect at least one SMB source and at least one first-class cloud provider source in the prototype flow.
- Error and recovery states are specific to source type, authorization, connectivity, and playable asset resolution.

## Scope

### In scope
- Android TV-first library browsing and playback prototype.
- SMB2/SMB3 source setup with credentials or anonymous login.
- First-class branded setup concepts for Dropbox, Google Drive, iCloud Drive, and OneDrive.
- Optional WebDAV as an advanced source only.
- Local Room-style metadata cache concept for books, chapters, provider refs, and progress.
- Background sync, sync status, provider capability display, and source recovery flows.
- Media3/ExoPlayer-style playback behavior, background media session behavior, progress persistence, speed control, and skip controls.
- TV-safe visual layout with overscan protection, D-pad focus, and large readable controls.

### Out of scope
- Local on-device file downloading.
- Third-party metadata scraping from book APIs.
- Mobile phone companion app.
- Multi-user account management beyond household/source ownership assumptions.
- Full production OAuth implementation details.
- Full provider API compliance review.

## Known constraints
- Native Android TV target, minimum SDK 26.
- 10-foot interaction model using hardware D-pad remote input.
- UI should use cached local data for browsing and avoid network calls during navigation.
- Playback depends on remote source capabilities for ranged downloads, direct streams, token refresh, and resumable access.
- Credentials, tokens, refresh tokens, app passwords, and source metadata must be stored in encrypted Android storage.
- Legacy televisions may crop edges, so core UI must remain inside a 48 dp to 96 dp safe margin.

## Key assumptions
- [assumed — high confidence] Phase 1 should optimize for a single household library rather than social, marketplace, or publishing workflows.
- [assumed — high confidence] A native Android TV implementation using Jetpack Compose for TV and Media3 is the intended build direction.
- [assumed — high confidence] The prototype should show source setup, sync, browsing, book detail, and now-playing as the primary experience.
- [assumed — high confidence] First-class cloud connectors need branded user-facing flows even if the prototype stubs authorization.
- [assumed — low confidence] The first prototype target tool is "other/native Android TV prototype" because no generation tool was specified.
- [assumed — low confidence] Household profiles, parental controls, and DRM-protected audiobook stores are not needed for the first prototype.

## Open questions
- Which prototype generation target should be optimized for: v0, Lovable, Bolt, Claude Design, Google Stitch, or native Android implementation notes?
- Which first-class cloud provider should be fully represented first in the prototype?
- Should iCloud Drive support be limited to app-password/WebDAV-like access where platform constraints require it, while still presenting it as a first-class source?
- Are multiple listeners/profiles required for progress separation?
- Should the app support audiobook formats beyond common audio files, such as M4B chapter metadata and embedded cover art?
- What is the expected library scale for first-release performance targets: hundreds of books, thousands of tracks, or larger?
