# Dinotigermon Fang Skills — V4 effects test build

V4 adds native wind and quake effect references to the accepted V3 motions.
It also adds a short, fading camera shake to Highland Fang's landing.

| Attack | Visual addition |
| --- | --- |
| Ground Fang | Native Wind Claw hit sequence (`30191`, `ef_b_gwi_s02`) |
| Highland Fang | Native Awesome Quake area sequence (`30182`, `ef_b_gro_a01`) and earth/rock hit (`12010`, `ef_b_hit_170_rock`) |
| Highland landing | Camera shake starts at frame 43, peaks around contact at 46, and ends at 62; zero-based frames at the source 30 fps |

The quake shake is present in both player and enemy camera clips. Its maximum
camera displacement is about 0.05 model units. Camera aim, field of view,
length and entry/exit poses are retained.

V3's actor clips, lively limbs, biting mouth and repaired jaw geometry are
byte-identical. Damage element remains Null for both skills. Power, SP cost,
hit count, targeting, skill text and all other gameplay fields are unchanged.
The effects use assets already in the base game; no global effect definition is
replaced and no other Digimon's skill is edited.

## Install

Replace the existing Dinotigermon mod folder with this ZIP's contents so
ModConfig.json and dsts-loader sit together. Keep the Reloaded-II/DSTS.ModLoader
dependencies enabled and use only one Dinotigermon copy. V3 remains available
separately for rollback.

## Timing and testing

This is an offline-validated, in-game-untested effects build. The effect slots
and asset names match native game tables and existing slot/type conventions.
Native particles cannot be rendered or inspected in the supplied Blender tools.
The camera GIF shows the exported camera and V3 actor, without staged particles.

The existing hit setting remains `1` (after the character animation). Ground's
wind hit therefore follows that engine hit event; it is not verified to occur
during the mid-clip bite. Awesome Quake's area playback is also engine-driven.
Only the camera shake is explicitly timed to the landing. An in-game check is
needed for particle placement, duration, scale and synchronization. The native
Wind Claw and Awesome Quake use other, undocumented hit-timing modes; V4 does
not guess their meanings or change the accepted combat timing.

Perfectly matching the wind/quake particles to arbitrary animation frames would
need the native effect bundles or a verified runtime event scheduler. The
original upload does not include those vanilla effect bundles. Enemy restraint,
terrain deformation and dismemberment are not implemented by this change.

## Validation and rebuild

The source bundle includes the V3 baseline report, native row excerpts and their
upstream CSV hashes, authored offsets, and V4 validation. Checks restrict the
change to three files: the two custom skill rows and Highland's two cameras.
They verify native effect references, unchanged gameplay/actor assets, bounded
shake, preserved camera channels and duration, and complete output hashes.

Use Python 3, numpy, and the previously inspected Romsstar feature/refactor
codec at commit dfcef08a33f67abd5b96f0f121902c27060b7c57. Set DSCS_TOOLS_ROOT
to that checkout. Extract V3's game files into modified_v3, leaving its bundled
README outside the asset folder. Start without a modified_v4 folder. Run from
the V4 source bundle root:

```sh
python3 scripts/add_effects_v4.py --baseline-validation baseline_v3_validation.json
python3 scripts/validate_mod_v4.py --baseline-report baseline_v3_validation.json
python3 scripts/package_outputs_v4.py
```

The included work/v4/native_effect_references.json supplies the reviewed native
rows, so rebuilding the mod does not need the full toolkit checkout. Optional
camera rendering uses Blender 4.3.2 and the unchanged V3 editable scene at
outputs_v3/Dinotigermon_Fang_Attacks_V3.blend:

```sh
blender --background --python-exit-code 1 --python scripts/render_camera_v4.py
python3 scripts/package_outputs_v4.py
```

Source tables: https://github.com/zeak6464/DigimonTimeStrangerToolKit/tree/f633513a7d86bd70d82f2766ce01157ec91285af/Base

Skill-field documentation: https://hyotan-dev.github.io/dstsworkshop/documentation/mbe/battle_skill/
