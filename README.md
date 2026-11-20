# OpenSCCT-Unreal-Content

Cooked map content for the Unreal client of OpenSCCT: each map's Unreal level with everything it draws and plays
(meshes, textures, materials, lights, effects, sounds), cooked for Windows so the game can download it and play the map
without importing it with the editor.

This repository works alongside [OpenSCCT-Content](https://github.com/OpenSCCT/OpenSCCT-Content), which holds the
converted maps in OpenSCCT's own formats. The two split the work:

| Repository | What a map download brings | Who needs it |
| --- | --- | --- |
| OpenSCCT-Content | the map's svmap pack: `manifest.json` and `sim/` (what the simulation and servers read) | every client, dedicated servers, the hub |
| OpenSCCT-Unreal-Content (here) | the map's cooked Unreal level | the Unreal client only |

The Unreal client downloads both for a map. It links them by the map's `id` and by the simulation's `contentHash`,
so the level it draws always matches the simulation it plays.

See the [OpenSCCT project directory](https://github.com/OpenSCCT/OpenSCCT) for the original PS2,
PC/Steam and modern Unreal editions. To contribute a cooked level, start with the
[Unreal client](https://github.com/OpenSCCT/OpenSCCT-Unreal) and follow the
[cook and publish guide](docs/PUBLISHING.md) for this catalog.

## What is here

The paks are ZIP archives attached to this repository's [releases](https://github.com/OpenSCCT/OpenSCCT-Unreal-Content/releases)
(`unreal-content-<date>`). The tree holds only:

| Path | What |
| --- | --- |
| [`index.json`](index.json) | Every pack: version, size, sha256, download URL, the cook it belongs to and the OpenSCCT-Content map it draws |
| [`schema/index.schema.json`](schema/index.schema.json) | JSON Schema of the index |
| [`docs/FORMAT.md`](docs/FORMAT.md) | The index and archive formats (the contract the Unreal client reads) |
| [`docs/PUBLISHING.md`](docs/PUBLISHING.md) | How the paks are cooked and published |
| [`tools/`](tools) | Packing, publishing and checking tools |

Nothing is published yet: the index is empty until the first cook.
