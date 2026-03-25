# Demo Mode Guide

## Overview
Demo mode is a launch-only path intended for product demos and App Review walkthroughs.

When demo mode is active:
- Yuumi pauses live LCU/Web polling.
- Yuumi shows a floating panel on the left with a catalog of demo snapshots.
- Snapshot state is stable until you load another snapshot.

## Entering Demo Mode
You can enter demo mode through either path:
1. Setup path:
   - Open Yuumi and go to the setup workflow.
   - **Hover near the bottom-right corner of setup and click `Start Demo Mode` when it appears.**
   - Yuumi initializes Data Dragon for the selected locale, then opens the main app in demo mode.
2. Diagnostics path:
   - Open the footer diagnostics panel and switch to the `snapshots` tab.
   - Click `Start Demo Mode` to enter immediately.
   - This path does not gate on Data Dragon readiness; if assets are missing, some snapshot rendering can be incomplete until Data Dragon is initialized.

Notes:
- Demo mode does not mark setup complete for the current app version.
- Demo mode is not persisted across restarts.

## Demo Sidebar
The demo panel floats over the main UI on the left and does not change the main content container's position.
It lists all available snapshots from two sources:
- `bundled`: JSON snapshots shipped with the app resources.
- `local`: JSON snapshots collected on your machine.

Each row includes:
- Snapshot name
- Description
- Captured game state
- Captured phase
- Source (`bundled` or `local`)

## Filters
The sidebar supports multi-select filters:
- `Source` (`bundled` / `local`)
- `Game State`
- `Phase` (uses `none` when phase is missing)

Source is shown first in the filter list.
Each filter chip shows a live faceted count (for example `local (3)`), and the sidebar shows a filtered total as `Snapshots: N`.

A snapshot must match all selected filter groups to appear.
Single click toggles a value on/off. Double-clicking a value selects only that value within its filter group.
Use the collapse control in the panel header to switch to an icon-only box, then click the expand icon to restore the full panel.

## Collecting Local Snapshots
Snapshot collection is only available in normal (non-demo) mode.

1. Open the footer diagnostics panel.
2. Open the `snapshots` tab.
3. Enter a snapshot name.
4. Enter a snapshot description.
5. Click `Collect Snapshot`.

Yuumi automatically captures:
- Current game state
- Current phase
- Current UI-state payload used for rendering the main app

Local snapshots are saved to:
`~/Library/Application Support/Yuumi/DemoSnapshots`

The `snapshots` tab also shows `Total / Bundled / Local` counts and includes an `Open Folder` button to reveal this directory in Finder.

## Bundled Snapshot Authoring
Bundled snapshot files live at:
`Sources/Yuumi/Resources/DemoSnapshots/`
This directory is included in the app bundle via a folder reference, so new files do not need per-file `Yuumi.xcodeproj` edits.

Recommended naming:
- `demo-snapshot-<short-name>.json`

Each file should use the `DemoSnapshotEnvelope` structure:
- `metadata`: id, name, description, createdAt, source, appVersion, build, capturedState, capturedPhase
- `payload`: renderable UI state fields consumed by the store

At runtime, Yuumi enforces source labeling by load location (`bundled` vs `local`).
