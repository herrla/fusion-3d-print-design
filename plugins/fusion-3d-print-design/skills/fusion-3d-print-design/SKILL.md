---
name: "fusion-3d-print-design"
description: "Use when designing FDM 3D-printed parts in Fusion: Fusion MCP check and setup, questions to ask first, Voron-style design rules, materials, tolerances, heat-set inserts, parametric modeling and checks."
---

# Designing 3D-printed parts in Fusion

Goal: parts that fit on the first print, print reliably and need as little post-processing as possible. The rules follow Voron community design principles plus values that have proven themselves in practice. Anything the user specifies for a project overrides these defaults.

These rules are for Claude. The user usually has 3D-printing experience and an own printer. Work with them as a peer: apply the rules silently, explain only decisions and trade-offs specific to the part, ask concrete questions, and adopt the user's own values (tolerances, materials, settings) without debate.

## 1. Workflow

1. **Check the Fusion MCP connection** (section 2).
2. **Clarify what is missing** (section 3).
3. **Propose a concept** for anything beyond a trivial part: shape, print orientation, fastening, and where support would be needed. Model after agreement.
4. **Model parametrically** (section 11).
5. **Check** (section 12).
6. **Hand over** with short print notes (section 13).

## 2. Fusion MCP connection

Before modeling, check whether Fusion MCP tools are available (tool names contain `fusion`, e.g. a script/execute tool and a read tool; if tools are deferred, search for them first). Confirm the connection with one read-only call, e.g. listing the open documents.

If the tools are missing or the call fails, help the user set it up:

1. Fusion must be installed, up to date and running; the MCP server only runs while Fusion is open.
2. In Fusion: **Preferences > General > API**, enable **Fusion MCP Server (runs locally on this device)**. Default port is 27182.
3. In the Claude desktop app: **Settings (or Customize) > Connectors > Add/Browse connectors**, search for **Autodesk Fusion**, install it, enable it, and check that the port matches Fusion's. Do not confuse it with the cloud-based *Fusion Data MCP Server*.
4. Start a new chat (or reload the tools) and test with a simple request.
5. In a cloud session, the Fusion connector runs on the user's computer: the session must be linked to that computer through the Claude desktop app, with Fusion and the app running there.

