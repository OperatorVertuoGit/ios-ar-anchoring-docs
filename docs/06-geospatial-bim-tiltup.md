# Working Brief: Geospatial BIM, Survey, Trimble & Tilt-Up for LayoutAR (Jarvis)

**Audience:** Assistant (Jarvis) supporting Brennan on LayoutAR-class construction AR  
**Focus:** Anchor design geometry to real-world features; RealityKit; units **decimal feet → meters × 0.3048**  
**Research date:** 2026-09-17 (PT). Product names/versions are time-sensitive—verify against Trimble/vendor docs before asserting current SKUs.

---

## Purpose

This brief gives practical construction-site context so Jarvis can reason about:

- How BIM geometry relates to survey control and georeferencing
- What field layout / stakeout tools and accuracy mean on site
- Where Trimble hardware/software fit in design → field → as-built loops
- How tilt-up construction creates layout demands (footings, panel edges, embeds)
- How that maps to LayoutAR: nails/marks → lines/midpoints → dig/pour rectangles → panel edge layout

**Hard unit rule for LayoutAR math:** convert US decimal feet to SI meters with the international foot: **meters = feet × 0.3048**. Prefer international foot unless a project CRS or surveyor explicitly states U.S. survey foot (legacy; deprecated nationally as of 2023 for most new work—still may appear in older control).

---

## 1. Modern geospatial tech in construction BIM

### 1.1 How GIS / geospatial intersects BIM

- **BIM** (Revit, IFC, civil/structural models) holds building/site geometry, elements, and relationships in a project coordinate system.
- **GIS / CRS** places that geometry on Earth: projected eastings/northings, height above a vertical datum, and orientation (true north / grid north).
- On construction sites, crews usually work in a **local site coordinate system** tied to a **control network**, not raw WGS84 lat/lon. Geospatial metadata (EPSG codes, map conversion) links local ↔ global.
- Civil/structural models (roads, grading, utilities) often live closer to survey/GIS conventions; architectural/structural building models often stay near a local origin for numerical stability, with georef stored as a transform.

**Practical rule:** Keep model geometry near a local origin (project base / site zero). Carry real-world position as shared coordinates / CRS metadata—not by putting multi-million-foot eastings into every vertex.

### 1.2 Common CRS, projections, local site coords, control

| Concept | What it is on site |
| --- | --- |
| **CRS / EPSG** | Named coordinate reference system (e.g. State Plane zone, UTM). Identified by EPSG code when used. |
| **Projected coordinates** | Easting / Northing on a map projection (meters or feet). |
| **Geodetic / geographic** | Lat/lon on an ellipsoid (NAD83, WGS84, etc.)—rarely used for stakeout marks. |
| **Vertical datum** | NAVD88, local project datum, or site benchmark elevation. |
| **Local site / grid** | Contractor “N/E/Elev” system from control points; may be rotated/scaled vs true geographic. |
| **Site calibration / localization** | GNSS points matched to local control so RTK works in site coords. |
| **Control network** | Intervisible monuments, nails, hubs with known N/E/Z used to set up total stations and verify GNSS. |

**Control hierarchy (typical):** Primary control (surveyor) → secondary/site control (contractor) → temporary layout marks (daily stakeout).

### 1.3 Reality capture methods

- **TLS (terrestrial laser scanning):** Tripod scanners (e.g. Trimble X9/X12 class). Millimeter-to-centimeter dense clouds indoors/structures; needs registration + control.
- **Photogrammetry:** Photos → mesh/cloud (ground or UAV). Good coverage/texture; accuracy depends on control/GCPs and processing.
- **Mobile mapping:** Vehicle/backpack SLAM or wheeled systems for corridors and large interiors; faster, often lower absolute accuracy without control.
- **UAV LiDAR / photogrammetry:** Site topo, stockpiles, roofs, large footprints; combine with ground control for survey-grade results.
- **Scan + total station hybrids:** Scanning total stations (e.g. Trimble SX12) capture sparse/dense scan with survey measurements.

### 1.4 Point clouds, meshes, digital twins, as-built vs design

