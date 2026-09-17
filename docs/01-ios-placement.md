# iOS / iPadOS — place 3D models in the world

Primary how-to sample from Apple:

**[Placing objects and handling 3D interaction](https://developer.apple.com/documentation/arkit/placing-objects-and-handling-3d-interaction)**

Covers: `ARWorldTrackingConfiguration`, plane detection, `ARCoachingOverlayView`, raycasts / tracked raycasts, placing content at a focus position.

## Environmental analysis hub

Browse siblings from:

[Environmental analysis](https://developer.apple.com/documentation/arkit/environmental_analysis)

## Raycasting (RealityKit `ARView`)

| API | Doc |
|-----|-----|
| One-shot raycast | [raycast(from:allowing:alignment:)](https://developer.apple.com/documentation/realitykit/arview/raycast(from:allowing:alignment:)) |
| Continuous / drag refine | [trackedRaycast(...)](https://developer.apple.com/documentation/realitykit/arview/trackedraycast(from:allowing:alignment:updatehandler:)) |
| Build a query | [makeRaycastQuery(...)](https://developer.apple.com/documentation/realitykit/arview/makeraycastquery(from:allowing:alignment:)) |

## Plane detection

[planeDetection](https://developer.apple.com/documentation/arkit/arworldtrackingconfiguration/planedetection) on `ARWorldTrackingConfiguration`.

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
