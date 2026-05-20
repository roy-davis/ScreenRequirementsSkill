> ⚠️ Provisional — estimated from context, not supplied or validated. Review before treating as authoritative.

# Design System Context

> Referenced by all screen artifacts. Every component in screen specs must be named here.

## Framework
- Framework: Native Android TV using Jetpack Compose for TV.
- Component library: androidx.tv:tv-material.
- Library version: Current stable compatible with target Android TV build.
- Documentation URL: https://developer.android.com/training/tv/playback/compose
- Styling: Kotlin theme tokens with Material-style semantic colors.
- Dark mode: Yes, dark-first; light mode optional for testing and screenshots.
- Custom component overrides: TV-safe focus cards, provider tiles, large playback controls, progress rail, source health badges, and folder rows.

## Visual tone
Quiet, media-focused, and technical enough to be trusted by home storage users while staying readable and calm from across the room.

## Color tokens

| Token | Light | Dark | Usage |
|---|---|---|---|
| primary | #1C6E5A | #38C7A1 | Main actions, focus ring, active playback |
| primary-foreground | #FFFFFF | #071713 | Text on primary |
| secondary | #455A64 | #8FA4AE | Secondary actions |
| secondary-foreground | #FFFFFF | #0D1417 | Text on secondary |
| background | #F5F7F8 | #0B1114 | Page background |
| foreground | #11181C | #F2F6F7 | Default text |
| card | #FFFFFF | #141C20 | Card and panel backgrounds |
| card-foreground | #11181C | #F2F6F7 | Text on cards |
| muted | #E7ECEF | #223036 | Subtle panels and inactive tracks |
| muted-foreground | #5A6970 | #A8B7BE | Secondary text |
| border | #CBD5DA | #33444C | Dividers and idle outlines |
| error | #B42318 | #FF8A7A | Blocking errors |
| error-foreground | #FFFFFF | #250604 | Text on error |
| success | #247A4D | #4DD083 | Healthy source and complete sync |
| success-foreground | #FFFFFF | #06150D | Text on success |
| warning | #9A6700 | #F2BF4B | Partial sync, degraded source, buffering |
| warning-foreground | #1B1200 | #1B1200 | Text on warning |
| info | #2563A8 | #78B7FF | Informational source details |
| info-foreground | #FFFFFF | #051322 | Text on info |

## Typography

| Role | Family | Size | Weight | Line height |
|---|---|---|---|---|
| Heading 1 | Roboto / system sans | 40 sp | 600 | 48 sp |
| Heading 2 | Roboto / system sans | 30 sp | 600 | 38 sp |
| Heading 3 | Roboto / system sans | 24 sp | 600 | 32 sp |
| Body | Roboto / system sans | 18 sp | 400 | 28 sp |
| Body small | Roboto / system sans | 15 sp | 400 | 22 sp |
| Label | Roboto / system sans | 16 sp | 600 | 22 sp |
| Caption | Roboto / system sans | 13 sp | 500 | 18 sp |
| Code | Roboto Mono / monospace | 15 sp | 400 | 22 sp |

## Spacing
- Base unit: 8 dp.
- Scale: 8, 16, 24, 32, 48, 64, 80, 96 dp.
- Component padding: 16 to 24 dp for compact controls; 24 to 32 dp for TV focus cards.
- Section gap: 32 to 48 dp.

## Layout
- Grid: TV-safe horizontal shelves and 5-column catalog grid at 1920 px width.
- Breakpoints: TV 960 dp, 1280 dp, 1920 dp; mobile is not a primary target.
- Container max width: Fill viewport within overscan-safe margins.
- Page padding mobile: Not required for Phase 1 except preview fallback.
- Page padding desktop: 64 dp minimum; 96 dp for primary layouts on large TVs.

## Shape
- Border radius default: 8 dp.
- Border radius scale: 4 dp small, 8 dp default, 12 dp large focus surfaces.
- Elevation style: Low shadow and clear outline; focus is shown with scale, border, and color shift rather than heavy elevation.

## Navigation
- Pattern: Left-to-right D-pad focus within shelves, vertical movement between regions, Back returns to the previous screen or parent region.
- Sidebar width: Settings screens may use a 320 dp source list at left.
- Mobile navigation: Not required; prototype may stack regions for narrow preview only.
- Active state style: 1.06 scale, 3 dp primary outline, brighter surface, and label contrast increase.

## Interaction patterns

### Loading
- Approach: Skeleton rows for cached data loading; explicit buffering state for playback.
- Button loading: Preserve button size, show progress indicator plus short verb phrase such as "Connecting" or "Authorizing".

### Empty states
- Structure: Clear cause, one primary recovery action, one secondary escape action.
- Tone: Plain and specific; distinguish "no books found" from "source unavailable".

### Error states
- Inline field: Field-level copy for required host, path, or credentials.
- Form level: Source-specific connection summary with retry and edit actions.
- Page level: Recovery screen only for blocking source, authorization, or playback failures.

### Success feedback
- Style: Status badge, toast for short-lived saves, and progress transition for sync completion.
- Toast duration: 4 seconds unless playback state changes immediately.

### Modals
- Sizes: TV-safe centered confirmation panel no wider than 720 dp.
- Overlay: Dim background without hiding current context.
- Dismiss on overlay click: No; D-pad users dismiss with Back or explicit Cancel.

### Toasts
- Position: Bottom-right within overscan-safe margin.
- Duration: 4 seconds.

### Confirmation (destructive)
- Style: Modal for source removal and clearing progress.
- Require typing: No on TV; require focused destructive confirmation and secondary Cancel.

## Tone and voice
- Label casing: Sentence case.
- Button copy: Verb-first and short, for example "Add source", "Use this folder", "Resume", "Retry".
- Error tone: Specific, calm, and recoverable.
- Help text: Explain storage and provider behavior without protocol jargon unless in advanced details.

## Icons
- Set: Material Symbols or Android TV platform icons.
- Sizes: 24 dp standard, 32 dp primary control, 48 dp media transport icon targets.
- Stroke weight: Platform default; use filled icons for active media controls.

## Accessibility
- WCAG level: AA minimum for text and critical indicators.
- Text contrast: 4.5:1 for text, 3:1 for large headings and non-text controls.
- UI contrast: Focus outline must be visible at 3:1 against adjacent surfaces.
- Focus style: Scale plus outline plus foreground/background contrast change; never rely on color alone.
- Reduced motion: Focus scale and loading animations can be reduced to opacity/outline transitions.

## Notes
- All core content must remain within a 48 dp to 96 dp overscan-safe margin.
- Do not use network browsing during normal catalog navigation; status indicators can reflect source health separately.
- Screens should use realistic audiobook examples and provider labels, not placeholder text.
