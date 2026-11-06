# The Legend of Zelda: Ocarina of Time

Historical community integration, superseded by upstream main `6121fd75312cb4d1af404a01921d437741fea91e` on 6 November 2026. The measurements below describe this branch, not the landed implementation. In particular, main uses a 4 GiB alias region with `mem_map_limit on`; this branch's 8 GiB experiment is not proposed as a replacement.

Switch 2 update `v131072` reaches early gameplay on the Mac. VideoToolbox decodes the 1920×1080 AV1 track. The movie library runs its NDVE compute shaders to produce the background behind the logo and menu. The baseline has also been exercised through movement, menus, a manual save and a process restart on an M3 Pro; see the dated test below. Wider gameplay validation remains open.

## Profile

SDK 22.2.8, NVN API 231.40, with Nintendo's sead, agl and gsys. The three loaded modules are `main`, `subsdk0` (NintendoSDK_movie), and `sdk`. The NPDM declares 48 MiB of system resources. Startup requests a 6.20 GiB heap, which fits in the profile's 17.625 GiB reservation with the stack and alias regions. The profile registers update `v131072`. Its original `v0` label was corrected after direct NUP header inspection. The base package has different module IDs and has not been validated here. This NUP lacks control data, so the official display version is unknown (`?`); the container update number is not substituted for it.

## Mac dry run

