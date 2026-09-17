# iOS / visionOS AR Anchoring Docs

Curated official Apple documentation for **placing and anchoring 3D models in the real world** with ARKit, RealityKit, Reality Composer / Reality Composer Pro, and visionOS.

This repo is Jarvis’s working index for LayoutAR-class work (anchor geometry to real-world features). Prefer **RealityKit at runtime** over AR Quick Look when you need real anchors.

## Quick start

| Goal | Start here |
|------|------------|
| Place a model on a detected plane (iOS) | [iOS placement](docs/01-ios-placement.md) |
| Core APIs (anchors, entities, tracking) | [Core concepts](docs/00-core-concepts.md) |
| Reality Composer / Composer Pro | [Composer tools](docs/02-reality-composer.md) |
| visionOS / persistence | [visionOS anchoring](docs/03-visionos.md) |
| WWDC sessions | [WWDC index](docs/04-wwdc-sessions.md) |
| Copy-paste recipes (ft→m, tap-to-place) | [Practical recipes](docs/05-practical-recipes.md) |
| Geospatial BIM, survey, Trimble, tilt-up | [Construction context brief](docs/06-geospatial-bim-tiltup.md) |
| Full link list | [REFERENCES.md](REFERENCES.md) |

## Project conventions (Brennan / LayoutAR)

- **Stack:** RealityKit + Reality Composer / Reality Composer Pro
- **Units:** RealityKit uses **meters**. Convert decimal feet with `× 0.3048`
- **Colors:** Bright, distinct AR visualization colors
- **Not enough alone:** AR Quick Look can show USDZ but does **not** run full anchor/component behavior
- **Field grammar:** nails/marks → lines → midpoints → dig/pour rectangles → panel edges (tilt-up / foundations)

## Maintenance

A weekly automation (Mondays 9:00 AM PT) checks Apple’s pages against this index, updates stale links/notes when needed, and notifies Brennan in chat.

Source Drive pack (also curated by Debian): see `MAINTENANCE.md`.

## License / provenance

Links and summaries point at Apple’s official Developer documentation and WWDC sessions. This repo does not redistribute Apple’s copyrighted page bodies — only curated indexes, recipes, and project notes. The construction context brief cites Trimble, ACI/TCA, Autodesk, and other primary sources without redistributing their manuals.
