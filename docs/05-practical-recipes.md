# Practical recipes

LayoutAR-oriented patterns. Cite Apple docs from this repo; do not invent APIs.

## Units

RealityKit world space is **meters**.

```swift
let feet: Float = 12.5          // decimal feet from layout
let meters = feet * 0.3048
```

## iOS RealityKit — tap to place (checklist)

1. Run `ARWorldTrackingConfiguration` with `planeDetection = [.horizontal]` (and/or `.vertical`).
2. Show `ARCoachingOverlayView` until tracking is ready.
3. On tap: `arView.raycast(from:allowing:alignment:)` → take first result’s `worldTransform`.
4. Create `ARAnchor(transform:)` and/or `AnchorEntity(world:)` / `AnchorEntity(anchor:)`.
5. Load USDZ/Reality model as `Entity`; `addChild` to the `AnchorEntity`; add anchor to the scene.
6. Prefer **tracked** raycasts while dragging for continuous placement refinement.

Primary Apple sample: [Placing objects and handling 3D interaction](https://developer.apple.com/documentation/arkit/placing-objects-and-handling-3d-interaction).

## Construction-style geometry (nails → midpoints → rectangles)

1. Capture / place markers at real-world features (e.g. footing offset nails).
2. Compute midpoints / edges in **meters**.
3. Anchor a rectangle (or guide mesh) with a world/plane `AnchorEntity`.
4. Keep visualization colors bright and distinct.
5. Do **not** rely on AR Quick Look alone for this — use a RealityKit app session.

## visionOS variants

- **Surfaces:** `AnchorEntity(.plane(...))` inside `ImmersiveSpace` + `RealityView`.
- **Durable poses:** `WorldTrackingProvider.addAnchor(WorldAnchor)` and map UUID → content.
- **Known objects:** Object tracking + Reality Composer Pro Anchoring target **Object**.

## When generating code or briefs

- Prefer RealityKit `AnchorEntity` / `AnchoringComponent` for new iOS placement.
- Link official Apple URLs from [REFERENCES.md](../REFERENCES.md).
- Only pull in RoomPlan if room mesh is actually required (separate from simple model anchoring).
