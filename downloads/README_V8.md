# Dinotigermon Fang Skills — V8 close-ups and main effects

V8 brings the camera close to the eyes and muzzle much earlier, then reveals
the full fangs for the strike. Ground Fang still ends on its jaw. The first
seven camera frames, left-to-right direction, active endings, mouth movement,
and accepted actor motion are preserved. This revision uses framing to reduce
visible leg motion; it does not change the leg poses.

| Attack | Camera | During-animation effect |
| --- | --- | --- |
| Ground Fang | Muzzle close-up from frame 16; full fangs for the bite; final jaw portrait | Ivory wind ribbons following the head during the bite and tear |
| Highland Fang | Muzzle close-up from frame 18; full fangs for the downswing | Earth-colored rocks and an expanding ground shockwave during the landing strike |

## Effects and sound

The supplied Kumamon & Korikakumon mod by Dacare provides working examples of
complete local main-effect bundles. V8 uses that same connection: skill column
12 points to a battle-effect row with mode 1/type 3, backed by local BSON, GEOM,
ANIM, NLST and texture assets. V7 had no main effect and used the later hit
slots, explaining why particles appeared only after the enemy was hit.

Ground uses the reference's crossing wind ribbons with the ghost claw meshes
and independent snowfall removed. Highland uses its rocky shockwave. The blue
uniform tints become ivory and earth colors; all other effect geometry bytes,
including textures, mesh buffers and skeletons, are preserved. Only the eight
required custom texture files are copied; no Kumamon character or gameplay
tables are installed.

Both effect animations retain their native 512 Hz timebase. Transforms and
material animation channels are retimed together to the attack windows, with
wind placed around the head and the quake centered under the final plant.
The wind dissipates before the last jaw portrait so the mouth remains clear.
Their lengths match the actor clips within one effect sample. All active
effect sound-volume fields are zero, and the obsolete later full effects are
cleared. This removes effect-owned attack sounds without changing global game
audio or voice settings.

Automatic description IDs stay `0`; custom descriptions remain removed.
Damage, SP cost, element, targeting, hit count and the engine's after-animation
damage setting are unchanged.

## Install and check

Replace the entire old Dinotigermon mod folder with this ZIP's contents and
enable only one copy. Keep the existing Reloaded-II/DSTS.ModLoader dependencies.
Do not merge over an older folder, because that can retain removed description
files. V7 remains available for rollback.

The camera and binary/data checks pass. Independent Blender projections put
the early muzzle at 75–83% image height and Ground's ending jaw at 65–67%.
The camera GIFs use the actual exported cameras and unchanged actor poses;
they have no final-frame pause and do not render native particles.

This build has not been run in the game. Main-effect playback, native shader
appearance and combat-space placement still need an in-game check. Blender
effect alignment renders use the real effect geometry with approximate
materials; they cannot establish native particle visibility or audio behavior.

## Rebuild

Use Python 3 with numpy, Pillow for GIFs, Blender 4.3.2 for visual checks, and
Romsstar's Blender-Tools-for-DSCS `feature/refactor` codec at commit
`dfcef08a33f67abd5b96f0f121902c27060b7c57`. Set `DSCS_TOOLS_ROOT` to that checkout.

Extract the published V7 test ZIP into `modified_v7`, excluding its bundled
README. Extract the supplied `KumamonKorikakumon_55047.zip` into
`reference_kumamon`, retaining that ZIP for provenance verification. Extract
the V8 source bundle into the working directory. Starting without `modified_v8`:

```sh
python3 -c "import shutil; shutil.copytree('modified_v7', 'modified_v8')"
python3 scripts/author_cameras_v8.py
python3 scripts/author_effects_v8.py --reference reference_kumamon --reference-zip KumamonKorikakumon_55047.zip
python3 scripts/validate_mod_v8.py --reference-dir reference_kumamon --reference-zip KumamonKorikakumon_55047.zip
python3 scripts/package_outputs_v8.py
```

The included framing audit can be reused for an unchanged rebuild: its hashes
must match every exported camera, actor, selection and diagnostic. After changing
cameras, regenerate it using the published V3 Blender scene at
`outputs_v3/Dinotigermon_Fang_Attacks_V3.blend`:

```sh
blender --background --python-exit-code 1 --python scripts/audit_camera_v8.py
blender --background --python-exit-code 1 --python scripts/render_camera_v8.py -- bs01
blender --background --python-exit-code 1 --python scripts/render_camera_v8.py -- bs02
python3 scripts/validate_mod_v8.py --reference-dir reference_kumamon --reference-zip KumamonKorikakumon_55047.zip
python3 scripts/package_outputs_v8.py
```