- **Point cloud:** Dense XYZ (+ intensity/RGB). Formats: LAS/LAZ, E57, proprietary (TZF/TDX, RCP/RCS).
- **Mesh:** Surface triangles from scans/photogrammetry—good for viz, volume, clash; weaker for precise edge stakeout unless derived carefully.
- **As-built:** Measured existing/constructed condition.
- **Design / BIM:** Intended geometry.
- **Digital twin (construction sense):** Living combination of models + reality capture + progress/QA data—not just a pretty 3D model.
- **QA use:** Overlay design vs scan to find misplacements, missing embeds, slab flatness, panel plumb, etc.

### 1.5 Field layout / stakeout from model

Common field tools:

1. **Robotic total station (RTS)** — primary for building layout (mm-level at typical range).
2. **GNSS RTK / PPK** — open-sky civil, grading, rough corners; cm-level when conditions are good.
3. **AR layout** — visualize design at 1:1 and/or guide marks (phone/tablet + positioning; accuracy depends on positioning stack).
4. **Optical levels / rotary lasers** — elevation and level lines.
5. **String / batter boards / tape** — still used for small forms and checks.

**Stakeout flow:** Office extracts points/lines from model → field software drives instrument → crew marks nails/paint/flags → verify with check shots / independent setup.

### 1.6 Data formats (what you will see)

| Format | Typical use |
| --- | --- |
| **IFC** | Open BIM exchange; IFC4+ can carry CRS via `IfcProjectedCRS` / `IfcMapConversion`. |
| **RVT / Navisworks (NWC/NWD)** | Autodesk design / federation / clash. |
| **DXF / DWG** | 2D/3D CAD planimetrics, grids, details. |
| **LandXML** | Alignments, surfaces, parcels—civil exchange. |
| **LAS / LAZ** | Point clouds (LAZ = compressed LAS). |
| **E57** | Vendor-neutral scan containers (often TLS). |
| **CSV / point files** | Stakeout lists: Point ID, N, E, Z, description. |
| **GeoJSON / shapefile** | GIS features (parcels, utilities)—less common for millimeter layout. |
| **TrimBIM / proprietary** | Trimble ecosystem model packages via Connect / Tekla workflows. |

### 1.7 BIM coordinates ↔ georeferencing (Revit-oriented, widely applicable)

Autodesk Revit concepts (also useful mental model for other BIM):

- **Internal Origin:** Fixed; keep geometry reasonably close (large coords hurt precision/viewers).
- **Project Base Point (PBP):** Origin of the project coordinate system—local project zero for measurements.
- **Survey Point:** Real-world reference near the site (property corner, control point); origin of the survey coordinate system.
- **Shared Coordinates:** The transform that aligns linked models/CAD to a common real-world/site system (acquire/publish coordinates between files).

**IFC georeferencing (buildingSMART IFC4.x):** Project global positioning stores map conversion (eastings, northings, height, rotation/scale) and projected CRS metadata so federated models can sit on Earth without baking huge coords into every solid.

**Contractor takeaway:** Ask “What is 0,0,0?”, “What CRS/datum?”, “Where is control CP1?”, and “Is elevation NAVD88 or site datum?” before trusting any layout file.

---

## 2. Survey techniques and data

### 2.1 Control surveys, benchmarks, traverse, leveling

- **Control survey:** Establishes durable points with known coordinates/elevations for the project.
- **Benchmark:** Vertical control point with known elevation.
- **Traverse:** Connected angle/distance path between control points; closed traverses include misclosure checks.
- **Leveling:** Differential leveling (optical/digital level) for elevations; often more reliable vertically than GNSS alone.
- **Resection / free station:** Total station set up by measuring to ≥2–3 known control points (more is better).

**Field rule:** Never stake critical structure from a single unverified control point. Check into a second point or known baseline.

### 2.2 GNSS / RTK / PPK basics for construction

