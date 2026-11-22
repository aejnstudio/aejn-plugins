# Maps, MLOs, buildings

## Before building an MLO: does it need to be one?
A real GTA interior (MLO: ytyp with CMloArchetypeDef rooms/portals) is fragile. One custom hub crashed the game 8 times (`GTA5_b3258.exe+93419F`, "high-sad-wolfram") until the rooms were shipped as **plain ymap entities + world-space collision (static build)**. That version worked first time.
- Default to the static approach for custom-built rooms: room drawables placed in a ymap, one world-space `.ybn`, lights/timecycle optional.
- Only go MLO when you truly need interior culling/timecycle, and then copy a vanilla interior's settings exactly.

## MLO settings that must match vanilla
- MLO instance in the placement ymap: `lodLevel` ORPHANHD (not HD), `contentFlags` 73 (not 9).
- Room archetype flags 32 (not 0).
- MLO instance rotation in a ymap is NOT inverted. Entities INSIDE an MLO ARE inverted (stored = -heading).
- Adding entities to an existing MLO's ytyp froze loading at 76%. Place extra props from client script: `CreateObject` + `ForceRoomForEntity`, only after `LocalPlayer.state.isLoggedIn`.
- Before claiming a fix, bisect: no props -> no MLO -> only collision cuts. Change one thing per test and write down each test result.

## Cutting doors into a GTA building
- Find the building drawable + its `.ybn` (and `hei_` override versions: the `hei_` ones win). Note origin + rotation; work in the building's local frame.
- Cut BOTH the drawable and the collision. Doorway floor gaps: add floor slabs or people fall through wall gaps.
- Collision boxes: split boxes along their own axes. Remapping single corners of slightly rotated boxes makes sheared/degenerate boxes = crash on approach. Verify 0 sheared boxes before shipping.
- Painted "doors" on facades are only textures; you have to cut the real opening.

## Removing / hiding world things
- Near models: `CreateModelHide(x,y,z,radius,model,true)` from client.
- Far LOD/SLOD versions are NOT hidden by CreateModelHide. Stream an edited copy of the game's ymap with those entities scaled to 0.001 (Vinewood letters: `ch2_03.ymap`).
- Overlapping map packs: when two packs stream the same ymap (e.g. `cs5_roads_strm_0`, `cs4_occl_04`), only one wins; merge or move one aside.

## Retexturing signs/logos in existing maps
- Swap texture data inside `.ytd` in memory with CodeWalker: OK.
- NEVER save an edited `.ydr` from memory (`YdrFile.Save` after swapping embedded textures) - crashes the game. Go ydr -> XML -> replace dds -> xml2bin.
- Runtime `AddReplaceTexture` on textures embedded in room ydrs is a crash suspect. For editable pictures use a separate ytd.
- Keep the originals.

## World names, map, minimap
- Street names: hashes are NOT joaat(name). Dump them in game (LoadAllPathNodes + GetClosestVehicleNode grid), then `AddTextEntryByHash(hash, name)`. Zones: `AddTextEntry(code, name)`.
- Custom minimap only shows if you stream stub `minimap_X_Y.ydd` radar files (77 of them), otherwise radar draws GTA vector roads.
- Pause map pixel <-> world (postal map used): px = 0.6779*x + 2833.3, py = 5747.8 - 0.6779*y.
- Cannot: new land, left-hand traffic (engine). Say so honestly.

## Finding positions
Ask the user to stand there and use a `/pos` or `/savespot <name>` style command; never guess a building from a description (it is easy to build on the wrong one).

## Height changes reach every script
When a map's floors move, every script position based on them moves too: shop peds, door system coords, stash points, flat zones. Regenerate them all from the build's spots and check the upload sends the regenerated file, by date, not by name.
