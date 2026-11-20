# Unreal map packaging

Players build the stock Versus content from their own Steam installation. Approved community-created maps are distributed as reviewed source recipes that can use those local assets. The [community catalogue](https://github.com/OpenSCCT/OpenSCCT-Content/blob/main/community.json) lists the current recipes, including file digests and source revisions.

## Inspect a local cook

Use a matching Unreal client checkout and engine version. Build the SVMap locally, then use the client repository's [map pak guide](https://github.com/OpenSCCT/OpenSCCT-Unreal/blob/main/docs/MAP-PAKS.md) to cook and inspect it. Keep generated containers under `Saved/MapPaks/` in that checkout and packaging inputs under this repository's ignored `staging/` directory.

Historical cook indices link a map ID and SVMap simulation hash to a cooked Unreal level. The [format](FORMAT.md) describes those fields and the Windows container layout. Local cooks can contain original meshes, textures, audio or effects from the player's game. A map author who wants to offer an optional download should submit the community-created source files and attribution through the community catalogue.
