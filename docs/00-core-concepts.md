# Core concepts

Read these first — they define how Apple represents “a thing stuck to the real world.”

## ARKit: session-level anchors

**[ARAnchor](https://developer.apple.com/documentation/arkit/aranchor)**  
Position and orientation of content in the physical environment. Adding anchors helps ARKit optimize tracking near that point; move anchors when content moves.

**[ARWorldTrackingConfiguration](https://developer.apple.com/documentation/arkit/arworldtrackingConfiguration)**  
6DOF device tracking relative to the environment. Key knobs for placement work:

- `planeDetection` — horizontal / vertical planes
- `initialWorldMap` — resume / relocalize a prior session

Related specialized anchors:

| Type | Use when |
|------|----------|
| [ARPlaneAnchor](https://developer.apple.com/documentation/arkit/arplaneanchor) | Detected floors, tables, walls |
| [ARImageAnchor](https://developer.apple.com/documentation/arkit/arimageanchor) | Known image targets |
| [ARObjectAnchor](https://developer.apple.com/documentation/arkit/arobjectanchor) | Scanned 3D reference objects |
| [ARWorldMap](https://developer.apple.com/documentation/arkit/arworldmap) | Save / load world for relocalization on iOS |

## RealityKit: entity-level anchors

**[AnchorEntity](https://developer.apple.com/documentation/realitykit/anchorentity)**  
Tethers entities to a scene or real-world target. Plane convenience init:

[AnchorEntity(plane:classification:minimumBounds:)](https://developer.apple.com/documentation/realitykit/anchorentity/init(plane:classification:minimumbounds:))

**[AnchoringComponent](https://developer.apple.com/documentation/realitykit/anchoringcomponent)**  
Attach any `Entity` to a real-world target (planes, images, objects, world positions, etc.).

## Mental model

```
ARSession / World tracking  →  finds real-world features
        ↓
ARAnchor / WorldAnchor      →  stable pose in the world
        ↓
AnchorEntity + children     →  your USDZ / Reality content
```

For new iOS placement work, prefer **RealityKit `AnchorEntity` / `AnchoringComponent`** on top of world tracking + raycasts.