- **GNSS:** Multi-constellation satellite positioning (GPS, GLONASS, Galileo, BeiDou, etc.).
- **RTK:** Real-time kinematic corrections from base or network (VRS/NTRIP) → typically ~1–3 cm horizontal in open sky when healthy; vertical usually worse.
- **PPK:** Post-processed kinematic—log raw data, process later; useful when radio/cellular RTK is unreliable.
- **Tilt compensation:** IMU-enabled poles allow tipped measurements; still needs good satellite geometry.
- **Site localization:** Match GNSS WGS/ITRF positions to local N/E control so the rover speaks “site feet.”

**Limits:** Multipath near buildings, cranes, walls; canopy; GNSS-denied interiors. For tight building layout, prefer total station.

### 2.3 Total station / robotic total station workflows

- Measure **angles + distance** to a prism (or reflectorless to surfaces).
- **Robotic:** One-person operation; instrument tracks prism; controller lays out points from a list/model.
- **Setup:** Occupy known point + backsight, or resection to control.
- **Typical building use:** Grids, wall lines, embeds, anchor bolts, column centers, slab edges, panel marks.

**Order-of-magnitude field performance (planning figures, not guarantees):** RTS often ~2–3 mm at typical building ranges when control and setup are good; GNSS RTK often ~1–3 cm open sky. Always defer to project specs and the surveyor of record.

### 2.4 As-built surveys, staking, foundations/walls

