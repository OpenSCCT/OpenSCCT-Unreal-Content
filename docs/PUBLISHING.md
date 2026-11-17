# Publishing

How the paks are cooked and published. The tooling is being built in OpenSCCT-Unreal; this page is the plan it
follows and will name the exact commands once they exist.

You need:

- an OpenSCCT-Unreal checkout at the commit to cook from, built, with the maps imported (`Tools/Import/Import-Map.ps1`
  and the README's asset steps) from the same OpenSCCT-Content packs the index's `contentHash` values name
- Unreal Engine at the version the cook names
- the GitHub CLI (`gh`, logged in with push access to this repository)

Steps:

1. **Cook.** OpenSCCT-Unreal cooks the shared imported content into one IoStore container and each map's level
   (with what only it uses) into its own, using Unreal's chunk assignment so a map's pack holds nothing a shared
   pack already has. Cooking runs headless.
2. **Pack.** Each container goes into a ZIP as `docs/FORMAT.md` describes; the tool writes the index entries
   (sizes, hashes, tree hashes, cook id, the map's `contentHash`).
3. **Publish.** One GitHub release `unreal-content-<date>` per publish with every archive attached, then the new
   `index.json` committed to `main`.
4. **Check.** The tool downloads the index and every archive it names and checks the hashes.

`staging/` (ignored by git) holds the cook output and the archives.
