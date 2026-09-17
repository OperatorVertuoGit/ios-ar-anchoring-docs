# visionOS / spatial anchoring

Anchoring beyond the window — durable world positions and object tracking.

## WWDC entry points

| Session | Why it matters |
|---------|----------------|
| [Meet ARKit for spatial computing — WWDC23](https://developer.apple.com/videos/play/wwdc2023/10082/) | `WorldAnchor` vs non-anchored content; automatic map/anchor persistence; you map identifier → content |
| [Evolve your ARKit app for spatial experiences — WWDC23](https://developer.apple.com/videos/play/wwdc2023/10091/) | iOS → visionOS; `AnchorEntity` vs `WorldTrackingProvider` + `WorldAnchor`; raycast then persist |
| [Enhance your spatial computing app with RealityKit — WWDC23](https://developer.apple.com/videos/play/wwdc2023/10081/) | `ImmersiveSpace` + `RealityView` + plane `AnchorEntity` |
| [Explore object tracking for visionOS — WWDC24](https://developer.apple.com/videos/play/wwdc2024/10101/) | Real-world objects as anchors; Composer Pro Object target; Create ML reference objects |
| [What’s new in RealityKit — WWDC25](https://developer.apple.com/videos/play/wwdc2025/287/) | `SpatialTrackingSession` + `AnchorStateEvents`; plane/table classification |

## World tracking APIs

- [WorldTrackingProvider](https://developer.apple.com/documentation/arkit/worldtrackingprovider)
- [WorldAnchor](https://developer.apple.com/documentation/arkit/worldanchor)
- [removeAnchor(_:)](https://developer.apple.com/documentation/arkit/worldtrackingprovider/removeanchor(_:))

## Pattern chooser

| Need | Prefer |
|------|--------|
| Simple surfaces (floor / wall / table) | `AnchorEntity(.plane(...))` in `ImmersiveSpace` |
| Durable world positions across sessions | `WorldTrackingProvider` + `WorldAnchor` (map UUID → content) |
| Known physical object | Object tracking + Reality Composer Pro Anchoring target **Object** |
