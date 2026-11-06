# Community integration sources

Source-only patches applied over #1299 head `ee9b7765d579c46e45a7297493565e098f3bda33`. Generated translator binaries and the shader-chain stamp were rebuilt after integration. Historical results in the source branches are not new measurements.

| Contribution | Source commit | Commit author | Integration |
| --- | --- | --- | --- |
| renderer-1312 | `25694069fc0c30502879cb1fa62efbfd2d81f3dc` | nanotchec | applied |
| f2ip-first-1300 | `10b3d311db3c3f040681a5071ad68949a63021e5` | Pedro Nascimento | applied (kept requested tests; omitted unrelated Vote context) |
| f2ip-rounding-1300 | `ac1ebfdf0f0f0f70a06fd2a0d8e44ef48df5e385` | Pedro Nascimento | applied |
| wide-writes-1320 | `7b4f34bd5ef15f30475fdc14f249a0c4f4fa0fe9` | seedds | applied |
| float-atomics-1359 | `ccf7a987437cc55e9c0f666fe45216a9dab08000` | Pedro Nascimento | applied |
| ndsr-1353 | `0edb78db7f0f00866cbff0d0b2f77a4e8a786d62` | Pedro Nascimento | applied |
| pool-table-1360 | `4857e4277a11cd9d08d37c1ff3a4f02f906806ce` | Pedro Nascimento | applied |
| global-atomic-cas | `f6748a2d3fa7f238e8b5b1b9f0f9e6ff028e4e22` | seedds | applied |
| bracket-depth-1357 | `56b73f21994e95d37df30faba8a04670599df3bf` | seedds | applied (kept bracket import, omitted unrelated IOS_PULL context) |
| pool-reuse-B2 | `04f58392bf368cb6721f3cd70d3f8b1919884860` | seedds | applied |
| retired-storage-B6 | `2ecffd2a982cfe233dc57f04fc8935d2f62fd16a` | seedds | applied |
| memory-accounting-B6 | `40dfe2ba4eab65d7561355c3906b383dc4437624` | seedds | applied |
| working-set-stop-B6 | `6c71e575a597d3129add256ab2634f1d397a5ab1` | seedds | applied (omitted unrelated iOS guest-file documentation context) |
| graphics-tier-1304 | `6542d07e9fedde27d9bcc969dbbde69a2673b6b3` | Pedro Nascimento | applied |
| display-layer-1305 | `0b91ce92e6600fa1ad7cf219dd4b3b4f5fb9175b` | Pedro Nascimento | applied (combined tier and layer docs; kept eventfd and display tests) |
| srgb-images-1308 | `e9d2e53752a64485bd5b4ffe831e1db88eb1faf9` | Pedro Nascimento | applied (kept all independent reference links) |
| audio-lock-1345 | `d7a43c2ad3cbc83f45c08264376998d1e8026b8b` | seedds | applied |
| live-inputs-1343 | `6941aa847dee2eeec66da4734f192067158a2269` | seedds | applied |
| gpu-timestamps-1350 | `9e5e87634b4a779df713f49fac071b5baff89647` | beecrepaldi-afk | applied |
| gpu-timestamps-followup-1350 | `ef817593e80d51997e07b6afb94501e86a30eada` | beecrepaldi-afk | skipped: upstream-main merge commit, intended GPU timestamp change already applied from first commit |
| ldsm-barriers-1376 | `2d6f86f819c4b80ed4bbd80c66aa623e6e7dc85f` | Metal | applied |
| ndsr-word-walk-1388 | `c8aeba864885f25cefff20e1811a447c461079ce` | Romen Adama Caetano Ramírez | applied: word-wise reader supersedes 1353 entry-wise scan; tests from both retained |
| cpu-terrain-ranges-1390 | `16e86fdc17543e7bc2b247c5763252d573cc89e6` | Pedro Nascimento | applied (kept B2/B6 and new range tests) |
| skip-retired-1393 | `3a534edd06b4c0d4a0a10a02137b0cec6850601f` | David Lepe | applied: moved skip into shared pool_entry before fast lookup; cumulative skip count retains patch/reuse metrics |
| packaged-texture-1420 | `58c001d92cf5e5f6b72b0b2c28d52c2facc7d55c` | Pedro Nascimento | applied (kept all bindless, reuse, range and packaged-entry tests) |
| gpu-lost-1332 | `7f5f2fcf967532d0bf6fbd824371277a032965b0` | Puca Vaz | applied |
| gpu-error-names-1382 | `c87cc8cdc9c23aaa6ba40d963078be3eb4f840b4` | Puca Vaz | applied (kept program classification and shader tags; retained both diagnostic switches) |
| heap-alias-preservation-1373 | `b44bcafdffcb147f2e224ee6d3a9ed45f5a4a708` | enjoyerofslop | applied |
| codec-eos-diagnostics | `7b4b5f6c77db11ecd62241b35da981c02407630c` | Pedro Nascimento | applied |

