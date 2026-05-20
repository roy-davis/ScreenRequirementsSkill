> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Provider Auth

> File: outputs/screens/provider-auth.md
> Screen ID: PG02

| Attribute | Value |
|---|---|
| Route | `/setup/provider-auth` |
| Access | Source owner |
| Mobile | Not required |
| Scope | In scope |

### Design intent

- Primary purpose: Collect source credentials or guide provider authorization safely on a TV.
- Primary actors: Harvey the Home Server Hobbyist; Chloe the Cloud Collector.
- Primary job: Connect to SMB or authorize a cloud provider with minimal remote typing and clear recovery.
- Principle served: Trust through specificity; connection and authorization states must say exactly what is happening.
- Entry points: Source Setup after selecting a source; Connection Recovery after retry/edit credentials.
- Success outcome: Source credentials or provider tokens are securely stored and the user proceeds to Folder Picker.
- Content priority: Primary: setup form or authorization instructions. Secondary: security copy, capability summary, troubleshooting.
- Critical variants: SMB credential form; anonymous SMB; cloud device-code authorization; authorization pending; authorization denied; token storage failure.
- Exit paths: Folder Picker, Connection Recovery, Source Setup.

### Related flows

- harvey-connects-home-library.md: Captures SMB host, share, and credentials.
- chloe-connects-cloud-library.md: Guides provider authorization without TV credential entry.

### Decisions on this screen

| ID | Decision | Stakes | Reversible |
|---|---|---|---|
| D1 | Submit source credentials or provider authorization | High | Yes |

**Submit source credentials or provider authorization — D1**
- What the user is deciding: Whether to trust the app with credentials/tokens and attempt connection.
- Options: Continue with entered SMB details; use anonymous SMB; start provider authorization; cancel setup.
- Information required: Source name, required fields, secure storage notice, permission scope, provider account label when available.
- Risk if wrong: Credentials could be incorrect, the wrong provider account could be authorized, or setup could proceed with insufficient access.
- Reversible: Yes — Recovery: Credentials can be edited before save; provider access can be revoked or reauthorized.
- Recommended default: For cloud providers, use a secondary-device authorization path instead of TV password entry.
- Conditions that change this: If encrypted storage is unavailable, all submission actions are blocked.

### States

| State | Description | Transitions |
|---|---|---|
| `smb_form` | SMB host/share/credential entry is active | Test connection → loading/success/error |
| `anonymous_smb` | Anonymous login selected and credential fields hidden | Test connection → loading/success/error |
| `cloud_start` | Provider permission explanation before auth begins | Continue → pending authorization |
| `cloud_pending` | Waiting for provider authorization from another device | Approved → Folder Picker; Denied/expired → error |
| `auth_error` | Credentials or provider authorization failed | Retry/edit → same screen; Details → Connection Recovery |

### Actions

| Action | Description | Precondition |
|---|---|---|
| Test connection | Attempts SMB connection | Required SMB fields valid or anonymous enabled |
| Start authorization | Begins provider auth flow | Internet available and provider selected |
| Restart authorization | Generates a new auth attempt | Previous attempt expired or denied |
| Continue to folder picker | Opens Folder Picker | Connection or authorization succeeds |
| Cancel | Returns to Source Setup | Always |

### Data required

| Data | Source | Offline | Notes |
|---|---|---|---|
| Selected provider/source type | Local navigation state | Local | Determines form variant |
| SMB host/share/credentials | Input | N/A | Stored only after successful connection |
| Provider auth code/session | Provider API | Real-time | May expire |
| Token/credential storage state | Device | Local | Blocks setup if unavailable |
| Capability model | Local/provider API | Cached/Real-time | Cloud capabilities may arrive after auth |

### Visual indicators

| Indicator | Meaning |
|---|---|
| Secure storage lock copy | Credentials/tokens are encrypted on device |
| Pending authorization countdown | Device-code auth is waiting or expiring |
| Field highlight | Current text-entry focus or validation error |
| Provider account badge | Cloud account authorized successfully |

### Critical variants

**SMB credentials**
Show host, share path, optional workgroup/domain, username, password, and anonymous login. Keep each field large and focusable, with remote-friendly edit states.

**Cloud authorization**
Show provider name, requested access purpose, device-code or secondary-device instruction, pending status, and account confirmation when approved.

**Authorization or connection failure**
Explain whether the host, credentials, provider authorization, account permission, or secure storage failed. Give Retry, Edit, and Details actions.

### Layout regions

| Region | Purpose | Required content |
|---|---|---|
| Header | Show selected source and current setup step | Source name, step label, Back |
| Primary auth panel | Collect or guide authorization | SMB fields or cloud authorization instructions |
| Security panel | Build trust | Encrypted storage copy, provider permission summary |
| Status panel | Show connection/auth state | Loading, pending, approved, denied, expired |
| Action row | Advance or recover | Test connection/Start authorization, Retry, Cancel |

### Content

| Element | Copy |
|---|---|
| Page title | Connect source |
| Primary action label | Test connection |
| Empty state heading | Source not selected |
| Empty state body | Choose a source before entering connection details. |
| Empty state CTA | Back to sources |
| Error heading | Connection failed |
| Error body | The app could not connect with these details. Check the host, share, and credentials. |
| Offline message | Internet is offline. Cloud authorization cannot start yet. |

### Screen generation prompt

> Target tool: other / native Android TV prototype

Create a TV-scale provider authorization screen that adapts between SMB setup for Harvey and cloud authorization for Chloe. For SMB, show large remote-friendly fields for host, share path, username, password, optional domain, and anonymous login, with a clear secure-storage message and source-specific validation. For cloud providers, show provider-branded authorization instructions, pending status, code expiry, requested access purpose, and account confirmation without asking the user to type a provider password on the TV. The screen must handle loading, pending, denied, expired, offline, and secure-storage-failed states with explicit recovery actions, large focus indicators, and a safe path back to Source Setup.

### Acceptance criteria

- [ ] Harvey can test an SMB connection with clear field focus and validation.
- [ ] Chloe can start and monitor a cloud authorization flow without TV password entry.
- [ ] Secure credential/token storage is visibly explained before submission.
- [ ] Authorization pending, denied, expired, and approved states are visually distinct.
- [ ] Connection failures name the likely cause and show Retry/Edit actions.
- [ ] Primary action is disabled until required information is present.
- [ ] No credential or token value is displayed after submission.

### Heuristic check

All core heuristic items are defined for this screen. Heuristic gaps: None.
