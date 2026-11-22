# Assets: textures, models, clothing, vehicles

## CodeWalker.Core from PowerShell (headless, no GUI)
- Load: `Add-Type -Path CodeWalker.Core.dll`, `GTA5Keys.LoadFromPath(<GTA dir>)`, `RpfManager.Init(...)` to read game files.
- Meta: `MetaXml.GetXml` (bin->xml), `XmlMeta.GetMeta` + `ResourceBuilder.Build(meta, 2)` (xml->ymt).
- Animations: Sollumz exports `.ycd.xml` only; convert with `XmlYcd.GetYcd(...)` then `Save()`.
- Generic xml2bin for ydr/ybn/ytyp/ymap via CodeWalker's XML importers.
- Use `-LiteralPath` everywhere: folders have `[brackets]`.

## Textures and lag
- Client lag/hitches are usually oversized streamed textures, not the server. `fxserver.log` "uses N MiB of physical memory" names the fat ones. Car ytds of 100-220 MB (8192px, no mips) are the classic cause. FiveM warns over 16 MB per asset.
- Shrink recipe: export DDS (CodeWalker DDSIO) -> Pillow resize cap 2048 (maps/normals 1024) -> full mip chain -> DXT1 (no alpha) / DXT5 (alpha) -> rebuild ytd from texture list -> reload check. One fire engine went from 218 MB to 57 MB.
- Upload replacements only when the server copy matches the local backup size; keep a resume list.
- Ytds truncated at exactly 16777215 bytes are broken dumps: car shows no textures.
- Always real compressed DDS with mips. Uncompressed BGRA labelled DXT1 with 0 mips crashed the game.

## Blender + Sollumz (models, headless)
- Run: `blender.exe -b -P script.py`. Export with `sollumz.export_assets` (binary ydr in `gen8/`).
- UV map must be named `UVMap 0`, colour layer `Color 1`, embedded textures DXT1 dds. Wrong UV name = flat colour.
- Skinned import with external skeleton: `dwd_import_external_skeleton = "FROM_DIR"` with the `.yft.xml` beside the `.ydd.xml`.
- Real body for checks: extract male freemode `uppr_000_r`, `lowr_000_r`, `head_000_r` ydd.xml + skeleton, copy the pose bone by bone, render, LOOK at it.
- Liveries/paint: find where a panel sits with a numbered grid texture first; check UV overlaps before baking (overlapping UVs = drop the model).

## Add-on clothing (EUP style)
- Pipeline: extract base `.ydd`/`.ytd` -> paint textures (Pillow) -> render on body in Blender to check -> write shop `.meta` + `CPedVariationInfo` XML -> ymt -> wrap DDS into ytd with the inner texture renamed.
- Apply in script: `SetPedCollectionComponentVariation(ped, comp, '<collection>', drawable, texture, 0)`.
- **Adding any clothing pack shifts global clothing numbers** and breaks saved outfits/skins stored by global index. Warn first; store collection name + local index.
- Show the user the 3D render, not the flat texture sheet.
- Crash hunting in clothing: disable packs in halves; `SET_PLAYER_MODEL`/`CREATE_PED` exceptions point to a clothing pack.

## Vehicles
- Model names: read `<modelName>` from the pack's vehicles.meta and check it against the file list.
- Plate shape is per model; UK plate textures via a plate texture replace.
- Sirens: unique siren ids per car (`carcols.meta`); LVC/ELS clash - pick one per car family.
- Map packs that reference unrelated servers' files, bundle other creators' paid files, or produce hundreds of "Duplicate Archetype" lines may be unlicensed dumps: ask the user where they came from.

## Files pulled out of a nested RPF have no header
RpfFile.ExtractFile on a resource returns the decompressed body with no RSC7 header. Streaming that file crashes every client on join: `Invalid fixup, address is neither virtual nor physical (in x.ytd)`. Wrap it first: ResourceBuilder.Compress, then AddResourceHeader. Before streaming, check every file starts with RSC7. After any upload, read logs/server.log for "Game crashed".
