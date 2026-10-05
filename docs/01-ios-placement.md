# iOS / iPadOS — place 3D models in the world

Primary how-to sample from Apple:

**[Placing objects and handling 3D interaction](https://developer.apple.com/documentation/arkit/placing-objects-and-handling-3d-interaction)**

Covers: `ARWorldTrackingConfiguration`, plane detection, `ARCoachingOverlayView`, raycasts / tracked raycasts, placing content at a focus position.

## Environmental analysis hub

Browse siblings from:

[Environmental analysis](https://developer.apple.com/documentation/arkit/environmental-analysis)

## Raycasting (RealityKit `ARView`)

| API | Doc |
|-----|-----|
| One-shot raycast | [raycast(from:allowing:alignment:)](https://developer.apple.com/documentation/realitykit/arview/raycast(from:allowing:alignment:)) |
| Continuous / drag refine | [trackedRaycast(...)](https://developer.apple.com/documentation/realitykit/arview/trackedraycast(from:allowing:alignment:updatehandler:)) |
| Build a query | [makeRaycastQuery(...)](https://developer.apple.com/documentation/realitykit/arview/makeraycastquery(from:allowing:alignment:)) |

## Plane detection

[planeDetection](https://developer.apple.com/documentation/arkit/arworldtrackingconfiguration/planedetection-swift.struct) on `ARWorldTrackingConfiguration`.

## Apple staff guidance (forums)

[How to align a 3D model in the real world](https://developer.apple.com/forums/thread/723558)  
Summary: Reality Composer template **or** `ARSession` + plane detection + Entity/SCNNode; use raycast for tap-to-place.

## Xcode starting point

**New Project → Augmented Reality App → RealityKit**  
Places a cube on a detected plane; open in Reality Composer to swap models.

## LayoutAR-oriented flow

1. Enable horizontal (and/or vertical) plane detection.
2. Coach until tracking is ready.
3. Raycast from tap / focus → `worldTransform`.
4. Create `ARAnchor` and/or `AnchorEntity`.
5. Load USDZ/Reality as child of the anchor.
6. Convert layout feet → meters (`× 0.3048`) before sizing or offsets.

See [Practical recipes](05-practical-recipes.md).

## Object tracking on iOS (iOS / iPadOS 27+)

Apple added [trackingObjects](https://developer.apple.com/documentation/arkit/arworldtrackingconfiguration/trackingobjects) to `ARWorldTrackingConfiguration` (listed in [ARKit updates, June 2026](https://developer.apple.com/documentation/updates/arkit)): detect and track known physical objects inside a normal world-tracking session. Useful when anchoring to a known physical object rather than a plane alone. Background: [Explore enhancements to visionOS object tracking — WWDC26](https://developer.apple.com/videos/play/wwdc2026/283/).

Note: requires the iOS 27 SDK (Xcode 27 era). Brennan’s 2017 iMac on Ventura / Xcode 15.2 can’t build against it — treat as future path, not MVP.