Use the rewritten `patched_v131072` and the extracted update RomFS. Build the host with `make -C dm/host GAME=0400486024DEC000` and use the [Mac shader build](../../../docs/development.md#build-by-hand-mac). From the repository, set `DUMP` to the folder containing `base` and `WORK` to the build and run workspace beside it.

```sh
mkdir -p "$WORK/run/save" "$WORK/run/temp" "$WORK/font"
python3 dm/tools/make_ext_font.py "$WORK/font/nintendo_ext.ttf"
DM_ROOT="$WORK" DM_ROMFS="$DUMP/base/romfs" DM_SAVE="$WORK/run/save" \
  DM_TEMP="$WORK/run/temp" DM_MSL_DIR="$WORK/shader/cache" DM_FONT_DIR="$WORK/font" \
  DM_NVN_SHMAP="$PWD/dm/games/0400486024DEC000/shader_map.tsv" \
  DM_WINDOW=1 DM_METAL_HUD=1 DM_FOCUS_KEEP=1 DM_AUDIO=1 \
  dm/host/dmhost "$WORK/patched_v131072"
```

`DM_FOCUS_KEEP=1` lets the title keep playing while another app has focus. The profile prewarms the 11 NDVE compute pipelines used by the title. On the Apple M5 that took 17.7 s on a cold run, with a progress screen, and 7 ms with the archive warm. Compiling the same pipelines during the first submission had blocked the first movie frame for tens of seconds. The larger scene shader set also triggers a separate graphics-pipeline prebuild. On the first run with that set, the startup screen took 86.9 s to hand out all pairs, and the remaining workers finished after 110.6 s total.

Start a new game to reproduce the opening scene transition. With the test save and title flow, `DM_KEYS=f2600:A,f2950:A` advances the menu automatically. `DM_NVN_SHDUMP` saves control sections and code for missing keys to the directory it names.

For a run past the opening, copy the committed save folder to the new run's `DM_SAVE` and load slot 00 from the menu. The checkpoint contains both `slot_00/progress.sav` and `slot_00/caption.sav`. The load screen identifies it as **Kokiri Forest Autosave**. A windowed reload reached the playable scene with the HUD and controls, so rendering can be tested without replaying the movie.

## Found on this game

- The online library's [eventfd for its poller](../../../docs/reference/services.md#eventfds-served-offline) and the [virtual audio input list](../../../docs/reference/audio.md#no-virtual-audio-input-devices-sdk-22) blocked startup.
- The SDK's [NDVE pack](../../../docs/reference/shaders.md#the-movie-librarys-ndve-shader-pack) contains 22 compute programs. The title route uses 11.
- The [raw write log and pool lookup](../../../docs/reference/frame-rate.md#a-full-frame-shader-overflows-the-raw-write-log) took the windowed title movie from a 47.6 ms median to 16.67 ms on an Apple M5, at 1920×1080 decode. A steady 300-frame window averaged 60.0 fps and its GPU median was 15.94 ms. A simultaneous ten-worker shader build caused additional stalls. The same run returned to 60.0 fps when the build paused.
- A [play report send](../../../docs/reference/services.md#an-asynchronous-play-report-opens-prepou) aborted at the first gameplay transition after a failed `prepo:u` connection. A Mac dry run with the offline completion finished the 9,254-picture opening movie and continued submitting the live scene. The title-only shader set left the scene black.
- The white 3D scene was agl's [conditional depth clears](../../../docs/reference/rendering.md#conditional-rendering-evaluated-at-replay) run unconditionally (resolved; the profile sets `render_cond on`). The main opaque pass tests `EQUAL` against a copy of the prepass depth that agl clears under a condition right after copying, a clear the console skips. Without the key the copy was 1.0, no lit geometry passed, and the full-screen sky (`943d8c0ece545cc8`) showed through. With the key the saved Kokiri Forest scene renders: Link, the ground, the props, lighting and the HUD.
- Mesh-shaded models were missing, among them the door of Link's house: the split read six stage slots of each BNSH program, and NVN2 keeps [task and mesh shaders in slots 7 and 8](../../../docs/reference/shaders.md#task-and-mesh-shaders-in-nvn2-bnsh-program-slots-7-and-8) (resolved). The new split adds 491 mesh and 17 task shaders to the map, 53 of the 54 keys the Kokiri Forest scene had asked for. 511 of their 520 build; the door renders.
- Kokiri Forest ran at about 20 fps with the GPU two-thirds idle: the [bindless pool table was rebuilt for every registration](../../../docs/reference/frame-rate.md#a-bindless-pool-table-rebuilt-for-every-registration), and about 12,000 descriptors of finalized texture objects churned the ghost ring (resolved). The frame is now 16–21 ms at the median on an Apple M5, submit 5–8 ms, and the GPU (14–21 ms) is the limit.
- The first 3D scene exhausted Metal memory when [bindless pool walks churned finalized textures](../../../docs/reference/nvn-objects.md#a-bindless-pool-churns-finalized-textures-within-a-submission). On the Apple M5, the failing run reached 18,930 MiB of residency. With submission-local backing, residency was 7,946 MiB about ten seconds after the movie and 7,871 MiB at 35 seconds. The opening 3D scene and its subtitles presented, and the test host exited normally.

## Open

- Resolved in the community shader build below (historical baseline): of the task and mesh shaders, nine did not translate: eight foliage shaders stop at Ampere opcode `0x243` (packed conversion, source A `RZ`, mode `0x0000c4ff`; three of them, `604134a5262ff670`, `ecead5cd0569091f` and `fed302d791a4ba43`, are drawn in Kokiri Forest), and one at ISBERD mode `0xb8` in a task or mesh shader. The scene also asks for `d17eefaff4f0bc23`, whose code is in none of the shader archives.
- The title-only build logged 3,331 distinct mapped libraries it could not open, including 363 shaders that failed when setting up programs. A targeted build for those programs and their dependencies translated 6,132 of 6,134 shaders. The two failures are foliage compute shaders `8692b34b521225be` and `83df46a0b1b379b4`, both with Ampere opcode `0x243`.
- With `render_cond` on and the shader build still incomplete, Kokiri Forest showed a hard horizontal cut, sky colour above the horizon over the trees. The volumetric fog composite (`888c1068ac8f7043`, agl) takes a pixel whose R32Float depth copy reads above 0.9999 as sky and paints the sky lookup there. The pass that fills that copy draws with agl's full-screen vertex shader `fb1ed54be1baced1`, which that build had not reached yet, so the host skipped the draw (`draw skipped: no render pipeline`) and dropped the target's store. Retest with the stamped full build. The sky lookup's peaks (about 105, 67 and 36 in RGB) are unverified against the console.
- Resolved by the integrated indirect-mesh implementation (#1312): the opening scene calls `nvnCommandBufferDrawMeshTasksIndirect`, which the earlier host did not implement. On update `v131072` (originally mislabeled `v0`) the probe caught the return at `main+0x2be2dc0`; the branch builds the command buffer pointer in `x0` and the indirect argument address in `x1`. It does not set `x2` on that path. A separate sample of the first 64 bytes at the indirect address returned zeros after the call; the populated record's layout is still unverified. The title-only run logged 11 keys missing from the map. The expanded run captured seven of them before the memory failure. A byte-for-byte comparison found none of those seven code areas in the extracted shader package.
- The 3D scene's CPU cost (37–59 ms a frame, with pool walks of about 33,000 descriptors) is [resolved](../../../docs/reference/frame-rate.md#a-bindless-pool-table-rebuilt-for-every-registration). Open: the GPU's 14–21 ms a frame in Kokiri Forest, which still drops frames below 60 fps; TBDR optimizations are off for this bindless game (`DM_NVN_TBDR=1` turns them on), and attachments load and store about 330 MB a frame.
- Historical leads, superseded by the integrated packed-conversion and float-atomic changes below: the Nov. 4 Lichii branches offered two shader leads. Its [packed conversion lowering](https://github.com/zortos293/Lichii/commit/35c043b3efaa147d1c1afe8a52cdf19fa92b77bf) handles opcode `0x243` with source A set to `RZ` and mode `0x0000c4ff`; both of this game's failed foliage programs use two registers and mode `0x000004ff`, which needs independent verification. Its [floating-point global reduction](https://github.com/zortos293/Lichii/commit/3aa89d1cdbe58a2bd5ded06b9c615c78da2ade70) matches a separate `F32FtzRn` diagnostic in the Mac shader build, where the translator drops that operation. Neither of the two shaders in the sky lookup and draw uses those instructions or the gather opcodes from Lichii's [fog work](https://github.com/zortos293/Lichii/commit/375c4a472bd95fdfaae0de6e3f192499cd14f806).

## Community integration experiment (2026-11-05)

The exact commits and integration decisions are recorded in [integration sources](integration-sources.md).

This branch combines community work discussed in [#1299](https://github.com/seedds/nxnative/pull/1299). It is an integration experiment: a comparison of the combined build cannot attribute an improvement to an individual change. Historical M5 measurements above remain the original author's observations, not measurements of this branch on an M3 Pro.

The source includes #1300 (packed conversion and ISBERD), #1304 (graphics tier), #1305 (display layer), #1308 (sRGB image views), #1312 (mesh indirect draws and image atomics), #1332/#1382 (GPU failure diagnostics), #1343 (multi-destination liveness), #1345 (audio update locking), #1350 (GPU timestamps), #1353/#1388 (NDSR entries), #1359 (floating-point atomics), #1360 (adaptive global-memory lookup), #1373 (heap-region helper), #1376 (LDSM barriers), #1390 (GPU-write range tracking), #1393 (retired texture descriptors), and #1420 (NvPkg unwrap). The related storage-retirement, pool-buffer reuse, bracket-depth and working-set guard changes are included as well. These are credited upstream contributions, not newly authored fixes in this branch.

The integrated NDSR reader uses #1388's wordwise walk while retaining both sets of regression tests. The retired-descriptor check runs in the common lookup before the fast path, retaining pool-buffer reuse and diagnostics. The duplicate forced virtual-input registration is removed; a regression test inspects the actual registration section and checks the remaining no-device handler.

### M3 Pro baseline

Test environment: M3 Pro, 36 GiB unified memory, macOS 27.2 (26B123), 2560×1440 window. Update v131072 was packaged with a derived profile; its three NSO build IDs match the profile originally labeled `v0`. Direct inspection confirms the base main is `33fa6c2c23848282a77e8b88a0a93f48`, while update main is `e0f3ccfaf50065ae4ea01eb5aeb13b1a`; the base subsdk0/sdk IDs are also different. The baseline source was #1299 head `ee9b7765d579c46e45a7297493565e098f3bda33`, plus the duplicate audio-registration removal.

Scripted input exercised Link's house, a door transition, the ladder, Kokiri Forest movement and camera, equipment/items/system menus, manual save, a clean process exit, relaunch and loading that manual save. The resumed outdoor scene accepted movement and opened Saria's conversation. No abort was seen in these bounded runs. Physical-controller input, audible sound quality, combat, later areas and extended outdoor stability were not verified.

The visible window showed overexposure and missing terrain/geometry. The user-confirmed Metal HUD sample was 22.15 FPS, 43.20 ms GPU and 45.14 ms frame interval. A separate warm outdoor 300-frame log window had frame median 48.45 ms, p95 59.58 ms and GPU median 47.39 ms. Peak sampled process/GPU memory was 19,439/14,207 MiB in the first run and 15,338/10,101 MiB after restart. These are samples, not guarantees or exhaustive peaks. Black padding in internal capture images was absent from the actual window and is not a player-visible rendering defect.

### Combined-build validation

Translator fixtures: 252 collected, 250 passed and two skipped. Static/profile/documentation checks: 77 passed. Host tests and the complete graphics fixture run passed on the M3 Pro. These validate mechanisms, not game compatibility.

The trial Mac bundle omits the offline GPU archive, so pipeline preparation occurs on the device. This historical trial profile uses an 8 GiB alias region and a 17.625 GiB reservation, preserving the original 9.125 GiB heap region. A 16 GiB alias region remains a separate diagnostic experiment. The matched measurements explicitly override the alias back to its original size; serial encoding (`DM_NVN_ENC_THREAD=0`) is also explicit. These choices do not change other games’ defaults. The combined build has now been compared at the same saved position; see the measured results below. The full build produced 42,853 of 42,854 base shader libraries, versus 42,823 of 42,851 in the baseline. The remaining translation failure is compute key `68bbe98b3bfd9876` (`agl_common.sharcb#BAHS@0x357f0`), unsupported Ampere instruction `481` at offset zero (`000000000000000000000000020c0481`); its origin and runtime relevance remain open. No Metal compilation failure remained. The additional three shader-map rows come from the NDSR reader, including previously missing `d17eefaff4f0bc23`. Comparing the actual library sets shows all 42,823 earlier successes remain ready, 27 earlier failures now compile, and all three added keys compile; [the per-key comparison](shader-compilation-comparison.json) records this. The remaining shader failure is not silently treated as a success.

### Matched saved-position results

All three cases used the same original manual save and camera, handheld mode, serial encoding, Metal4 on, bundled/user archives off, and the original `0x98000000` alias size. The trial bundle's private 8 GiB alias setting was explicitly overridden for this comparison. No build or signature work ran during the measured windows. The baseline and combined build render different amounts of geometry, so the baseline's faster incomplete picture is not equivalent rendered work.

The values below are medians of four separate 300-frame window medians, with the range of those windows' p95 values. They are not pooled 1,200-frame percentiles. [All measured windows](mac-comparison.tsv) are retained.

| Case | Frame median (ms) | Frame p95 range (ms) | GPU median (ms) | Peak sampled process/GPU through measurement (MB) |
| --- | ---: | ---: | ---: | ---: |
| Baseline, no archive, TBDR off | 47.79 | 58.90–60.19 | 47.23 | 10,362 / 9,347 |
| Community, TBDR off | 74.67 | 96.35–108.89 | 73.46 | 10,463 / 9,382 |
| Community, TBDR on | 46.84 | 72.09–78.07 | 46.49 | 10,518 / 9,428 |
| Community, warm off repeat | 46.05 | 69.78–82.48 | 45.59 | 10,495 / 9,446 |
| Community, warm on repeat | 47.20 | 69.46–75.98 | 46.82 | 10,488 / 9,395 |

The combined build restores visible terrain and foliage and substantially reduces the washed-out lighting in this view. This is not a claim that all lighting matches the console. The TBDR-on view retained those improvements; no new visible defect was observed there. The warm off/on repeats returned 46.05/47.20 ms respectively. The initial 74.67 versus 46.84 ms difference did not reproduce and does not establish a causal TBDR benefit; the exact cause remains unresolved.

Actual window captures: [baseline](../../../docs/images/ocarina-of-time-baseline.jpg), [community build](../../../docs/images/ocarina-of-time.jpg). The community capture was taken with TBDR off; its HUD shows the slower complete scene. Both captures show the actual window, without internal-capture padding.

The longer TBDR-on run exercised movement, camera changes, Saria's introductory conversation and its choice, Equipment/Items/System menus, and a manual save commit. The new manual save was visible in the load list after a full process restart, loaded back into the saved outdoor position, and accepted movement. An older house checkpoint also loaded and transitioned through the door back into Kokiri Forest, descended the ladder, and walked on the ground. It ran for more than fifteen minutes including title, outdoor play and dialogue, then exited cleanly. Peak sampled memory reached 11,583 MB process / 10,397 MB GPU; later samples fell to 10,842 / 9,734 MB. That bounded run does not establish leak-free or long-session behavior. Physical controller, audible sound quality, combat, the Deku Tree/Gohma and Hyrule Field have not been tested here.

The adaptive raw-memory table grew to 8,192 entries for 4,096 ranges; retired IDs were skipped and pool-buffer reuse was logged. These are direct observations of the integrated mechanisms, not isolated proof of each PR's gameplay effect. Remaining diagnostics include `nvnCommandBufferSetComputeWorkloadPriority`, AV1 packets refused while twelve pictures await buffer release, and a texture-type mismatch in shader `09ab31c69af710c9` (slot7, declared Metal type3, bound type5). No missing-library failure appeared in the compared community run.

A normal LaunchServices start stalled after the initial eleven-compute-pipeline preparation (90.01s cold). Two thread samples placed the main game thread in `pso_user_record → fopen → open$NOCANCEL`, before the usage file existed. The same signed bundle launcher, same environment and same external folders ran immediately when invoked directly for development. The cause of that launch-path/file-open difference is unresolved; it is not reported as a renderer deadlock or an audio-lock defect. The stalled run needed the host's timed shutdown; subsequent development runs exited cleanly.
