# Debugging FiveM problems

Read logs before guessing:
- Client: `%LOCALAPPDATA%\FiveM\FiveM.app\logs` (CitizenFX_log_*.log, crash dumps). Grep "BLOCKING LOAD", "Duplicate Archetype", "has no mounter", the crash signature.
- Server: txAdmin `fxserver.log` (resource errors, memory per asset), `admin.log` (who changed what).

| Symptom | Usual cause | Fix |
|---|---|---|
| Crash `GTA5_b3258.exe+93419F` "high-sad-wolfram" near a custom interior | MLO setup / bad map files / leaked map dumps | Bisect (props -> MLO -> collision); ship static ymap build; disable leaked packs |
| Crash on approach to edited building | Sheared/degenerate collision boxes | Split boxes along their own axes, verify |
| Crash after retextured ydr | ydr saved from memory | Redo via XML -> xml2bin |
| `SET_PLAYER_MODEL` / `CREATE_PED` exception | A clothing pack | Disable packs in halves until found |
| White box instead of texture/NUI | Stream/files list changed without refresh | `refresh`, `ensure`, or full restart |
| "download corrupted" / invisible walls | Stream files uploaded while server running | Full restart |
| "Failed to open packfile: Invalid magic" | Resource folder name with spaces/&/brackets | Rename folder |
| Loading stuck at 76% | Entities added to an MLO ytyp | Spawn props from script instead |
| Stuck at "Nearly there" | qb-spawn restarted, multicharacter stopped | `ensure qb-multicharacter` |
| Server restarts every ~75 s | A third-party licence check can't validate | Disable that pack; never bypass |
| Escrowed resource won't start | Licence key not on buyer's Cfx account | User must own it on that account |
| Car has no textures | Truncated ytd (16777215 bytes) or missing in dump | Get a full copy |
| Lag / hitching with few players | Oversized streamed textures | Shrink ytds (assets.md) |
| Saved outfits wrong after new clothing | Global clothing index shift | Store collection + local index |

Method: change ONE thing per test, write down each test and its result (in a notes file for long hunts), tell the user exactly what to do in game and what to look for.
