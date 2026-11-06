# Community integration sources

Source-only patches applied over #1299 head `ee9b7765d579c46e45a7297493565e098f3bda33`. Generated translator binaries and the shader-chain stamp were rebuilt after integration. Historical results in the source branches are not new measurements.

| Contribution | Source commit | Original author | Integration |
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
