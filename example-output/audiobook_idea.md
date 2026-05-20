# **Product Requirement Document (PRD)**

## **Project Title: Local & Cloud Network Audiobook Player for Android TV**

**Document Version:** 1.0

**Status:** Draft

## ---

**1\. Executive Summary & Objective**

The objective of this project is to build a lean, high-performance, native Android TV audiobook application optimized for the "10-foot user experience."

Unlike mobile players, this app will not rely on internal device storage. Instead, it features a decoupled architecture: a localized **Metadata Database** handles ultra-fast library browsing and state persistence, while the core playback engine streams media assets on-demand using provider-backed **Playable Asset URIs** pointing to local home shares (SMB) and first-class cloud connectors.

## ---

**2\. Target User Persona & Problem Statement**

* **Target User:** Home server hobbyists or cloud storage users who want to enjoy long-form audiobooks on their home theater or television setup.  
* **The Problem:** Current Android TV media players are heavily optimized for video or short-form music tracks. They lack granular audiobook persistence (remembering the exact millisecond across a 40-hour file), handle remote D-pad focus poorly, and suffer from high UI latency when reading large directory trees over local network shares or cloud providers in real-time.

## ---

**3\. High-Level System Architecture**

The application implements a localized cache architecture to insulate UI performance from network fluctuations.

\+-----------------------------------------------------------------+  
|                        Android TV UI                            |  
|             (Jetpack Compose for TV \- 100% Focused)            |  
\+-------------------------------++--------------------------------+  
                                ||  
                                v  
\+-------------------------------+---------------------------------+  
|                    Local Metadata Cache                         |  
|  (Room SQLite DB: Stores Title, Author, Progress, & Asset Ref)  |  
\+-------------------------------++--------------------------------+  
                                ||  
                                v  
\+-------------------------------+---------------------------------+  
|                       Playback Engine                           |  
|       (Jetpack Media3 ExoPlayer \+ MediaSession Service)         |  
\+-------------------------------++--------------------------------+  
                                ||  
                                v  
\+-------------------------------+---------------------------------+  
|                    Remote Storage Targets                       |  
|   (SMB / Dropbox / Google Drive / iCloud Drive / OneDrive /     |  
|              Optional WebDAV-Compatible Providers)              |  
\+-----------------------------------------------------------------+

## ---

**4\. Functional Requirements**

### **4.1 Storage & Protocol Integration**

* **FR-1.1 (SMB Connectivity):** The app must connect to local Samba/Windows network shares using SMB2/SMB3 protocols via username/password or anonymous login.  
* **FR-1.2 (First-Class Cloud Connectors):** The app must support Dropbox, Google Drive, iCloud Drive, and OneDrive as first-class source types with branded setup flows, provider-specific authorization, provider-specific folder browsing, and provider-specific error handling.  
* **FR-1.3 (Optional WebDAV Connectivity):** The app may support generic WebDAV-compatible storage as an advanced/custom source, but WebDAV must not be treated as the implementation path for Dropbox, Google Drive, iCloud Drive, or OneDrive.  
* **FR-1.4 (Credential & Token Security):** All network configurations, IP addresses, credentials, OAuth tokens, refresh tokens, app passwords, session references, and provider account metadata must be stored securely using native encrypted Android storage.  
* **FR-1.5 (Provider Capability Model):** Each source must expose its capabilities to the app, including whether it can list folders, provide direct/ranged media downloads, refresh authorization silently, and support resumable streaming.

### **4.2 Library Indexing & Metadata Caching**

* **FR-2.1 (Asynchronous Background Sync):** The app must crawl connected network directories in the background using WorkManager. It will interpret folder hierarchies as *Books* and individual audio files as *Chapters/Tracks*.  
* **FR-2.2 (Metadata Database Persistence):** The app must index library entries into a local Room SQLite database. The key data matrix required per file includes:  
  * Book Title, Author, and calculated unique ID (folder path hash).  
  * Chapter Title, Track Number, provider source ID, provider file ID/path, and a playable asset reference that can resolve to a streamable SMB URI or provider download URL at playback time.  
* **FR-2.3 (UI Independence):** The browsing UI must populate *exclusively* from the local SQLite database to guarantee immediate rendering without hitting the network thread during navigation.

### **4.3 Media Playback Engine**

* **FR-3.1 (On-Demand Progressive Streaming):** When a user selects a track, Jetpack Media3 (ExoPlayer) must use the stored playable asset reference to resolve a streamable SMB URI or provider download URL and stream data progressively.  
* **FR-3.2 (Granular Progress Tracking):** The app must save the exact millisecond playback position to the local database every 5 seconds of active playback, and instantly upon pause, track skip, or application termination.  
* **FR-3.3 (Persistent Background Service):** Media playback must operate inside an Android foreground service (MediaSessionService). Audio must persist if the user navigates away to the Android TV dashboard.  
* **FR-3.4 (Audiobook Controls):** The playback architecture must support variable playback speed ($0.5\\times$ to $2.5\\times$) and native seek caching to prevent buffering pauses during small interval skips (e.g., skip back 15 seconds).

### **4.4 TV-First User Interface**

* **FR-4.1 (D-Pad Navigation):** Every interactive layout component must explicitly respond to hardware D-pad remote arrow events.  
* **FR-4.2 (Visual Focus Indicators):** Elements currently selected by the remote must feature an overt visual transition, scaling up by $1.05\\times$ to $1.1\\times$ with distinct color variations.  
* **FR-4.3 (Overscan Protection):** Core layout components must stay within a safe margin ($48\\text{dp}$ to $96\\text{dp}$ from outer edges) to avoid asset clipping on legacy TV displays.

## ---

**5\. Non-Functional Requirements**

* **Performance (Latency):** The browsing screen grid must render catalog details in under 100ms when waking from cache. The audio stream must initiate within 2000ms of selecting a track over a typical standard 5GHz home Wi-Fi connection.  
* **Resilience:** The player engine must configure an aggressive network buffer window to withstand brief network dropouts or router power-saving states without interrupting audio playback.  
* **Target SDK Compatibility:** Minimum SDK level 26 (Android 8.0) up to the latest modern Android TV versions.

## ---

**6\. Technical Stack Specifications**

The engineering pipeline should leverage the following Kotlin-centric libraries to keep code footprint small and modular:

| Layer | Recommended Technical Tooling | Purpose |
| :---- | :---- | :---- |
| **UI** | Jetpack Compose for TV (androidx.tv:tv-material) | Modern declarative, D-pad native layouts. |
| **Core Core Engine** | Jetpack Media3 (ExoPlayer \+ MediaSession) | Background service handling, streaming, and progress caching. |
| **Database** | Room SQLite Persistence | Local metadata architecture and progress saving. |
| **SMB Client** | jCIFS-NG | Pure Kotlin/Java bridge for modern SMB2/SMB3 communication. |
| **Cloud Connectors** | Provider-specific REST/API clients | Dropbox, Google Drive, iCloud Drive, and OneDrive folder listing, auth, file metadata, and playable media URL resolution. |
| **Async Threading** | Kotlin Coroutines & Flow | Asynchronous database reads and event streams. |
| **DI** | Hilt | Streamlined dependency injection with minimal boilerplate. |

## ---

**7\. Future Scope & Exclusions (Phase 2\)**

* Local on-device file downloading (Phase 1 focus is strictly on-demand streaming).  
* Metadata scraping from third-party APIs (Phase 1 infers metadata entirely from local folder structures and standard ID3 audio tags).
