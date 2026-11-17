# Format

What the Unreal client of OpenSCCT reads from this repository and what it downloads. The design follows
[OpenSCCT-Content's format](https://github.com/Lumbridge/OpenSCCT-Content/blob/main/docs/FORMAT.md): the tree holds
only the index, the packs are GitHub Release assets, and older releases stay so a player can keep a pack until the
index moves on.

## The index

`index.json` on `main`, fetched raw:
`https://raw.githubusercontent.com/Lumbridge/OpenSCCT-Unreal-Content/main/index.json`.
`schema/index.schema.json` is its JSON Schema.

```json
{
  "format": "openscct.unreal-content",
  "formatVersion": 1,
  "updated": "2026-11-17T12:00:00Z",
  "cooks": [
    {
      "id": "5.8.4-0.8.161-1",
      "engine": "5.8.4",
      "game": "0.8.161",
      "commit": "<OpenSCCT-Unreal commit the cook was made from>",
      "platform": "Windows"
    }
  ],
  "shared": [
    {
      "id": "imported-shared",
      "title": "Shared imported content",
      "cook": "5.8.4-0.8.161-1",
      "version": "1.0.0",
      "treeSha256": "<sha256 of the pack's file list>",
      "download": { "url": "https://github.com/Lumbridge/OpenSCCT-Unreal-Content/releases/download/<tag>/shared-imported-shared-1.0.0.zip",
                    "size": 123456789, "sha256": "<sha256 of the zip>" }
    }
  ],
  "maps": [
    {
      "id": "stock/AquaE",
      "code": "AquaE",
      "title": "Enhanced Aquarius",
      "cook": "5.8.4-0.8.161-1",
      "version": "1.0.0",
      "contentHash": "66b2f44d79c6cf46",
      "level": "/Game/Imported/Maps/AquaE/L_AquaE",
      "requires": ["imported-shared"],
      "treeSha256": "<sha256 of the pack's file list>",
      "download": { "url": "https://github.com/Lumbridge/OpenSCCT-Unreal-Content/releases/download/<tag>/stock-AquaE-1.0.0.zip",
                    "size": 23456789, "sha256": "<sha256 of the zip>" }
    }
  ]
}
```

### Top level

- `format` is always `openscct.unreal-content`. `formatVersion` changes only when an old client could not read the
  index; a client skips an index with a newer version and says the game needs an update.
- Unknown fields are ignored everywhere.

### Cooks

Cooked Unreal content only loads in a game built from the same engine version and the same project content it was
cooked against. A cook is one such build:

- `id` is `<engine>-<game version>-<n>`, `n` bumped on every re-cook against the same game version.
- `commit` is the OpenSCCT-Unreal commit cooked from.
- A client uses only the shared packs and maps whose `cook` is its own cook id (built into the game at packaging
  time). Packs of other cooks are listed as "needs a game update" or "made for an older game". The index can list
  several cooks while players move between game versions.

### Shared packs

Imported content more than one map uses (character models, shared textures and sounds) is cooked once into a shared
pack. A map lists the shared packs it needs in `requires`; a client installs them first.

### Maps

- `id`, `code` and `title` are the same as the map's entry in OpenSCCT-Content.
- `contentHash` is the OpenSCCT-Content map's `contentHash` the level was imported from. A client plays the map only
  when its installed svmap pack has this hash; otherwise the level is out of date for the simulation and the client
  shows it as needing an update.
- `level` is the cooked level's package path, which the game opens to play the map.
- `version` is SemVer, starting at 1.0.0, the patch number bumped for every republish of the same id and cook.

The file list behind `treeSha256` is the archive's files as `<path>\t<sha256>\n` lines sorted by path (UTF-8),
hashed with sha256, as in OpenSCCT-Content.

## Pack archives

Each pack is a ZIP holding the cooked IoStore container of that pack at the root:

```
<name>.pak
<name>.utoc
<name>.ucas
```

- Archive names: `shared-<id>-<version>.zip` and `<category>-<code>-<version>.zip`. Clients use the URL from the
  index, never a name they build.
- The containers are already compressed, so entries are stored. Entries are sorted by path and have fixed
  timestamps (1980-01-01).
- Paths are plain file names: no `/`, no `..`. A client refuses any archive that breaks this.
- The containers are not signed or encrypted. The client checks the ZIP's sha256 against the index before mounting.

## Where packs install

| Who | Root | Layout |
| --- | --- | --- |
| Unreal client | `%LOCALAPPDATA%\OpenSCCT\unreal-packs` | `<cook id>/shared/`, `<cook id>/maps/<code>/`, `installed.json`, `downloads/` |

The client mounts its cook's installed shared packs at start and a map's pack when the map is chosen. A level the
player imported with the editor (in the game's own content) wins over a downloaded one.
