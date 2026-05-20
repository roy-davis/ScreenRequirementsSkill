> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Sitemap

> File: outputs/screens/sitemap.md
> Canonical screens only. State variants are not separate nodes.

## Entry points

- Source Setup — first launch when no source is configured.
- Library Home — app launch when at least one source and cached library exist.
- Settings and Sources — reached from Library Home settings action.
- Now Playing — restored from active MediaSession/background playback.

## Screen connections

- source-setup → provider-auth (user selects SMB or cloud provider)
- source-setup → settings-and-sources (user cancels setup from settings context)
- provider-auth → folder-picker (connection or authorization succeeds)
- provider-auth → connection-recovery (connection/auth fails)
- folder-picker → sync-status (user confirms library root)
- folder-picker → connection-recovery (folder listing or permission fails)
- sync-status → library-home (partial or complete cached results available)
- sync-status → connection-recovery (blocking sync failure)
- library-home → book-detail (user opens book card)
- library-home → now-playing (user selects Continue listening)
- library-home → source-setup (user adds source from empty or source rail)
- library-home → settings-and-sources (user opens settings)
- book-detail → now-playing (user selects Resume, Start, or chapter)
- book-detail → connection-recovery (book or chapter asset unavailable)
- now-playing → book-detail (user opens chapter/book context)
- now-playing → connection-recovery (playback stream cannot resolve or continue)
- settings-and-sources → source-setup (user adds source)
- settings-and-sources → sync-status (user starts resync)
- settings-and-sources → connection-recovery (source needs recovery)
- connection-recovery → provider-auth (reauthorize or edit credentials)
- connection-recovery → sync-status (retry sync)
- connection-recovery → library-home (return to cached library)

## Orphaned screens

- None.
