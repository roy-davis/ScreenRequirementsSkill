> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Source Setup

> File: outputs/screens/source-setup.md
> Screen ID: PG01

| Attribute | Value |
|---|---|
| Route | `/setup/source` |
| Access | Source owner, listener |
| Mobile | Not required |
| Scope | In scope |

### Design intent

- Primary purpose: Help the user choose where their audiobook library lives.
- Primary actors: Harvey the Home Server Hobbyist; Chloe the Cloud Collector.
- Primary job: Select SMB, a first-class cloud provider, or advanced WebDAV and understand what setup will require.
- Principle served: TV-first clarity; source choices must be recognizable at a glance from across the room.
- Entry points: First launch with no sources; Add source from Settings and Sources; empty Library Home.
- Success outcome: User chooses a source type and moves to the appropriate setup path.
- Content priority: Primary: source tiles for SMB, Dropbox, Google Drive, iCloud Drive, and OneDrive. Secondary: advanced WebDAV and source capability explanations.
- Critical variants: No sources connected; add another source; internet unavailable; cloud providers temporarily unavailable.
- Exit paths: Provider Auth, Settings and Sources, Library Home if sources already exist.

### Related flows

- harvey-connects-home-library.md: Starts SMB setup for a local network share.
- chloe-connects-cloud-library.md: Starts branded cloud provider setup.

### Decisions on this screen

| ID | Decision | Stakes | Reversible |
|---|---|---|---|
| D1 | Select source type | Med | Yes |

**Select source type — D1**
- What the user is deciding: Which storage provider the app should connect to.
- Options: SMB for local network shares; Dropbox, Google Drive, iCloud Drive, or OneDrive for cloud libraries; WebDAV for advanced custom sources.
- Information required: Provider name, source type, setup requirement, whether folder browsing and resumable streaming are supported.
- Risk if wrong: User enters the wrong setup flow and loses confidence before connecting a library.
- Reversible: Yes — Recovery: Back returns to this screen before credentials or tokens are saved.
- Recommended default: SMB when first launching from a local-network focused context; otherwise no preselected provider.
- Conditions that change this: If internet is offline, cloud providers remain visible but disabled with an explanation.

### States

| State | Description | Transitions |
|---|---|---|
| `empty_first_run` | No sources exist; setup is the primary path | Select source → Provider Auth |
| `add_source` | Existing library exists and user is adding another source | Select source → Provider Auth; Back → Settings |
| `offline` | Internet unavailable; cloud sources cannot authorize | SMB enabled; cloud disabled; Retry network |
| `provider_issue` | Provider status cannot be checked | Cloud tiles remain selectable with warning |

### Actions

| Action | Description | Precondition |
|---|---|---|
| Choose SMB | Opens SMB credential/setup path | Always |
| Choose cloud provider | Opens provider authorization path | Internet available |
| Choose WebDAV | Opens advanced custom source path | Advanced option visible |
| Back | Leaves setup when launched from settings | Existing source exists |

### Data required

| Data | Source | Offline | Notes |
|---|---|---|---|
| Supported source types | Local | Local | Static capability model |
| Provider availability | Server | Real-time | Optional status; do not block tile rendering |
| Existing source count | Local database | Cached | Determines first-run vs add-source framing |
| Capability descriptions | Local | Local | Used to explain listing, streaming, refresh support |

### Visual indicators

| Indicator | Meaning |
|---|---|
| Primary outline around focused tile | Current D-pad focus |
| Source health badge | Whether source type is available now |
| Advanced label | WebDAV is a custom source, not a primary provider |
| Disabled cloud tile with network copy | Cloud setup requires internet |

### Critical variants

**First run**
The screen opens with source choice as the only main task. It should make "Add your first audiobook source" clear without marketing copy.

**Adding another source**
The screen includes a compact path back to Settings and Sources, with existing sources summarized in the secondary region.

**Offline**
SMB remains available because it may work on the local network. Cloud providers are shown disabled with "Internet required to authorize" and a Retry network action.

### Layout regions

| Region | Purpose | Required content |
|---|---|---|
| Header | Orient user and show setup status | Page title, short setup line, Back affordance when relevant |
| Primary source grid | Let user choose a source | SMB and four branded cloud provider tiles |
| Advanced source row | Keep WebDAV available without competing with primary providers | WebDAV tile, custom-source explanation |
| Capability detail panel | Explain focused source | Listing, streaming, authorization, and sync capability summary |
| Footer actions | Navigation and recovery | Continue is implicit through tile activation; network retry if needed |

### Content

| Element | Copy |
|---|---|
| Page title | Add audiobook source |
| Primary action label | Select source |
| Empty state heading | No sources connected |
| Empty state body | Add a network share or cloud folder to build your audiobook library cache. |
| Empty state CTA | Add source |
| Error heading | Source options unavailable |
| Error body | Cloud setup needs an internet connection. Local SMB may still work on your network. |
| Offline message | Internet is offline. Cloud providers are disabled until the connection returns. |

### Screen generation prompt

> Target tool: other / native Android TV prototype

Create a TV-first source selection screen for Harvey and Chloe, who need to choose where their audiobook library lives using only a D-pad remote. The screen must show SMB as a local network source, Dropbox, Google Drive, iCloud Drive, and OneDrive as branded first-class cloud sources, and WebDAV as a clearly advanced custom option. The focused source must reveal setup requirements and capabilities such as folder listing, ranged streaming, silent token refresh, and resumable streaming without crowding the primary choice grid. Handle first-run, add-source, offline, and provider-warning states with large safe-margin layouts, visible focus scale/outline, disabled cloud actions when internet is unavailable, and no generic file-browser language.

### Acceptance criteria

- [ ] Harvey can choose SMB or Chloe can choose a cloud provider using only D-pad focus on a standard TV viewport.
- [ ] SMB, Dropbox, Google Drive, iCloud Drive, and OneDrive are all immediately visible without scrolling.
- [ ] WebDAV is visible as an advanced custom option and not implied as the implementation path for cloud providers.
- [ ] Offline state disables cloud setup but leaves local SMB visibly available.
- [ ] Focused source tile has a distinct visual state beyond color alone.
- [ ] Capability details are specific to the focused source.
- [ ] All content uses realistic provider and source labels.

### Heuristic check

All core heuristic items are defined for this screen. Heuristic gaps: None.
