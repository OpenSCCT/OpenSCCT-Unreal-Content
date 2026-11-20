# OpenSCCT Unreal Content

This repository documents the Unreal IoStore packaging format and tools for local development cooks. In the [OpenSCCT project directory](https://github.com/OpenSCCT), Godot and Unreal are **Modernisations**. The PS2 disc and original PC/Steam game are **Originals**.

## Playing and editing maps

Prepare your installed Steam copy of Splinter Cell Chaos Theory in the launcher. The [Unreal modernisation](https://master.lumbridge.co.uk/projects/unreal), [Godot modernisation](https://master.lumbridge.co.uk/projects/godot) and [OpenSCCT Editor](https://github.com/OpenSCCT/OpenSCCT-Editor) use the locally built Versus maps and shared assets. The Steam-supplied maps form the starting roster. Additional public downloads are limited to approved community-created map content. The launcher can convert a community map source with the player's own stock assets.

## Local development cooks

An SVMap contains the manifest and simulation. An optional cooked Unreal map contains an engine-specific presentation. The map ID and simulation content hash identify its compatible simulation. Keep local cooks that include retail meshes, textures or sounds on your own machine.

| File | Purpose |
| --- | --- |
| [Unreal map pak guide](https://github.com/OpenSCCT/OpenSCCT-Unreal/blob/main/docs/MAP-PAKS.md) | Cook and inspect a map in a development checkout |
| [Cook catalogue](index.json) | Cook IDs and map packaging metadata |
| [Schema](schema/index.schema.json) | Catalogue validation |
| [Archive format](docs/FORMAT.md) | Unreal archive layout |
| [Packaging tools](tools/) | Local archive preparation and verification |
