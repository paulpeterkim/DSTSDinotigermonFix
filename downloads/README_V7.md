# Dinotigermon Fang Skills — V7 active endings

V7 removes the recovery and idle tails from both attacks. The clips now stop
on an active attacking pose instead of settling back to idle.

| Attack | New length | Final pose |
| --- | --- | --- |
| Ground Fang | 68 frames, about 2.27 seconds | Final sideways tear, jaw closed and paws still braced |
| Highland Fang | 48 frames, about 1.60 seconds | Landing strike, forepaws planted and rear legs still following through |

The actor motion is the unchanged V6 prefix: source frames 0–67 for Ground and
0–47 for Highland. Nothing is slowed down, blended to neutral or held at the
end. Both poses are still moving when the clips finish.

All four cameras match the shorter lengths. Ground's jaw close-up is brought
forward to the last tear, using a near-frontal left-side view so the head turn
does not hide the mouth. Both skills retain left-to-right action framing.
The preview GIFs also have no added final-frame pause.

Automatic description IDs remain `0`, custom description files remain removed,
and skill names, effects, damage, SP cost, targeting, textures and jaw binding
are unchanged. Of 125 game files, only two actor clips and four camera clips
change.

## Install

Replace the old Dinotigermon folder with this ZIP's contents. Keep the existing
Reloaded-II/DSTS.ModLoader dependencies and enable only one Dinotigermon copy.
If upgrading from V5 or earlier, replace the entire folder so the old custom
description files are removed. V6 remains available for rollback.

## Validation and game testing

Six checks verify the exact V6 baseline, retained actor poses, active endpoints
without a tail/hold, clip durations and camera lengths, preserved assets, and
independent Blender framing. Both actor prefixes match their source poses
exactly after decoding. The final jaw shot occupies about 65–67% of the image.

The shorter clips have not been tested in the game. The existing hit setting
still means after the character animation, so the engine hit/effect sequence
can now begin sooner. Native particles and the engine's return-to-formation or
idle handoff still require a runtime check. The GIFs show the actual exported
cameras and active actor frames, without staged game particles.

## Rebuild

Use Python 3 with numpy and the inspected Romsstar feature/refactor codec at
commit dfcef08a33f67abd5b96f0f121902c27060b7c57. Set DSCS_TOOLS_ROOT to that
checkout. Extract V6's game files into modified_v6, excluding its bundled
README. Begin without modified_v7. From the V7 source bundle root:

```sh
python3 scripts/trim_attacks_v7.py --baseline-report baseline_v6_validation.json
python3 scripts/validate_mod_v7.py --baseline-report baseline_v6_validation.json
python3 scripts/package_outputs_v7.py
```

The included audit can be reused for an unchanged rebuild because its camera,
actor, geometry and diagnostic hashes must match. After changing the camera
authoring, rerun the audit with Blender 4.3.2 and the unchanged V3 actor scene at
outputs_v3/Dinotigermon_Fang_Attacks_V3.blend. That scene supplies the same exact
retained prefix poses. Pillow is used for GIF packaging.

```sh
blender --background --python-exit-code 1 --python scripts/audit_camera_v7.py
python3 scripts/validate_mod_v7.py --baseline-report baseline_v6_validation.json
blender --background --python-exit-code 1 --python scripts/render_camera_v7.py -- bs01
blender --background --python-exit-code 1 --python scripts/render_camera_v7.py -- bs02
python3 scripts/package_outputs_v7.py
```