## Original PR contributors

Commit author fields above are preserved metadata and do not replace attribution to the people who proposed the PRs. The first group was selected from [stan69b’s pinned oot-clean integration](https://github.com/stan69b/nxnative/commit/6c71e575a597d3129add256ab2634f1d397a5ab1), then combined with the follow-ups below.

| PR | Contributor | Proposal |
| --- | --- | --- |
| [#1300](https://github.com/seedds/nxnative/pull/1300) | [lunks](https://github.com/lunks) | Ampere F2IP and a task shader's own payload read: Ocarina of Time's untranslated shaders translate, all but one that is data |
| [#1304](https://github.com/seedds/nxnative/pull/1304) | [lunks](https://github.com/lunks) | The graphics scale is tier 0 in both modes: Star Fox's docked 3D was 768x432, smaller than handheld |
| [#1305](https://github.com/seedds/nxnative/pull/1305) | [lunks](https://github.com/lunks) | A Switch 2 display's largest layer is its largest buffer: Star Fox outputs 1920x1080 on a 4K screen, rendering 960x540 |
| [#1308](https://github.com/seedds/nxnative/pull/1308) | [lunks](https://github.com/lunks) | An sRGB texture in an image slot is loaded and stored raw, as on NVN: Star Fox's picture was encoded twice |
| [#1312](https://github.com/seedds/nxnative/pull/1312) | [nanotchec](https://github.com/nanotchec) | Zelda: Ocarina of Time — Restore missing mesh and texture effects on Mac |
| [#1320](https://github.com/seedds/nxnative/pull/1320) | [nanotchec](https://github.com/nanotchec) | NXNative: keep wide GPU writes from stalling unrelated draws |
| [#1332](https://github.com/seedds/nxnative/pull/1332) | [PucaVaz](https://github.com/PucaVaz) | A GPU whose access macOS revoked stops the game instead of leaving it running with nothing drawn |
| [#1343](https://github.com/seedds/nxnative/pull/1343) | [nanotchec](https://github.com/nanotchec) | Fix Ocarina of Time foliage jitter by preserving live shader inputs |
| [#1345](https://github.com/seedds/nxnative/pull/1345) | [nafields](https://github.com/nafields) | SDK 22's audio update lock is read off UpdateImpl: the host locked +0x2d0 where SDK 22.2.8 locks +0x2d8, and Ocarina of Time's render thread crashed in the opening movie |
| [#1350](https://github.com/seedds/nxnative/pull/1350) | [beecrepaldi-afk](https://github.com/beecrepaldi-afk) | GPU timestamps per segment leave out the GPU's idle time between segments (dynamic resolution flicker, issue #1290) |
| [#1353](https://github.com/seedds/nxnative/pull/1353) | [lunks](https://github.com/lunks) | NDSR pack read entry by entry: a program after a 4-byte data entry is no longer stepped over (Ocarina of Time's SMAA weights) |
| [#1357](https://github.com/seedds/nxnative/pull/1357) | [lunks](https://github.com/lunks) | Metal compiles take -fbracket-depth=1024: two Ocarina of Time compute shaders nest 259 brackets |
| [#1359](https://github.com/seedds/nxnative/pull/1359) | [lunks](https://github.com/lunks) | Float add atomics are kept: RED .ADD.F32.FTZ.RN was dropped, which washed out Ocarina of Time's outdoor scenes |
| [#1360](https://github.com/seedds/nxnative/pull/1360) | [lunks](https://github.com/lunks) | Raw global memory's pool table grows with the pools: pools past 4,095 were left out and hung the GPU in Ocarina of Time |
| [#1373](https://github.com/seedds/nxnative/pull/1373) | [enjoyerofslop](https://github.com/enjoyerofslop) | A provisional profile's heap check that grows the block keeps the profile's alias region out of the heap region (Ocarina of Time, issue #1361) |
| [#1376](https://github.com/seedds/nxnative/pull/1376) | [MetaIX](https://github.com/MetaIX) | LDSM is fenced by subgroup barriers: Ocarina of Time's movie filter read other lanes' shared-memory stores before they landed |
| [#1382](https://github.com/seedds/nxnative/pull/1382) | [PucaVaz](https://github.com/PucaVaz) | A GPU hang or page fault names its command buffer: frame, segment, passes and the shader keys it ran |
| [#1388](https://github.com/seedds/nxnative/pull/1388) | [Romen-Adama-Dev](https://github.com/Romen-Adama-Dev) | An NDSR pack's record table is read one word at a time: a constant-data pair between two shaders' pairs hid every other shader of Ocarina of Time's agl_ndsr_vr.pck |
| [#1390](https://github.com/seedds/nxnative/pull/1390) | [lunks](https://github.com/lunks) | GPU-written storage is tracked as ranges, so a pool that holds both kinds no longer leaves its CPU-written textures at zero |
| [#1393](https://github.com/seedds/nxnative/pull/1393) | [david-on-spring](https://github.com/david-on-spring) | Profile key pool_skip_retired: pool tables give a finalized texture's ID the dummy instead of making its texture again (Ocarina of Time out of GPU memory at a scene change) |
| [#1420](https://github.com/seedds/nxnative/pull/1420) | [lunks](https://github.com/lunks) | A packaged texture entry is no longer handed out as the texture: a serial that left the CPU-written set while its NvPkg stayed made tex_mtl return the NvPkg and tex_by_id throw |

The codec end-of-stream diagnostic comes from [lunks’s source commit](https://github.com/lunks/nxnative/commit/7b4b5f6c77db11ecd62241b35da981c02407630c). The GPU-write tracking proposal credits aCidcOde’s patch. ChiefSZ13’s original game port remains the base of this work.

## Recorder follow-up from #1387

A focused comparison of [vinifloriano’s alternate port](https://github.com/seedds/nxnative/pull/1387) identified a distinct indirect-mesh recorder addition. This integration takes only `nvn_rec.h` and the NVN command-recording regression from `42203e3396cfc10a832e34f484cf325af70339b8` (Vinicius Floriano). The duplicate profile/audio registration and that fork’s unrelated newer-main cache architecture are not overlaid. The recorder preserves the indirect address and all object/mesh threadgroup dimensions when the encoder thread is enabled; the earlier serial-encoding measurements do not exercise it.

## Validation coverage

These checks distinguish mechanism coverage from gameplay attribution. A combined picture improvement does not isolate one source PR, and mocked failure fixtures do not establish recovery from a real GPU fault.

| Contribution group | Checks exercised | Runtime scope / limits |
| --- | --- | --- |
| Packed conversion, ISBERD, float/global CAS atomics, multi-result liveness, LDSM and bracket depth | Translator fixtures:252 collected,250 passed,2 skipped; full shader build and per-key comparison |27 previous failures now compile,3 newly found keys compile,1 existing failure remains. Compiling does not prove every shader's numerical behavior. |
| NDSR readers (#1353/#1388) | Both entry-walk and word-walk fixtures retained; final reader is wordwise |Three added keys include previously missing`d17eefaff4f0bc23`. |
| Wide writes / GPU-written CPU terrain / adaptive raw-global lookup |`t_sbw`, `t_cpu_storage`, `t_gmem_raw` |Terrain is visible in the combined build; raw table grows to8192 entries for4096 ranges. No isolated terrain-patch attribution. |
| Mesh indirect draws and recorded encoding |`t_nvn` command-recording checks; `t_mesh_indirect` GPU-written grid/base/zero-work checks and deferred dispatch of all7 arguments |Serial gameplay route exercised. Threaded route is checked separately. |
| Image usage, integer aliases and sRGB views |`t_image_usage`, `t_image_alias`, `t_image_srgb` and image/view fixtures |Restored picture observed with the complete integration; console color matching is not established. |
| Deferred release / retired storage / descriptor skipping / pool buffer reuse |`t_deferred_release`, `t_pool_retired`, `t_pool_chunks`, CPU texture/storage fixtures |Runtime records skips/reuse and no finalized-texture reconstruction in the sampled run; the off variant is reported separately. |
| Packaged texture unwrap (#1420) |Packaged-entry coverage in`t_cpu_tex`; runtime manual save and repeated scene loads |No`-[NvPkg buffer]` exception in the tested route. The Deku Tree/Gohma route is untested. |
| Graphics tier and display layer |`t_oe`, `t_vi_display`/front fixtures |Docked tier0/1 is a separate comparison; no universal speed claim. |
| Audio duplicate takeover / renderer lock |Actual forced-registration-section uniqueness and empty-device/no-buffer-write checks in`t_audio`; host suite |Boot, dialogue, save and restart work. Audio callback logs are not an audible-quality or contention stress test. |
| Heap-region preservation / alias headroom |Profile-guess/memory-profile fixtures; separate8/16GiB runtime configurations |The6.20GiB heap request still fits the9.125GiB heap region. The reported later streaming-address abort is not claimed reproduced or fixed by the short comparison. |
| GPU loss, labels and working-set stop |`t_gpu_lost`, `t_gpu_hang_names`, `t_gpu_mem_stop` and front fixtures |Synthetic failure coverage; no real GPU reset deliberately induced. A guard exit would be reported as a limit, not success. |
| Codec end-of-stream diagnostic |Host build; clean-ending log when available |Diagnostic only. It does not fix or validate the reported Gohma/Hyrule Field abort. |

The full host/graphics suites, Python/C lint and documentation checks are reported in the game note and PR. Other-game and iOS gameplay validation remain outside the available prepared builds on this machine.
