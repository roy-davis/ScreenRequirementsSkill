> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Harvey the Home Server Hobbyist — Home Server Listener

> File: outputs/personas/harvey-the-home-server-hobbyist.md

**Composite summary:** Harvey keeps a small home server running for media, backups, and personal archives. He is comfortable configuring network shares, but when he sits down in front of the TV he expects the app to behave like an appliance: fast, focused, and reliable with a remote. He trusts systems that expose enough source and sync detail to diagnose problems without making ordinary playback feel technical.

## Identity

| Attribute | Value |
|---|---|
| Actor group | Primary listener and library owner |
| Access level | Full source setup, library sync, playback, and settings access |
| Scope | Household Android TV device connected to local SMB shares |
| Archetype name | Harvey the Home Server Hobbyist |
| Example role titles | Home server hobbyist, self-hosted media user, NAS owner |

## Context of Use

| Attribute | Value |
|---|---|
| Primary device | Android TV or Google TV device with D-pad remote |
| Secondary device | Laptop or phone used outside the app to manage files and server settings |
| Typical connectivity | 5 GHz Wi-Fi or wired Ethernet inside the home |
| Physical environment | Living room, home theater, or bedroom TV setup |
| Environmental constraints | Seated several feet from screen; no keyboard; interruptions from household activity |

## Technical Profile

| Attribute | Value |
|---|---|
| Technical literacy | High for networking and storage concepts |
| Software familiarity | Comfortable with NAS dashboards, SMB shares, media servers, and file paths |
| Data entry comfort | Low on TV remote; prefers minimal credential entry or pairing-style flows |
| Map / spatial comfort | High for folder hierarchy and library structure |
| Tolerance for complexity | Moderate during setup, low during playback |

## Time and Attention

| Attribute | Value |
|---|---|
| Typical work hours | Evenings and weekends |
| Peak usage times | After dinner, late evening, weekend chores |
| Session duration | 20 minutes to 2 hours |
| Interruption frequency | Medium; playback may continue while he leaves the app |
| Time pressure level | Low during setup, medium when resuming a book |
| Attention availability | Split between TV, household tasks, and audio listening |

## Decision Authority

| Attribute | Value |
|---|---|
| Strategic decisions | Chooses source architecture and library organization |
| Operational decisions | Runs sync, fixes credentials, changes source paths |
| Financial authority | Decides whether to install, subscribe, or purchase app |
| Delegation patterns | May set up app for other household members |
| Approval dependencies | None for local network sources |

## Workday Shape

| Period | Description |
|---|---|
| Morning | May resume playback briefly while preparing for the day |
| Midday | Rare use unless working from home |
| Afternoon | May manage files on the server from another device |
| Evening | Primary listening and TV interaction window |
| Seasonal variation | More long listening sessions during holidays and winter evenings |

## Goals

| Goal type | Description |
|---|---|
| Primary job goal | Connect an SMB library, index it once, and resume long audiobooks reliably from the TV |
| Secondary goals | Inspect sync health, identify unreachable files, adjust playback speed, recover from network interruptions |
| Success criteria | Library opens instantly from cache; playback resumes at exact position; source errors are diagnosable |
| Quality signals | Clear sync timestamps, stable focus movement, exact progress display, source-specific recovery actions |

## Frustrations

| Frustration type | Description |
|---|---|
| Current pain points | Media apps re-scan folders slowly, lose position, or treat chapters like music tracks |
| Legacy system complaints | Generic players expose file trees instead of book-level context |
| Workflow friction | Entering long passwords with a remote and discovering SMB auth issues late |
| Information gaps | Unclear whether a failure is Wi-Fi, permissions, stale cache, or unsupported stream access |

## Constraints for Flow Design

| Constraint | Implication |
|---|---|
| Device constraint | Use large focus targets, no dense forms, and clear D-pad order |
| Connectivity constraint | Keep browsing local and expose source health separately from playback |
| Time constraint | Resume flow must be one or two remote actions from app launch |
| Environment constraint | Status and controls must be readable from across the room |
| Authority constraint | Full settings and recovery controls can be available to this persona |
| Literacy constraint | Technical details can be shown progressively, not on the primary playback path |

## Jobs This Persona Performs

| Job ID | Job statement | Frequency | Notes |
|---|---|---|---|
| J-001 | Add an SMB audiobook library to the app | Initial setup and occasional source changes | Needs credential and path validation |
| J-002 | Resume the current audiobook from the exact position | Daily or weekly | Must avoid browse friction |
| J-003 | Inspect sync health and recover unreachable files | As needed | Needs provider/source-specific diagnostics |
| J-004 | Adjust playback speed and skip intervals | During listening | Must work with remote while audio plays |

## Scenarios Involving This Persona

| Scenario ID | Scenario name | Role in scenario |
|---|---|---|
| SC-01 | Connect a home SMB library | Source owner and setup actor |
| SC-02 | Resume a long book after leaving the app | Primary listener |
| SC-03 | Recover from a network or credential problem | Troubleshooter |
