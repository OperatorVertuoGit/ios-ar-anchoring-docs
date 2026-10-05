# Full reference links

Official Apple URLs used by this repo. Prefer these over third-party blogs when helping with anchoring.

Last checked: 2026-10-05 (PT).

## Core concepts

- [ARAnchor](https://developer.apple.com/documentation/arkit/aranchor)
- [ARWorldTrackingConfiguration](https://developer.apple.com/documentation/arkit/arworldtrackingconfiguration)
- [AnchorEntity](https://developer.apple.com/documentation/realitykit/anchorentity)
- [AnchorEntity plane init](https://developer.apple.com/documentation/realitykit/anchorentity/init(plane:classification:minimumbounds:))
- [AnchoringComponent](https://developer.apple.com/documentation/realitykit/anchoringcomponent)
- [SpatialTrackingSession](https://developer.apple.com/documentation/realitykit/spatialtrackingsession) — RealityKit-managed spatial tracking / authorization (iOS + visionOS)

## iOS / iPadOS placement

- [Placing objects and handling 3D interaction](https://developer.apple.com/documentation/arkit/placing-objects-and-handling-3d-interaction)
- [Environmental analysis](https://developer.apple.com/documentation/arkit/environmental-analysis)
- [ARView.raycast](https://developer.apple.com/documentation/realitykit/arview/raycast(from:allowing:alignment:))
- [ARView.trackedRaycast](https://developer.apple.com/documentation/realitykit/arview/trackedraycast(from:allowing:alignment:updatehandler:))
- [ARView.makeRaycastQuery](https://developer.apple.com/documentation/realitykit/arview/makeraycastquery(from:allowing:alignment:))
- [planeDetection](https://developer.apple.com/documentation/arkit/arworldtrackingconfiguration/planedetection-swift.struct)
- [ARPlaneAnchor](https://developer.apple.com/documentation/arkit/arplaneanchor)
- [ARImageAnchor](https://developer.apple.com/documentation/arkit/arimageanchor)
- [ARObjectAnchor](https://developer.apple.com/documentation/arkit/arobjectanchor)
- [ARWorldMap](https://developer.apple.com/documentation/arkit/arworldmap)
- [Forum: align 3D model in real world](https://developer.apple.com/forums/thread/723558)

## Object tracking (known physical objects)

- [ARWorldTrackingConfiguration.trackingObjects](https://developer.apple.com/documentation/arkit/arworldtrackingconfiguration/trackingobjects) — **iOS / iPadOS 27+**: detect and track physical objects inside a world-tracking session
- [ObjectTrackingProvider](https://developer.apple.com/documentation/arkit/objecttrackingprovider) — visionOS 2+
- [Implementing object tracking in your app](https://developer.apple.com/documentation/visionos/implementing-object-tracking-in-your-app) — train reference objects and track them
- [Exploring object tracking with ARKit](https://developer.apple.com/documentation/visionos/exploring_object_tracking_with_arkit) — sample

## Reality Composer / Composer Pro

- [Using a reference object with Reality Composer Pro](https://developer.apple.com/documentation/visionos/using-a-reference-object-with-reality-composer-pro)
- [Adding 3D content to your app](https://developer.apple.com/documentation/visionos/adding-3d-content-to-your-app)

## visionOS

- [WorldTrackingProvider](https://developer.apple.com/documentation/arkit/worldtrackingprovider)
- [WorldAnchor](https://developer.apple.com/documentation/arkit/worldanchor)
- [removeAnchor(_:)](https://developer.apple.com/documentation/arkit/worldtrackingprovider/removeanchor(_:))
- [Combining spatial support from multiple frameworks](https://developer.apple.com/documentation/visionos/combining-spatial-support-from-multiple-frameworks) — `SpatialTrackingSession` + `AnchorStateEvents` patterns
- [SharedCoordinateSpaceProvider](https://developer.apple.com/documentation/arkit/sharedcoordinatespaceprovider) — visionOS 26+: shared world coordinate space among nearby participants
- [WorldAnchor.isSharedWithNearbyParticipants](https://developer.apple.com/documentation/arkit/worldanchor/issharedwithnearbyparticipants) — visionOS 26+: persist/share world anchors across nearby devices

## Framework hubs

- [ARKit](https://developer.apple.com/documentation/arkit)
- [RealityKit](https://developer.apple.com/documentation/realitykit)
- [visionOS](https://developer.apple.com/documentation/visionos)
- [ARKit updates (Apple changelog)](https://developer.apple.com/documentation/updates/arkit) — check here first for newly added anchoring APIs
- [WWDC26 visionOS guide](https://developer.apple.com/wwdc26/guides/visionos/)

## WWDC

See [docs/04-wwdc-sessions.md](docs/04-wwdc-sessions.md).
