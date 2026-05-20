> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Chloe the Cloud Collector — Cloud Library Listener

> File: outputs/personas/chloe-the-cloud-collector.md

**Composite summary:** Chloe keeps audiobooks in cloud drives because they are easy to move between devices and do not require running a server at home. She is less interested in storage protocols than in knowing that the app can connect to the provider she already uses, find the right folder, and keep playing without repeatedly asking her to sign in. She expects cloud setup to feel branded, trustworthy, and recoverable.

## Identity

| Attribute | Value |
|---|---|
| Actor group | Primary listener and cloud account owner |
| Access level | Source setup for owned cloud accounts, playback, sync, and basic settings |
| Scope | Android TV app connected to one or more personal cloud drives |
| Archetype name | Chloe the Cloud Collector |
| Example role titles | Cloud storage user, audiobook collector, household listener |

## Context of Use

| Attribute | Value |
|---|---|
| Primary device | Android TV device with remote |
| Secondary device | Phone or laptop for OAuth confirmation, account security, and file organization |
| Typical connectivity | Home Wi-Fi with internet dependency |
| Physical environment | Living room TV or shared family room |
| Environmental constraints | Remote-first input, account security concerns, token expiration, cloud throttling |

## Technical Profile

| Attribute | Value |
|---|---|
| Technical literacy | Medium |
| Software familiarity | Comfortable with Dropbox, Google Drive, iCloud Drive, or OneDrive |
| Data entry comfort | Low on TV remote; expects device-code or browser-based auth |
| Map / spatial comfort | Medium; recognizes folders but prefers friendly labels and recents |
| Tolerance for complexity | Low unless provider language explains the issue clearly |

## Time and Attention

| Attribute | Value |
|---|---|
| Typical work hours | Evenings, weekends, occasional background listening |
| Peak usage times | When relaxing, cooking, cleaning, or winding down |
| Session duration | 15 minutes to 90 minutes |
| Interruption frequency | High; TV may switch apps and playback should continue |
| Time pressure level | Medium when starting playback |
| Attention availability | Low during playback; medium during initial setup |

## Decision Authority

| Attribute | Value |
|---|---|
| Strategic decisions | Chooses which cloud provider and folder to connect |
| Operational decisions | Reauthorizes account, picks library folders, starts sync |
| Financial authority | May pay for cloud storage or app subscription |
| Delegation patterns | May ask a technical household member for source setup |
| Approval dependencies | Provider authorization and account security requirements |

## Workday Shape

| Period | Description |
|---|---|
| Morning | Brief listening sessions while getting ready |
| Midday | Low usage unless at home |
| Afternoon | May upload or organize files from another device |
| Evening | Primary TV listening and browse time |
| Seasonal variation | More usage during travel planning or downtime, but TV app remains home-based |

## Goals

| Goal type | Description |
|---|---|
| Primary job goal | Connect a cloud drive folder and listen to long audiobooks without managing local copies |
| Secondary goals | Know whether sync is current, recover expired authorization, switch between providers |
| Success criteria | Provider setup feels legitimate; library updates appear after sync; errors explain next steps |
| Quality signals | Provider logos/names, clear account status, folder picker, automatic token refresh messaging |

## Frustrations

| Frustration type | Description |
|---|---|
| Current pain points | TV apps treat cloud drives as generic files, lose authorization, or require awkward browser workarounds |
| Legacy system complaints | Generic WebDAV-style setup feels unsafe and confusing for branded providers |
| Workflow friction | Choosing folders on a remote and interpreting provider permissions |
| Information gaps | Unclear whether a missing book is not synced, not supported, or inaccessible |

## Constraints for Flow Design

| Constraint | Implication |
|---|---|
| Device constraint | Prefer device-code auth and short remote input forms |
| Connectivity constraint | Distinguish internet outage from provider token or file permission failure |
| Time constraint | Provide visible sync status and allow playback from cached metadata |
| Environment constraint | Avoid small provider/account text; show status in glanceable blocks |
| Authority constraint | Limit destructive source actions behind confirmation |
| Literacy constraint | Use provider language instead of protocol jargon |

## Jobs This Persona Performs

| Job ID | Job statement | Frequency | Notes |
|---|---|---|---|
| J-005 | Connect a first-class cloud provider | Initial setup and account changes | Needs branded auth flow |
| J-006 | Pick a cloud folder as the audiobook library root | Initial setup and occasional changes | Needs folder browsing from provider API |
| J-007 | Resume a cloud-backed audiobook | Frequent | Needs token refresh and playable URL resolution |
| J-008 | Recover an expired provider session | As needed | Needs reauthorize action and clear explanation |

## Scenarios Involving This Persona

| Scenario ID | Scenario name | Role in scenario |
|---|---|---|
| SC-04 | Connect a cloud audiobook library | Account owner and setup actor |
| SC-05 | Resume playback from cloud storage | Primary listener |
| SC-06 | Reauthorize provider access | Recovery actor |
