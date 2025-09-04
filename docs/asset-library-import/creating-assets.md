# Design, Prepare & Import 3D Assets for Meta Horizon Creator Program

Sahil Mandal (Horizon: SahilMandal)

**Last updated:** August 17, 2025

## Introduction

This document is a complete, competition-ready tutorial that teaches creators how to design, prepare, and import optimized 3D assets into the Desktop Editor for Meta Horizon Worlds. It follows the MHCP Documentation Competition template and reproduces all required submission elements: a clear title, author, purpose, step-by-step actionable instructions, screenshot placeholders, optional video instructions, and a submission checklist that maps directly to the competition evaluation criteria.

Why read this? Quality 3D assets improve immersion and performance in Horizon Worlds. This guide focuses on real-world scale, PBR material setup, export settings, LODs & collision, importing and publishing assets to the Public Asset Library — plus the exact deliverables you must include for the MHCP submission.

### Prerequisites and Expectations

- Basic familiarity with a 3D modeling tool (Blender, Maya, 3ds Max). Examples use Blender 3.x+.
- Basic knowledge of textures and PBR (albedo, normal, roughness, metallic).
- Desktop Editor installed and an active Horizon Worlds Creator account.
- A GitHub account to submit the Markdown file and assets (per MHCP rules).

### Document Organization

This document is split into two main topics:

- [Asset Preparation & Export](#asset-preparation--export)
- [Importing, Publishing & Submission Checklist](#importing-publishing--submission-checklist)

## Asset Preparation & Export

This section explains the step-by-step modeling and texture workflow that produces a Horizon-ready asset.

### 1 — Concept, Blocking & Scale

1. Block out the silhouette using simple primitives. Keep proportions consistent with a \~1.75 m human reference.
2. Set your unit system to **Metric (meters)** in your 3D package to avoid scale issues on import.

**Tip:** Create a `human_reference` mesh scaled to 1.75 m to test asset scale early.

### 2 — Modeling Best Practices

- Keep the mesh low- to mid-poly for props. Suggested tri budgets: small props 500–5k, furniture 5k–25k.
- Use clean topology; avoid hidden interior faces.
- Place the pivot at the logical interaction point (e.g., base or handle).

### 3 — UVs & Texture Planning

- Unwrap with consistent texel density. For multiple small items, use an **atlas** to reduce draw calls.
- Recommended maps: Albedo (Base), Normal, Roughness, Metallic, AO (optional), Emissive (optional).
- Texture sizes: 512/1024 for props, 2048 for hero assets.

### 4 — Baking & Optimization

If you sculpted high-poly details, bake normals and AO to the low-poly mesh. Remove unseen faces and merge static geometry to cut draw calls.

### 5 — LODs & Collision

- Create LOD0 (full), LOD1 (\~50% tris), LOD2 (\~10–25% tris).
- Make a simplified collision mesh named `asset_collision` or follow Desktop Editor naming conventions for collision. Use simple primitive colliders where possible.

### 6 — Export (FBX / GLB)

**FBX (Blender export example):**

```txt
File → Export → FBX (.fbx)
- Apply Transform: scale=1.0
- Forward: -Z Forward, Up: Y Up (match Desktop Editor expectations)
- Apply Modifiers: ON
- Selected Objects: ON
- Path Mode: COPY (embed textures) or AUTO
```

**GLB (preferred if you want a single binary with PBR preserved):**

```
File → Export → glTF (.glb)
- Format: GLB
- Include: Selected Objects
- Export Materials: Metallic-Roughness
```

## Importing, Publishing & Submission Checklist

This section covers the Desktop Editor import flow, publishing assets, and the exact items you must include in your competition submission.

### 1 — Importing into Desktop Editor

1. In Desktop Editor → Assets → Import → choose `.fbx` or `.glb`.
2. Verify scale and orientation using the human_reference.
3. If textures were not embedded, upload them to the asset’s material slots and assign correctly.
4. Assign the collision mesh or enable automatic collision generation.
5. Test physics and interactions in Play/Test mode.

**Screenshot placeholders** (capture these steps):

- `screenshots/01_blockout.png` — Blockout with human reference.
- `screenshots/02_uvs.png` — UV layout in Blender.
- `screenshots/03_export_dialog.png` — FBX/glTF export dialog.
- `screenshots/04_import_editor.png` — Import window in Desktop Editor.
- `screenshots/05_material_assign.png` — Material panel with maps assigned.

### 2 — Publishing to Public Asset Library

1. Select the imported asset → choose **Make Public** or **Publish** in the Desktop Editor.
2. Provide descriptive metadata: title, tags, category, thumbnail.
3. Test that the public asset can be spawned in a new world and behaves as expected.

### 3 — What to submit (matches MHCP requirements)

Include the following in your GitHub submission repository and Markdown file:

- Final Markdown guide (this document) in the competition template.
- At least 8–10 high-quality screenshots (see placeholders above).
- Optional: a short-form video walkthrough uploaded to YouTube (public) — include the URL.
- Example source `.blend` or `.fbx/.glb` (one asset) and a small texture set (avoid very large files).
- Any scripts (Blender export script, material assignment script) used — include `scripts/export_fbx.py`.

### 4 — Mapping to Evaluation Criteria

| Evaluation Criterion    | How this submission meets it                                                                           |
| ----------------------- | ------------------------------------------------------------------------------------------------------ |
| Impact                  | Shows a complete, reusable workflow and publishes to Public Asset Library so others can reuse assets.  |
| Clarity & Completeness  | Step-by-step procedures, export settings, import screenshots, and a testing checklist.                 |
| Engagement & Creativity | Includes optimization tips, LOD, atlas strategies, and a video walkthrough option.                     |
| Screenshot Quality      | Includes a list of required screenshots and recommended framing for each.                              |
| Code Snippets           | Includes FBX/glTF export instructions and an export script placeholder.                                |
| Video Quality           | Guidance to record 1080p+ at 30–60fps, clear audio, and include timestamps in the YouTube description. |

## Topic Two — Quick Reference Tables & Diagrams

### Export Cheat Sheet

| Step           | FBX             | GLB              |
| -------------- | --------------- | ---------------- |
| Embed textures | Path Mode: COPY | Binary GLB (yes) |
| Units          | Metric (meters) | Metric (meters)  |
| PBR support    | Yes             | Yes              |

### Simple flow diagram

```mermaid
graph TD
  A[Model & UV] --> B[Bake & Textures]
  B --> C[Create LODs & Collision]
  C --> D[Export (FBX/GLB)]
  D --> E[Import to Desktop Editor]
  E --> F[Publish to Public Library]
```

## References

- MHCP Documentation Competition Template — use this file as the root for your Markdown formatting.
- Meta Horizon Worlds Custom Model Import — follow the developer docs for the latest format/limitations.

---


---

Screenshot placeholders (capture these steps):

Below are 10 recommended screenshots with short captions, framing tips, and suggested Blender camera transforms or quick viewport controls to capture them quickly. Save each at 1920×1080 PNG and use the filenames below.

screenshots/01_blockout.png — Blockout with human reference.

Caption: "Blockout stage showing silhouette, proportions, and human reference (1.75 m)."

Framing tip: Show entire prop and human reference centered; leave margin around objects.

Blender quick capture: Use the Camera object or viewport.

Viewport: Perspective, Numpad 1 (front) then tilt up by ~20° (drag Middle Mouse).

Camera object (approx): Location: (0.00, -4.00, 1.60) — Rotation (Euler deg): (78.0, 0.0, 0.0).

screenshots/02_uvs.png — UV layout in Blender.

Caption: "UV Editor view showing islands, packing, and texel density guides."

Framing tip: Open UV Editor at left and 3D View on the right with the selected mesh highlighted.

Blender quick capture: Split view; in 3D View select the mesh, then open UV Editor and display UV Islands with Show Overlays on.

screenshots/03_export_dialog.png — FBX / glTF export settings.

Caption: "Export dialog with recommended settings (Apply Modifiers, -Z Forward / Y Up, Path Mode: COPY)."

Framing tip: Capture the full export dialog window; crop to show important toggles.

No camera required — capture the File → Export → FBX/glTF dialog.

screenshots/04_import_editor.png — Desktop Editor import window.

Caption: "Import dialog inside Desktop Editor with scale/orientation checks visible."

Framing tip: Capture the entire editor with the Import modal centered; include the asset in the scene behind the modal if possible.

screenshots/05_material_assign.png — Material panel with maps assigned.

Caption: "Material inspector showing Albedo, Normal, Roughness, Metallic slots populated."

Framing tip: Show Shader/Material panel alongside a small preview of the model in the 3D View.

Blender quick capture: Camera close-up to the model.

Viewport: Position view to focus on material area; use Ctrl+Alt+Numpad0 to align camera to view.

Camera object (approx): Location: (1.50, -2.00, 1.20) — Rotation (Euler deg): (75.0, 0.0, 30.0).

screenshots/06_bake_settings.png — Baking settings (normal/AO) in Blender.

Caption: "Bake panel with normal and ambient occlusion bake settings and selected cage/targets."

Framing tip: Show the Bake settings UI with the low/high poly selection visible in the 3D View.

screenshots/07_lods_sidebyside.png — LOD meshes displayed side-by-side.

Caption: "LOD0, LOD1, and LOD2 meshes arranged side-by-side to compare silhouette and polycount."

Framing tip: Position meshes in a row on the X axis, orthographic view to clearly show silhouette differences.

Blender quick capture:

Viewport: Use Numpad 5 (toggle Orthographic), then Numpad 1 (front) and pan back to fit all LODs.

Camera object (approx): Location: (0.00, -6.00, 1.80) — Rotation (Euler deg): (90.0, 0.0, 0.0).

screenshots/08_collision_mesh.png — Collision mesh (wireframe overlay).

Caption: "Simplified collision mesh overlaid on the visible mesh (wireframe) to show fit and optimization."

Framing tip: Enable Wireframe overlay for the collider and Solid for the visible mesh; use X-ray if needed.

Blender quick capture: In 3D View, enable Viewport Overlays → Wireframe for collider object; use perspective camera aimed at the collision area.

screenshots/09_exported_asset_in_editor.png — Asset placed in Desktop Editor scene.

Caption: "Final asset placed in a world — verify scale, material response to scene lighting, and collision."

Framing tip: Capture player POV (first-person height ~1.7 m) standing next to the asset to show scale.

screenshots/10_publish_metadata.png — Publish dialog and metadata.

Caption: "Publish modal filled with title, description, tags, and thumbnail before making the asset public."

Framing tip: Ensure the thumbnail and text fields are visible and legible; include the preview thumbnail.