- **Construction staking / layout:** Place design in the ground (corners, offsets, batter boards, dig lines).
- **As-built survey:** Measure what was built (footing corners, bolt groups, top of wall, utilities).
- **Foundation workflow:** Control → excavate to grade → stake footing corners/offsets → pour → as-built bolts/embeds → walls/panels.
- **Offset staking:** Mark points offset from final face (e.g. 2' outside dig line) so excavators/forms don’t destroy the mark.

### 2.5 Accuracy / tolerance expectations (typical—verify contract)

| Work type | Rough expectation |
| --- | --- |
| Civil grading / earthwork | Often centimeter-class GNSS OK |
| Building grid / wall layout | Millimeter-to-few-mm with RTS |
| Prefab / embeds / bolts | Tight; follow ACI / AISC / project notes |
| Anchor rods | Industry conflict historically: ACI concrete tolerances vs AISC steel—coordinate early; oversized base-plate holes are common mitigation |

**ACI 117** family covers concrete construction tolerances. **AISC Code of Standard Practice** covers steel erection expectations. They do not always match—read the project specs.

**ASCE/SEI 37** addresses design loads during construction (relevant to temporary bracing, not layout accuracy per se).

### 2.6 Common deliverables (high level)

- **Control report:** Point list, datum/CRS, methods, residuals/misclosures, monument descriptions.
- **Point / stakeout files:** CSV or field-software packs (ID, N, E, Z, code).
- **Topo / surface:** Contours, TIN/LandXML surface for grading.
- **As-built drawings / point clouds:** Redlines, scanned conditions.
- **ALTA/NSPS Land Title Survey:** Boundary/title-focused survey for real-estate/lender needs (minimum standards; **2026 ALTA/NSPS standards effective 2026-02-23**). Optional Table A items add detail. Not a substitute for construction layout control unless the project explicitly uses it that way.

---

## 3. Trimble software and hardware (product families as of research)

> **Flag — time-sensitive:** Trimble SKUs, firmware, and subscription tiers change. Names below are from Trimble public product pages / help portals circa 2025–2026. Do **not** invent APIs or claim unsupported instrument pairings; check current FieldLink / Access supported-hardware matrices.

### 3.1 Hardware (common construction / geospatial families)

**GNSS**

- **Trimble R-series** (e.g. R780, R12i, R980 class)—survey GNSS smart antennas; tilt IMU on newer units.
- **SPS-series** (e.g. SPS986)—site positioning / construction GNSS.
- **Catalyst DA2**—subscription GNSS receiver used with apps (FieldLink, SiteVision).
- **R750** modular GNSS (building construction site survey/layout messaging on Trimble BC pages).
- **HPS2**—handheld phone + GNSS/EDM style accessory (SiteVision ecosystem).

**Total stations**

- **S-series** (S5/S7/S9 and earlier S3/S6/S8)—geospatial/survey robotic/mechanical family (Access).
- **RTS series**—construction layout robotic total stations.
- **Ri**—construction-oriented robotic total station marketed with FieldLink.
- **SX10 / SX12**—scanning total stations (survey + scan).
- **SPS series total stations**—construction site instruments.

**Scanners / tablets / targets**

- **X9 / X12**—terrestrial laser scanners; Perspective field software; export LAS, E57, RCP, etc.
- **X7**—earlier scanner still referenced in FieldLink hardware matrices (verify support for a given FieldLink version).
- Controllers/tablets: **TSC5**, **TDC6**, **T10x**, etc. (Access / Perspective / field apps).
- **ST30 smart target**—building construction layout accessory family.

### 3.2 Software / cloud

| Product | Role |
| --- | --- |
| **Trimble FieldLink** | Construction field layout + scanning; drives RTS/GNSS/scanners; model-to-field. |
| **FieldLink Office** | Prep/analyze layout data for FieldLink. |
| **Trimble Field Points** | Create layout points from AutoCAD / Revit / SketchUp. |
| **Trimble Access** | Survey field software for total stations + GNSS (geospatial/construction survey). |
| **Trimble Business Center (TBC)** | Field-to-finish survey CAD; prep surfaces/alignments; connected construction data exchange. |
| **Trimble Connect** | Cloud CDE / collaboration; model sharing; SiteVision project integration. |
| **Trimble SiteVision** | AR visualization + reality capture on phone/tablet; GNSS (Catalyst) for outdoor cm-class positioning; lidar capture workflows; Connect integration. Core vs Pro tiers (Pro adds GNSS outdoor positioning per Trimble BC pages). |
| **Trimble Reality Capture** | Platform/service for reality data collaboration; streaming into Revit via plug-in mentioned on Trimble scan pages. |
| **Trimble RealWorks** | Scan processing / 3D deliverables. |
| **Trimble Earthworks / Siteworks** | Machine control / site positioning for earthmoving (civil); related but distinct from building AR layout. |
| **Tekla Structures** | Structural BIM (Trimble); shares via Trimble Connect / TrimBIM / IFC. |
| **SketchUp** | Conceptual/modeling; Trimble Scan Essentials for working with scans; Field Points support. |

### 3.3 Typical construction workflow (Trimble-shaped)

1. **Design** — Revit / Tekla / Civil 3D / IFC models; establish shared coordinates / site calibration basis.
2. **Office prep** — Extract layout points/lines (Field Points / FieldLink Office / TBC); publish to Connect; generate stakeout lists.
3. **Field stakeout** — FieldLink + Ri/RTS/GNSS lays out footings, grids, embeds; paint/nails/hubs.
4. **As-built / scan** — Collect points or X9/SX scans; SiteVision lidar/AR verification.
5. **Model update / QA** — Compare as-built to design in office tools; issue RFIs; update federated model.

### 3.4 How Trimble AR / SiteVision relates to layout & verification

- **SiteVision** emphasizes **seeing the model at 1:1 on site**, QA/QC, conflict spotting, progress documentation, and georeferenced lidar capture—not necessarily replacing RTS for millimeter stakeout.
- Outdoor positioning often uses **Catalyst DA2** (claimed RTK-class precision on Trimble pages: on the order of **1 cm H / 2 cm V** under good conditions—treat as vendor performance claim, validate on project).
- **FieldLink** is the closer cousin to **precision contractor layout** (robotic stakeout from model).
- LayoutAR-class apps sit in the same problem space as SiteVision/FieldLink: **register digital geometry to physical features**, then guide marks—accuracy depends on the positioning/anchor stack (Vision anchors vs GNSS vs surveyed control).

---

## 4. Basics of tilt-up construction

### 4.1 What tilt-up is

Per **Tilt-Up Concrete Association (TCA)** / **ACI**: cast reinforced concrete wall panels **horizontally on site** (usually on the slab-on-ground or casting beds), then **tilt/lift** them with a crane onto prepared foundations into final vertical position. Recognized as a form of site-cast precast (ACI 318 / IBC framing).

Primary guide: **ACI 551.1R-14 Guide to Tilt-Up Concrete Construction**.

### 4.2 Typical sequence (construction-site oriented)

Exact order varies by job; common pattern (Dayton Superior / ACI-aligned industry practice):

1. Site prep; underslab utilities
2. Interior column footings (as required)
3. Cast/cure floor slab (often also the casting bed)
4. Form/cast/cure exterior footings / foundations
5. Form panels on slab/beds: rebar, embeds, reveals, lifting & bracing inserts
6. Pour/cure panels to lift strength
7. Crane erect panels; temporary bracing before releasing rigging
8. Roof / diaphragm / permanent connections
9. Closure / pour strip between slab and panels
10. Remove braces; finishes

### 4.3 Elements that drive layout accuracy

- **Footings / continuous or spread foundations:** Panel sits on prepared foundation elevation; wrong offset = panels won’t line up or seats fail.
- **Panel casting layout:** Where each panel is formed relative to final location and crane access (side-by-side, stack casting, temporary beds).
- **Panel joints / edges:** Consecutive panels form the wall line; joint gaps and edge plumb matter for openings and connections.
- **Embeds / weld plates:** Structural connections to roof, floors, adjacent panels.
- **Reveals / architectural formers:** Appearance and sometimes weather details—layout relative to panel edges.
- **Lifting inserts:** Engineered locations for crane hardware—misplacement is a safety issue.
- **Bracing inserts / brace anchors:** Temporary wind bracing to slab or deadmen until diaphragm is complete (TCA bracing guidance; ASCE/SEI 37 construction wind concepts).

### 4.4 Why survey / layout accuracy matters

- Panel **length/width/openings** are formed on the slab—errors compound when erecting a long wall line.
- **Footing offsets** and seat elevations must match panel geometry and design.
- **Embeds and inserts** must land where structural/crane engineering assumes.
- Poor layout → rework, crane delays, connection fights, waterproofing/joint failures.

### 4.5 Connection to LayoutAR-style work

Tilt-up and LayoutAR share the same geometric grammar:

| Physical task | Geometric primitive |
| --- | --- |
| Nail / hub / paint mark | Point |
| Chalk line between marks | Line / edge |
| Midpoint of wall or footing | Midpoint |
| Dig / pour footing | Rectangle / polygon with offsets |
| Panel edge / joint line | Line + length + bearing |
| Embed / insert | Point with offset from edge |

**LayoutAR job:** Anchor those primitives to **real-world features** (existing slab corner, control nail, form face), convert **decimal feet ↔ meters (×0.3048)**, and present them in **RealityKit** so the crew can mark dig lines and panel edges without losing the survey intent.

---

## Key vocabulary

- **CRS / EPSG** — Coordinate reference system identifier.
- **Easting / Northing** — Projected map coordinates (X/Y analogs).
- **Project Base Point / Survey Point / Shared Coordinates** — Revit (and analogous) georef triad.
- **Control / monument / hub / nail** — Physical survey marks.
- **Benchmark** — Elevation reference.
- **Stakeout / layout** — Placing design points on site.
- **As-built** — Measured constructed condition.
- **RTK / PPK** — Real-time vs post-processed GNSS precision modes.
- **Total station / RTS** — Angle+distance instrument; robotic variant.
- **TLS / LiDAR / photogrammetry** — Reality capture methods.
- **Point cloud / mesh** — Dense XYZ vs triangulated surface.
- **IFC / LandXML / LAS/LAZ / E57** — Common exchange formats.
- **Site calibration / localization** — GNSS ↔ local site transform.
- **Tilt-up panel / casting bed / embed / lifting insert / brace** — Site-cast wall system terms.
- **Closure / pour strip** — Slab strip left out until panels are set.
- **Decimal foot** — Survey/construction linear unit; ×0.3048 → meters (international foot).
- **RealityKit** — Apple AR framework used to render/anchor geometry on device.
- **LayoutAR** — Class of AR apps that register construction geometry to physical features for field marking.

---

## How this helps LayoutAR

1. **Anchor choice:** Prefer surveyed control nails, slab corners, or known footing corners as AR world anchors—same idea as total-station resection to control.
2. **Units:** Ingest design in decimal feet; convert with **×0.3048** for RealityKit meters; never mix survey-foot and international-foot silently.
3. **Primitives:** Implement nails → lines → midpoints → rectangles (dig/pour) → panel edges; that matches how crews actually mark tilt-up and foundation work.
4. **Accuracy honesty:** AR visualization (SiteVision-like) ≠ RTS millimeter layout. Communicate expected tolerance; use AR for guidance/verification, surveyed marks for critical embeds when specs demand mm.
5. **Coordinate story:** Know whether incoming points are local site grid, State Plane, or model-relative; apply the same shared-coordinates discipline BIM uses.
6. **Tilt-up workflows:** Support footing rectangles with offsets, panel edge strings, and insert points relative to edges—core Brennan LayoutAR construction scenarios.
7. **Trimble adjacency:** Speak FieldLink (layout), SiteVision (AR viz/capture), Connect (models), TBC (survey CAD) correctly when Brennan compares or imports data—without inventing APIs.
8. **As-built loop:** After pour/erect, compare marks or scans to design; feed discrepancies back as updated points—same digital-twin QA pattern.

---

## Uncertainty / time-sensitive flags

- Exact Trimble firmware matrices (FieldLink 2026.x instrument lists), SiteVision Core vs Pro feature packs, and Catalyst accuracy claims change—**re-check Trimble docs**.
- Project tolerances always override this brief’s typical mm/cm figures.
- U.S. survey foot vs international foot: default **0.3048**; ask if legacy control uses survey foot.
- ALTA/NSPS **2026** standards (effective **2026-02-23**) are title-survey focused—not construction stakeout specs.
- NIST does not publish a single “BIM layout accuracy” standard covering all of the above; rely on project specs + ACI/AISC/ASCE as applicable.

---

## References (primary / reputable)

1. https://tilt-up.org/construction/basics/ — TCA tilt-up basics  
2. https://www.concrete.org/store/productdetail.aspx?ItemID=551114 — ACI 551.1R-14 tilt-up guide  
3. https://standards.buildingsmart.org/IFC/RELEASE/IFC4_3/HTML/concepts/Project_Context/Project_Global_Positioning/content.html — IFC4.3 project global positioning  
4. https://help.autodesk.com/cloudhelp/2026/ENU/Revit-Model/files/GUID-68611F67-ED48-4659-9C3B-59C5024CE5F2.htm — Revit project base point & survey point  
5. https://help.autodesk.com/cloudhelp/2025/ENU/Revit-Collaborate/files/GUID-049BE99D-249F-4D1F-A79C-A348955AB49C.htm — Revit shared positioning  
6. https://www.trimble.com/en/products/building-construction-field-systems — Trimble building construction field systems hub  
7. https://www.trimble.com/en/products/building-construction-field-systems/fieldlink-software — Trimble FieldLink  
8. https://geospatial.trimble.com/en/products/software/trimble-sitevision — Trimble SiteVision  
9. https://geospatial.trimble.com/en/products/hardware/laser-scanning — Trimble X9/X12/SX12 scanning  
10. https://help.fieldsystems.trimble.com/sitevision/en/release-notes.htm — SiteVision release notes (version currency)  
11. https://help.fieldsystems.trimble.com/trimble-access/2025.10/en/equipment-supported.htm — Trimble Access supported equipment  
12. https://www.alta.org/topics/topic-land-survey-standards — ALTA/NSPS land title survey standards  
13. https://www.nist.gov/pml/us-surveyfoot — NIST U.S. survey foot (legacy unit context)  
14. https://ascconline.org/Portals/ASCC/Files/Position%20Statements/PS-14_AnchorBoltTolerances_09-11_Web_SC.pdf — ASCC on ACI vs AISC anchor bolt tolerance conflict  
15. https://www.daytonsuperior.com/docs/default-source/handbooks/tilt-up-handbook.pdf — Dayton Superior tilt-up handbook (industry sequence/practice)

---

*End of brief. For durable one-liners, see `geospatial-bim-tiltup-memory-bullets.md`.*
