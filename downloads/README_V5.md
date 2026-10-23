# Dinotigermon Fang Skills — V5 face cameras

V5 keeps the current opening shots, then pushes into Dinotigermon's face for
the bite and downward strike. The camera follows his muzzle and fangs through
the action, then eases back to the existing ending shot.

| Attack | Camera movement, in zero-based frames at 30 fps |
| --- | --- |
| Ground Fang | Opening unchanged through 30; push in by 42; hold the face through bite/tear to 70; ease out by 90 |
| Highland Fang | Opening unchanged through 27; push in by 41; hold through the downward strike and landing to 53; ease out by 74 |

Both player and enemy cameras are revised. The close view stays near-frontal
and lowers slightly when the head tips down, with floor clearance maintained.
Its target follows the skinned face bounds, including eyes, muzzle, lower jaw
and long fangs. It avoids orbiting with the rapid head turns. Highland's fading
landing shake is retained in the closer view.

Only four camera animation files change. The V4 wind/quake effect references,
V3 limb animation and biting mouth, repaired jaw geometry, skill descriptions,
damage, SP cost, targeting and all other game files are byte-identical.

## Install and preview

Replace the existing Dinotigermon folder with this ZIP's contents, with
ModConfig.json and dsts-loader at the mod root. Keep the existing
Reloaded-II/DSTS.ModLoader dependencies and enable one Dinotigermon copy.
V4 remains available for rollback.

The two GIFs use the actual exported player-camera position, rotation and field
of view at 16:9. They show the unchanged actor with the new close-ups. Native
game particles are not rendered in Blender.

## Checks and remaining game test

Seven checks verify the V4 baseline, four-camera-only changes, metadata and clip
lengths, unchanged opening/ending samples, smooth transitions, increased face
magnification, and the independent Blender framing audit. All full-close-up
frames of both camera angles keep the selected face/eyes/fangs inside the image
with margin. Their actual vertical coverage is about 68–74% of the 16:9 image.
Ground remains 120 frames; Highland remains 96.

These cameras have not been tested in the game. Differently sized enemies may
obstruct a close shot. V4's native particle playback still follows the existing
engine hit/effect sequence; this change does not retime those particles or
change combat hit timing. See the V4 notes for that limitation.

## Rebuild

Use Python 3 with numpy and the inspected Romsstar feature/refactor codec at
commit dfcef08a33f67abd5b96f0f121902c27060b7c57. Set DSCS_TOOLS_ROOT to that
checkout. The source ZIP contains scripts, the V4 validation baseline and shake
data, camera authoring diagnostics, and the independent framing report.

Extract V4's game files into modified_v4, excluding its bundled README. Begin
without modified_v5. From the V5 source bundle root, run:

```sh
python3 scripts/author_cameras_v5.py --baseline-report baseline_v4_validation.json
python3 scripts/validate_mod_v5.py --baseline-report baseline_v4_validation.json
python3 scripts/package_outputs_v5.py
```

An unchanged rebuild can reuse the supplied framing audit because its geometry,
camera, selection and diagnostic hashes must match. After editing the camera
authoring script, rerun the Blender audit. Download the unchanged V3 editable
scene and place it at outputs_v3/Dinotigermon_Fang_Attacks_V3.blend. With Blender
4.3.2 and Pillow installed, run:

```sh
blender --background --python-exit-code 1 --python scripts/audit_camera_v5.py
python3 scripts/validate_mod_v5.py --baseline-report baseline_v4_validation.json
blender --background --python-exit-code 1 --python scripts/render_camera_v5.py -- bs01
blender --background --python-exit-code 1 --python scripts/render_camera_v5.py -- bs02
python3 scripts/package_outputs_v5.py
```

The baseline actor scene remains unchanged. Editing the .blend alone does not
export the game camera clips; regenerate them through author_cameras_v5.py.
