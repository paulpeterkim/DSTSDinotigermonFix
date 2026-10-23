# Dinotigermon Fang Skills — V6 camera direction, jaw ending and automatic descriptions

V6 makes the three requested changes:

- Ground Fang and Highland Fang now read left-to-right in the player view.
  The already-correct enemy-side action view is retained.
- Ground Fang stays close after the bite and eases into a mouth/jaw portrait,
  holding it through the final frame instead of returning to the feet shot.
  Both Ground camera variants use this ending.
- Both skills' description IDs are set to `0` for game-generated descriptions.
  All eleven localized `skill_explanation.mbe/000_Sheet1.ap.csv` overrides are
  removed. The custom Ground Fang and Highland Fang names are preserved.

The camera uses a proper opposite-side 3D view with normal rotation and field
of view. It does not mirror the rendered image or the character model.
The jaw-ending transition runs from frame 70 to 100, then holds through 119.
The close-ups during the attacks remain in place.

Actor animations, lively limbs, the working biting mouth, model/skin weights,
textures, Wind Claw/Awesome Quake references, Highland's camera shake, damage,
SP cost and targeting are unchanged from V5.

## Install

Replace the entire old Dinotigermon mod folder with V6. This is necessary because
the eleven old description files must be removed; extracting over the old
folder can leave them behind. ModConfig.json and dsts-loader belong together
at the mod root. Keep the existing Reloaded-II/DSTS.ModLoader dependencies and
enable only one Dinotigermon copy. Previous builds remain available for rollback.

## Validation and previews

Seven checks verify the exact V5 baseline, the eleven deletions, description
column `5 = 0`, preserved names/gameplay/assets, valid camera channels/timing,
positive left-to-right attack projection, and the independent Blender audit.
The final mod has 125 game files. Only three camera files and the skill CSV
change; Highland's enemy camera is already correct and remains byte-identical.

Actual Blender projection verifies both attacks' face close-ups and the ending
jaw shot at 16:9. The jaw occupies about 67–68% of the final image height and
stays inside the image with margin. The GIFs use the exported player cameras.
Native game particles are not rendered in Blender.

The files have not been tested in the game. V4's native particle/hit timing is
retained; this edit does not retime effects or implement synchronized victims.

## Rebuild

Use Python 3 with numpy and the inspected Romsstar feature/refactor codec at
commit dfcef08a33f67abd5b96f0f121902c27060b7c57. Set DSCS_TOOLS_ROOT to its
checkout. Extract V5's game files into modified_v5, excluding its bundled
README. Begin without a modified_v6 folder. From the V6 source bundle root:

```sh
python3 scripts/revise_mod_v6.py --baseline-report baseline_v5_validation.json
python3 scripts/validate_mod_v6.py --baseline-report baseline_v5_validation.json
python3 scripts/package_outputs_v6.py
```

The source bundle includes the V5 baseline, authoring helpers, face/jaw
selections, diagnostics and the hash-bound framing audit. An unchanged rebuild
can reuse the audit. After camera edits, rerun the audit with Blender 4.3.2 and
the unchanged V3 actor scene at
outputs_v3/Dinotigermon_Fang_Attacks_V3.blend. Pillow is used for GIF packaging.

```sh
blender --background --python-exit-code 1 --python scripts/audit_camera_v6.py
python3 scripts/validate_mod_v6.py --baseline-report baseline_v5_validation.json
blender --background --python-exit-code 1 --python scripts/render_camera_v6.py -- bs01
blender --background --python-exit-code 1 --python scripts/render_camera_v6.py -- bs02
python3 scripts/package_outputs_v6.py
```

Description-field reference:
https://hyotan-dev.github.io/dstsworkshop/documentation/mbe/battle_skill/
