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
| [Explore enhancements to visionOS object tracking — WWDC26](https://developer.apple.com/videos/play/wwdc2026/283/) | High-frame-rate / handheld objects; metric-space poses; **object tracking on iOS** |

## World tracking APIs

- [WorldTrackingProvider](https://developer.apple.com/documentation/arkit/worldtrackingprovider)
- [WorldAnchor](https://developer.apple.com/documentation/arkit/worldanchor)
- [removeAnchor(_:)](https://developer.apple.com/documentation/arkit/worldtrackingprovider/removeanchor(_:))
- [SpatialTrackingSession](https://developer.apple.com/documentation/realitykit/spatialtrackingsession)
- [Combining spatial support from multiple frameworks](https://developer.apple.com/documentation/visionos/combining-spatial-support-from-multiple-frameworks)

## Pattern chooser

| Need | Prefer |
|------|--------|
| Simple surfaces (floor / wall / table) | `AnchorEntity(.plane(...))` in `ImmersiveSpace` |
| Durable world positions across sessions | `WorldTrackingProvider` + `WorldAnchor` (map UUID → content) |
| Known physical object | Object tracking + Reality Composer Pro Anchoring target **Object** |
