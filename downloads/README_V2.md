# Dinotigermon Fang Skills — V2 test build

This revision replaces the first animation pass with a longer, articulated
performance. It is based on dacare's uploaded Dinotigermon v1.11.0 mod.

## Ground Fang

A low approach closes to the original physical attack's combat area, both
forepaws brace in staggered contacts, and the body settles over the hold. The
fangs draw up, stab sharply, briefly hold tension, then wrench sideways and pull
free. The forepaw anchors remain fixed through the pin/stab/tear section.
Length: 120 frames at the original approximately 30 fps (about 4 seconds).

## Highland Fang

The body loads into a crouch, pushes off, curls its limbs during the rise, then
rotates forward into an accelerating fang sweep. Forepaws stay tucked until the
fangs pass through the cutting stroke; they extend afterward to catch the
landing. Length: 96 frames at the original approximately 30 fps (about 3.2 seconds).

## What V2 fixes

- Restores about 4.14 model units of approach, measured from the original
  close-range attack clip. V1 had reduced this to less than one unit.
- Uses world-space paw targets and two-segment joint IK for the planted hold.
  Every solved target is verified again after animation compression.
- Uses flowing interpolation with distinct anticipation, fast strikes, contact
  holds, and follow-through instead of easing to a stop at every key pose.
- Uses body pitch/twist and delayed mane/tail movement during the aerial cut.
- Retimes both player/enemy camera clips to match each longer character clip.
  Contact framing is held steady. Both attacks finish at close range, following
  the original physical clip's approach convention; the game return/handoff
  remains untested.

Names, exact requested English descriptions, combat parameters, model, textures,
skeleton, and unrelated animations remain unchanged from the first edited build.
Ground Fang targets one enemy. Highland Fang keeps the original second skill's
all-enemy scope. Both retain power 55, SP 300, one hit, physical Null.

## Install

Back up the existing mod folder, then extract this ZIP in its place so
ModConfig.json and dsts-loader sit together at the mod root. Enable the existing
Reloaded-II/DSTS.ModLoader dependencies. Enable only one Dinotigermon mod copy.
The first build is preserved separately in the repository for rollback.

## Scope and game testing

These files control Dinotigermon and its cameras. They do not animate or constrain
a victim, add an immobilization status, dismember enemy meshes, or implement an
instant-kill mechanic. The pin is represented by Dinotigermon's braced hold.
The previews show that attacker performance, without a staged enemy.

The upload contains no victim track, and the inspected tools expose no verified
paired-character event editor. A true synchronized grapple needs the missing
base-game chr424LOC locator assets, a known paired-attack/event example, and
runtime testing. Generic enemy damage animations are shared with other attacks;
they were not globally replaced.

No Windows game runtime is available here. Skill hit timing remains the existing
engine setting (column 11 = 1, after the clip). The longer choreography therefore
needs an in-game check for reaction/damage timing, combat spacing, camera framing,
return to formation, and differently sized targets. Structural and Blender
checks do not establish those engine behaviors.

## Review and editing

- Ground_Fang_V2.gif / Highland_Fang_V2.gif: actual baked V2 attacker motions;
  a preview-only floor grid makes travel and paw placement visible.
- Dinotigermon_Fang_Attacks_V2.blend: editable packed-texture Blender scene with
  actions chr424857_bs01 and chr424857_bs02. Frames start at 1 in Blender.
  Materials approximate the base texture, not the original game shaders.
- validation.json: file hashes, typed data/reference checks, timing, endpoint,
  and planted-paw/compression checks.
- clearance.json: every-frame head/fang clearance and fang-versus-paw checks.
  The source already has small floor intersections; this is not a collision solver.
- Dinotigermon_Fang_Skills_V2_Source.zip: authoring/validation/preview scripts.

## Rebuild

Use Python 3 with numpy (Pillow for GIF packaging) and Blender 4.3.2. Extract the
untouched uploaded mod into original, and the first edited build into modified
for the V1 comparison. Neither source folder is rewritten. Begin without a
modified_v2 folder, then run from the script bundle's root:

```
python3 scripts/patch_skills.py --destination modified_v2 --disable-highland-effect
python3 scripts/author_animations_v2.py
python3 scripts/validate_mod_v2.py
blender --background --python-exit-code 1 --python scripts/inspect_blender.py
blender --background --python-exit-code 1 --python scripts/render_preview_v2.py -- --save-only
blender --background --python-exit-code 1 --python scripts/check_clearance_v2.py
blender --background --python-exit-code 1 --python scripts/render_sequence_v2.py -- bs01
blender --background --python-exit-code 1 --python scripts/render_sequence_v2.py -- bs02
python3 scripts/package_outputs_v2.py
```

Set DSCS_TOOLS_ROOT to an inspected checkout of Romsstar/Blender-Tools-for-DSCS,
feature/refactor, commit dfcef08a33f67abd5b96f0f121902c27060b7c57.
The animation writer is used directly; this branch has no enabled Blender
animation export operator. Editing the .blend alone does not rebuild the mod;
edit author_animations_v2.py and regenerate the binary clips.

Sources:
https://github.com/Romsstar/Blender-Tools-for-DSCS/tree/feature/refactor
https://hyotan-dev.github.io/dstsworkshop/