Setup reference: [Connecting to the Autodesk Fusion MCP Server](https://help.autodesk.com/cloudhelp/ENU/ADSKMCP-FusionDesktopMcp/files/ADSKMCP_FusionDesktopMcp_connecting_to_the_fusion_mcp_server_html.html). If the connection cannot be established, continue with step-by-step modeling instructions and all parameters, so the user can build the part by hand.

## 3. Questions to ask first

Ask only what is not already known from the conversation or the project, in one batch:

- **Material** (section 5). There is no default material.
- **Printer:** nozzle diameter, layer height, build volume. If not given, assume 0.4 mm nozzle, 0.2 mm layer height and a 250 x 250 x 250 mm build volume, and say so.
- **Mating parts:** measured dimensions of everything the part touches (profiles, PCBs, connectors, screws, inserts, magnets). Use measured values, not catalog values.
- **Fastening:** screws, heat-set inserts, nuts, snap fits, glue, and which hardware is on hand.
- **Environment and load:** temperature, outdoor use, mechanical load, how often it is assembled.
- **Visible faces:** they influence orientation and support placement.

## 4. Print defaults (unless stated otherwise)

- 0.4 mm nozzle, 0.2 mm layer height, 0.4-0.45 mm line width
- 4 perimeters, 5 top and bottom layers, 40 % infill (Voron default)
- Heights of steps, bridges and floors as multiples of the layer height

## 5. Material

Common materials: PETG, ABS/ASA, PLA, TPU, occasionally Nylon. If the material is not stated, ask.

| Material | Design implications |
|---|---|
| PLA | Stiff and accurate, brittle, softens from about 55 °C. Short snap fits with little engagement, long spring arms (length to thickness 8:1 or more). Not for warm environments. |
| PETG | Tough, slightly flexible, up to about 70 °C. Snap fits length to thickness 5:1 to 8:1. Bridges sag more: keep them short. |
| ABS / ASA | Up to about 90 °C, shrinks and warps. Avoid large flat areas and sharp inside corners, round corners. Allow for shrinkage on fits. ASA for outdoor use. |
| TPU | Walls 1.2 mm or more, no fine snap features, tight fits. Avoid bridges and overhangs. No heat-set inserts. |
| Nylon | Tough and wear resistant, dimensions change with moisture. Slightly looser fits, warps similar to ABS. |

## 6. Orientation, strength and support

- Choose the print orientation before modeling and state it.
- Tension and bending should act within the layer plane. Spring elements (snap arms, clips, hooks) bend parallel to the layers.
- Vertical holes (along Z) for accurate fits; horizontal holes need a teardrop shape.
- **Support material:** avoid it where reasonably possible, but use it when the part needs it.
  - First try to design it away: 45° chamfer instead of a flat overhang, teardrop hole, splitting or rotating the part.
  - If support remains, keep it on non-visible, non-functional faces; never in holes, threads, insert pockets, snap features or sliding surfaces; keep it reachable for removal.
  - Built-in sacrificial ribs designed into the part are often cleaner than slicer supports.
  - State where support is needed and why.

## 7. Geometry rules (Voron style)

- **Overhangs** up to 45° without support; steeper ones per section 6.
- **Edges on the build plate:** 0.4 mm chamfer, no fillet.
- **Vertical edges:** fillets are fine; touched edges R 0.5-1.0.
- **Horizontal holes:** teardrop or diamond shape, point up.
- **Bridges** short and straight (up to about 10 mm is safe). Overhanging ring surfaces (e.g. a counterbore floor facing the bed) get sacrificial layers:
  - Layer 1: slot, width = hole diameter, length = counterbore diameter, depth 1 layer height
  - Layer 2: square, side = hole diameter, depth 2 layer heights
- **Wall thickness** in multiples of the line width: minimum 1.2 mm, load-bearing 1.6 mm or more, clamping spring walls about 3 mm or more.
- **No features** below 2 line widths; pins 3 mm diameter or more.
- **Lead-in chamfers** of 0.4 mm on everything that is inserted.
- **Text:** at least 0.6 mm deep/high, stroke width 0.8 mm or more, preferably on top faces.
- **Large parts:** split to fit the build volume; join with screws, alignment pins or dovetails.

## 8. Holes, fits, hardware

| Purpose | Dimension |
|---|---|
| M3 clearance hole | 3.4 mm diameter |
| M5 clearance hole | 5.4 mm diameter |
| M3 socket head (ISO 4762), recessed | Counterbore 6.2 mm diameter, depth = head height (3 mm) plus margin |
| Heat-set insert M3 x 5 x 4 | **Hole 4.6 mm diameter**, depth at least 5 mm, **0.4 mm chamfer on the top edge** |
| Wall around a heat-set insert | at least 1.6 mm, preferably 2 mm |
| M3 hex nut trap | 5.8 mm across flats, depth 2.6 mm; bridge the hole above it with sacrificial layers |
| Screw threaded directly into plastic | hole 0.1 mm smaller than nominal; occasional assembly only |
| Printed threads | M8 and larger only; otherwise inserts or nuts |
| Press fit (bearing, pin) | pocket +0.1 to +0.2 mm |
| Sliding or plug fit | +0.3 to +0.4 mm on the diameter |
| Clearance for snap-on lids and lips | 0.2-0.25 mm per side |
| Clamping slot (e.g. for profiles) | clear width = measured width + 0.05 mm per side, wall 3 mm or more |

Preferred screw lengths: M3 x 6, 8, 10, 12, 16, 20, 25, 30; as few different lengths per project as possible. For critical fits, offer a test print of just the fitting section.

## 9. Snap fits

- Spring arm bends parallel to the layers
- Length to thickness per material (section 5)
- Lead-in angle 25-35° to the insertion direction, retaining face 45°
- Engagement 0.5-1.2 mm, toward the lower end for PLA
- No floor under a spring arm
- Start slightly weak; it is easier to tighten after a test than to fix a broken snap

## 10. Mistakes Claude must avoid

- Nominal dimensions without clearance on mating surfaces
- Catalog dimensions instead of measured ones
- Fillets on bed edges; overhangs over 45° without a support plan
- Walls or features below 2-3 line widths
- Undersized holes, especially horizontal ones
- Snap arms or hooks bending across the layers, or sitting on a floor
- Counterbores and nut traps facing the bed without bridging
- Ignoring the material (PLA in heat, large flat ABS plates, inserts in TPU)
- Parts larger than the build volume, tall thin parts without a stable base
- Small printed threads, tiny text, sharp inside corners under load
- Saving or overwriting the user's document without being asked

## 11. Working in Fusion

- **User parameters** for every dimension that may change. Readable English names with comment and unit (e.g. `wall_thickness`, `insert_hole_diameter`); derived dimensions as formulas (e.g. `slot_width = profile_width + 2 * profile_clearance`).
- **Fully constrain every sketch** and bind dimensions to parameters. Bracket expressions: `( a - 2 * b ) / 2`, never `a - 2 * b / 2`.
- **Repeated geometry:** draw once, then pattern in the sketch or as a feature pattern.
- **Components:** e.g. assembly > base, lid, placeholders. Model purchased parts as simple placeholder components for interference checks.
- **Feature order:** base body, cuts, added features, holes, then fillets and chamfers last.
- **Name features and sketches** (e.g. `insert_hole`, `lip_cutout`).
- **Never save the document** unless the user asks.

### Fusion MCP scripts (lessons learned)

- The API works in cm. Read values with `unitsManager.evaluateExpression(expr, 'mm')`; set dimensions as expression strings.
- Always set `participantBodies` on cut and join extrusions.
- `addScribedPolygon` creates its own center point: make it coincident with the sketch origin.
- Constrain axis-aligned lines as horizontal/vertical before dimensioning, and dimension only one coordinate per aligned chain, or Fusion reports over-constraint.
- Fillets and chamfers often fail at edge ends (`ASM_BL_...`): split the edge set, use `isTangentChain = False`, skip edges shorter than 1 mm.
- After inserting a feature earlier in the timeline, check fillets and chamfers for lost edge references.
- A failed script rolls back its changes; build in steps (parameters, one part, finishing).
- Do not catch exceptions. Use `readOnly: true` for inspection-only scripts.
- Verify by reading the model back (body count, volume, bounding box) and with a screenshot.

## 12. Checks before handover

- [ ] Timeline without warnings, all sketches fully constrained
- [ ] No interference between parts and placeholders (boolean intersection = 0)
- [ ] Clearance and rotational play of moving or plugged parts checked
- [ ] No unplanned overhangs over 45°, bed edges chamfered, counterbores and nut traps bridged
- [ ] Heat-set inserts: 4.6 mm hole, 0.4 mm chamfer, depth 5 mm or more, wall 1.6 mm or more
- [ ] Material considered, part fits the build volume
- [ ] One body per part, appearances set
- [ ] Screenshot taken
- [ ] Anything that could not be checked is stated

## 13. Print notes in the handover

Keep them short, no basics:

- Material and print orientation
- Support: none, or where and why
- Hardware with sizes and counts
- Fits that may need a test print
- Parameters most likely to be adjusted
