# Reality Composer / Reality Composer Pro

Authoring tools that pair with RealityKit. Useful for LayoutAR when artists iterate on USDZ / Reality content and anchoring targets.

## Sessions

- [Building AR Experiences with Reality Composer — WWDC19](https://developer.apple.com/videos/play/wwdc2019/609/)
- [The artist’s AR toolkit — WWDC20](https://developer.apple.com/videos/play/wwdc2020/10601/)

## visionOS + reference objects

**[Using a reference object with Reality Composer Pro](https://developer.apple.com/documentation/visionos/using-a-reference-object-with-reality-composer-pro)**

Typical path:

1. Immersive Space + RealityKit
2. Open `.realitycomposerpro`
3. Transform entity → **Anchoring** component → Target: **Object**
4. Import `.referenceobject`
5. Place USDZ children
6. Run `SpatialTrackingSession` at runtime

## App integration overview

**[Adding 3D content to your app](https://developer.apple.com/documentation/visionos/adding-3d-content-to-your-app)**  
RealityKit + SwiftUI; `ImmersiveSpace` for unbounded placement; ARKit after permission for surroundings.

## Important limitation

**AR Quick Look** often displays USDZ but does **not** run full anchor / component behavior. A **runtime RealityKit app** is required for real anchoring.
