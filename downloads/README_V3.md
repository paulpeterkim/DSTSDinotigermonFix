# Dinotigermon Fang Skills — V3 test build

V3 adds more independent limb movement and repairs the model's jaw binding so
the mouth visibly opens before the bite and snaps closed during contact.
It builds on the reviewed V2 choreography for dacare's Dinotigermon v1.11.0 mod.

## Changes

- Shoulders and hips load, push and recoil independently. Airborne legs reach
  and tuck at different times, with articulated ankles and toes.
- Takeoff and landing use continuous joint bend directions and blended paw
  targets to avoid abrupt changes between airborne and planted poses.
- The lower jaw was previously bound to the head, so jaw animation could not
  move the mouth. V3 transfers selected lower-mandible skin weights to the
  existing jaw bone in all three meshes. Upper fangs, upper teeth and cheek
  plates remain attached to the head.
- The mouth opens to 30 degrees and closes to -6 degrees in the jaw's local
  coordinates. A separate head-fixed preview demonstrates actual mesh motion.

Ground Fang retains its approach, braced pin, fang stab and tearing follow-through.
Its forepaw targets stay fixed during frames 33–70. Highland Fang retains its
elevated coil and fast fang-led downswing, extending the forepaws afterward to
catch the landing. Ground runs for 120 frames and Highland for 96 at the source
approximately 30 fps. Both camera pairs match those lengths. Both attacks
retain the approximately 4.14-unit approach and finish at close range.

Skill names, descriptions, targeting and combat parameters are unchanged from
V2: Ground targets one enemy; Highland targets all enemies. Both retain physical
Null damage, power 55, SP 300 and one hit. Textures, mesh topology, vertex
positions, materials, skeleton and unrelated animation files are preserved.
Only existing vertex skin indices/weights are changed in the model file.

## Install

Back up the existing mod, then extract the V3 test ZIP in its place so
ModConfig.json and dsts-loader sit together at the mod root. Keep the existing
Reloaded-II/DSTS.ModLoader dependencies enabled. Enable one Dinotigermon copy.
V1 and V2 remain available separately for rollback.

## Validation and limits

The exported binaries are checked for valid channels, finite normalized
rotations, timing, camera lengths, planted-paw accuracy after compression and
neutral start/end articulation. Geometry checks restrict changes to the intended
skin fields and verify preserved upper fangs. Blender evaluates the actual
patched geometry for the jaw proof and the full attack previews. Archive CRC and
packed-asset hashes are verified before publication.

This build has not been tested in the game. Damage/reaction timing, combat
spacing, camera framing and return to formation still need a runtime check.
The existing engine hit setting triggers after the clip. These files animate
Dinotigermon and its cameras; they contain no synchronized victim track or
paired-event definition. Enemy restraint, dismemberment and instant-kill logic
are not implemented. The previews show the attacker motion in Blender with a
preview-only floor grid and approximate base-texture materials.

## Review and rebuild

The editable scene contains actions chr424857_bs01 and chr424857_bs02. Blender
frames begin at 1; the authoring reports use zero-based frames. Editing the
scene alone does not export the game clips. Edit author_animations_v3.py and
regenerate them with the inspected binary writer.

The source ZIP includes scripts, choreography and validation/clearance/jaw
reports. Use Python 3 with numpy and Pillow, plus Blender 4.3.2. Set
DSCS_TOOLS_ROOT to a checkout of Romsstar/Blender-Tools-for-DSCS,
feature/refactor, commit dfcef08a33f67abd5b96f0f121902c27060b7c57.
That branch's Blender animation export operator is disabled; these scripts use
its animation codec directly. GEOM is patched in place without serializing its
opaque material sections.

Extract the untouched upload into original. Extract the V2 test build's game
files into modified_v2: dsts-loader/, ModConfig.json and
Sewer56.Update.Metadata.json. Leave the ZIP's README_Fang_Skills_V2.md outside
that folder so the strict asset inventory matches. From the source bundle's
root, run:

```sh
cp -a modified_v2 modified_v3
python3 scripts/patch_jaw_v3.py
python3 scripts/author_animations_v3.py
blender --background --python-exit-code 1 --python scripts/render_jaw_demo_v3.py
python3 scripts/validate_mod_v3.py --max-adjacent-limb-degrees 60
blender --background --python-exit-code 1 --python scripts/render_preview_v3.py -- --save-only
blender --background --python-exit-code 1 --python scripts/check_clearance_v3.py
blender --background --python-exit-code 1 --python scripts/render_sequence_v3.py -- bs01
blender --background --python-exit-code 1 --python scripts/render_sequence_v3.py -- bs02
cp README_V3.md outputs_v3/
cp work/v3/clearance.json work/v3/jaw_patch_report.json work/v3/jaw_deformation_probe.json work/v3/jaw_demo_report.json outputs_v3/
python3 scripts/package_outputs_v3.py
```

Begin without an existing modified_v3 folder. The scripts leave original and
modified_v2 unchanged. Reports are regenerated under work/v3 and outputs_v3;
absolute paths in the bundled reports refer to the build workspace.

Sources:
https://github.com/Romsstar/Blender-Tools-for-DSCS/tree/feature/refactor
https://hyotan-dev.github.io/dstsworkshop/
