# Dinotigermon 1.13.0 with V8 Fang Skills

This build merges the accepted V8 Ground Fang and Highland Fang into dacare's
uploaded Dinotigermon v1.13.0 update. It preserves the updated base texture,
the two new riding animations, the riding-animation table and the v1.13.0
configuration/update metadata.

Both attack animations, all four cameras, the lower-jaw binding, wind and quake
effects, effect timing and effect sound muting are byte-identical to V8.
The close-ups, left-to-right framing, final jaw shot and active attack endings
are unchanged. Skill names and automatic description IDs (`0`) are retained.

The only change to the V8 skill rows is their voice-reference field (column 6),
which is set to `0` for both custom skills. This follows the native boss versions
of Good Night Moon and Arrow of Artemis, which retain the corresponding bs01/
bs02 animation and main effect while using a zero reference. No global voice
tables or audio banks are replaced, and normal attacks and other Digimon are
unaffected by this edit. Actual voice suppression still needs an in-game check.

## Install

Replace the entire existing Dinotigermon mod folder with this ZIP's contents.
Enable only one Dinotigermon copy and keep the existing Reloaded-II and
DSTS.ModLoader dependencies. A full replacement also removes old custom skill
description files from earlier builds.

## Validation

The new upload contains 117 files. The merge applies the 37-file V8 delta,
producing 144 files. All six upstream additions/changes are preserved exactly.
Every file in the V8 skill overlay is unchanged except the two voice-reference
cells in the skill table. The archive and file inventories are checked against
both the uploaded 7z and the published V8 ZIP. The combat geometry and skeleton
are unchanged in v1.13.0, so V8's validated animations, jaw binding and cameras
remain compatible.

The build has not been run in the game. Voice dispatch, native effects and
combat behavior still require a runtime check. Existing V8 previews show the
same motion and cameras with the older base texture.

## Rebuild

Extract the supplied `dinotigermon1_13_0.7z` into `original_v113`. Extract the
published V8 test ZIP into `modified_v8`, excluding its bundled README. Extract
this build's source ZIP into the working directory. Keep both original archives
for validation. Python 3 is sufficient for merging/packaging; the validator also
uses `py7zr` to independently check the uploaded archive.

Starting without `modified_v8_v113`:

```sh
python3 scripts/merge_v8_v113.py --archive dinotigermon1_13_0.7z
python3 scripts/validate_v8_v113.py --archive dinotigermon1_13_0.7z --v8-zip Dinotigermon_Fang_Skills_V8_TEST.zip
python3 scripts/package_v8_v113.py
```

The source bundle includes the original manifest, V8 validation inventory,
merge/validation reports and the native voice-pattern evidence. It does not
require Blender or re-author the accepted V8 animations.
