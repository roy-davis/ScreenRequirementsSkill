> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Folder Picker

> File: outputs/screens/folder-picker.md
> Screen ID: PG03

| Attribute | Value |
|---|---|
| Route | `/setup/folder-picker` |
| Access | Source owner |
| Mobile | Not required |
| Scope | In scope |

### Design intent

- Primary purpose: Let the user choose the folder that represents the audiobook library root.
- Primary actors: Harvey the Home Server Hobbyist; Chloe the Cloud Collector.
- Primary job: Browse source folders safely, select the correct root, and start indexing.
- Principle served: Cache before browse; folder selection should not feel like the long-term library UI.
- Entry points: Provider Auth success; Connection Recovery after folder retry.
- Success outcome: A folder is selected, saved as source root, and sync begins.
- Content priority: Primary: current path, folder list, Use this folder. Secondary: accessibility/capability warnings and folder details.
- Critical variants: Root folder; nested folder; empty folder; folder permission denied; listing loading; provider rate limit.
- Exit paths: Sync Status, Provider Auth, Connection Recovery.

### Related flows

- harvey-connects-home-library.md: Picks SMB share folder.
- chloe-connects-cloud-library.md: Picks cloud provider folder.

### Decisions on this screen

| ID | Decision | Stakes | Reversible |
|---|---|---|---|
| D1 | Choose library root folder | High | Yes |

**Choose library root folder — D1**
- What the user is deciding: Which folder the app will crawl and interpret as the audiobook library.
- Options: Use current folder, open child folder, go up one level, cancel.
- Information required: Current source, path/breadcrumb, folder contents, whether files/folders are accessible.
- Risk if wrong: The app may index the wrong folder, find no books, or crawl a very large unrelated tree.
- Reversible: Yes — Recovery: Source root can be changed later, but sync results may need clearing/resyncing.
- Recommended default: Do not preselect Use this folder until at least one folder or supported audio file is visible, unless source root was explicitly selected.
- Conditions that change this: Permission errors, empty folders, or provider rate limits block the primary action.

### States

| State | Description | Transitions |
|---|---|---|
| `loading` | Listing folders for the current path | Success → ready; failure → error |
| `ready` | Folder rows and breadcrumbs are visible | Open folder; Use this folder → Sync Status |
| `empty` | Current folder has no subfolders or supported audio hints | Back or choose parent |
| `permission_denied` | Source cannot list current folder | Retry, back, or Connection Recovery |
| `rate_limited` | Cloud provider delayed listing | Wait, retry later, or back |

### Actions

| Action | Description | Precondition |
|---|---|---|
| Open folder | Navigates into focused folder | Folder is accessible |
| Go up | Navigates to parent folder | Parent exists |
| Use this folder | Saves selected root and starts sync | Current folder accessible |
| Retry listing | Reattempts folder listing | Listing failed |
| Back to auth | Returns to edit credentials/auth | Always |

### Data required

| Data | Source | Offline | Notes |
|---|---|---|---|
| Current source/account | Local secure metadata | Cached | Shows context |
| Folder list | SMB/provider API | Real-time | Setup-time network access is expected |
| Breadcrumb path | Source API/local state | Cached during session | Must be readable from TV distance |
| Folder accessibility | SMB/provider API | Real-time | Drives disabled/error states |
| Capability warnings | Provider model | Cached | Ranged download/listing support |

### Visual indicators

| Indicator | Meaning |
|---|---|
| Focused folder row | Current D-pad focus |
| Folder access badge | Folder can be opened or has warning |
| Breadcrumb highlight | Current path segment |
| Primary action lock/disabled | Current folder cannot be selected |

### Critical variants

**SMB folder list**
Show server/share context and path segments. Permission and host errors should point back to Provider Auth or Connection Recovery.

**Cloud folder list**
Show provider account label and provider-specific folder names. Rate limits and authorization problems must use provider language.

**Empty folder**
Explain that no folders or supported audio hints were found here and offer parent navigation or Use anyway only if allowed.

### Layout regions

| Region | Purpose | Required content |
|---|---|---|
| Header | Show source and folder-picking step | Source name, account/host label, Back |
| Breadcrumb rail | Keep navigation context visible | Root/share/provider path |
| Folder list | Let user navigate folders | Folder name, accessibility, optional item count |
| Detail panel | Explain focused folder | Path, warnings, selection consequence |
| Action row | Confirm or recover | Use this folder, Retry, Back |

### Content

| Element | Copy |
|---|---|
| Page title | Choose audiobook folder |
| Primary action label | Use this folder |
| Empty state heading | This folder looks empty |
| Empty state body | Choose another folder or use this one if your audiobook files are deeper in the source. |
| Empty state CTA | Go up |
| Error heading | Folder cannot be opened |
| Error body | The source connected, but this folder is not available with the current permissions. |
| Offline message | Folder browsing needs the source to be reachable. Cached library browsing will work after setup. |

### Screen generation prompt

> Target tool: other / native Android TV prototype

Create a TV-first folder picker for selecting an audiobook library root after SMB connection or cloud authorization. The user must see the current source, account or host label, breadcrumbs, a large focusable folder list, and a clear "Use this folder" action that starts indexing. The screen should support loading, empty, permission-denied, rate-limited, and source-unreachable states with recovery actions that point back to credentials, provider authorization, or retry. The folder picker is setup-only and should not look like the long-term library browser; it must prioritize safe selection, readable path context, and source-specific warnings over dense file details.

### Acceptance criteria

- [ ] The user can navigate folder rows and breadcrumbs with D-pad focus.
- [ ] The current path and source/account context are visible at all times.
- [ ] "Use this folder" is disabled or explained when the folder cannot be selected.
- [ ] Empty, loading, permission, and rate-limit states have specific copy and recovery actions.
- [ ] Folder selection clearly leads to indexing, not immediate playback.
- [ ] The screen distinguishes setup-time network folder listing from cached library browsing.

### Heuristic check

All core heuristic items are defined for this screen. Heuristic gaps: None.
