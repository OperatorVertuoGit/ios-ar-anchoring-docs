# Maintenance

## Purpose

Keep this repo’s Apple AR anchoring index current for LayoutAR / Reality Lab work.

## Primary sources

1. **This GitHub repo** — day-to-day readable index Jarvis uses when helping with code and briefs.
2. **Drive pack** (Debian-curated Google Doc in Reality-Lab): also treated as a curated index of official Apple URLs.

## Weekly check (Mondays 9:00 AM PT)

Jarvis should:

1. Re-open key starter URLs in [REFERENCES.md](REFERENCES.md) and the docs under `docs/`.
2. Note broken links, renamed APIs, deprecations, or clearly newer official guidance / WWDC sessions that belong here.
3. Update this repo (and the Drive index when appropriate).
4. **Always** notify Brennan in chat — even if nothing changed (“checked YYYY-MM-DD — all current” or a short changelog).

## Editing rules

- Prefer official Apple Developer docs and WWDC sessions.
- Keep the pack **curated and anchoring-focused** — not a scrape of every AR page.
- Do **not** invent APIs. Cite Apple URLs in briefs and generated code comments.
- Preserve LayoutAR conventions: RealityKit runtime, ft→m `× 0.3048`, bright AR colors.

## Last freshness check

- **2026-09-21 (PT):** Existing Apple doc + WWDC links still live and titles match. Added WWDC26 object-tracking / RealityKit sessions, `SpatialTrackingSession`, combining-spatial-support doc, and canonical `environmental-analysis` URL. Drive Google Doc still dated 2026-09-17 — needs parallel WWDC26 additions when editable.
