---
name: fivem-dev
description: FiveM and GTA V server development - MLOs and map edits (ymap/ytyp/ybn/ydr), CodeWalker and Blender+Sollumz pipelines, textures and lag (ytd), add-on clothing/EUP, vehicles, QBCore/ox_lib scripts, NUI, packaging, and crash debugging. Use when the work touches a FiveM resource, a GTA asset file, an MLO, a map, a car or clothing pack, or a FiveM crash.
---

# FiveM development

These lessons come from real crashes on live roleplay servers. They are field experience, so verify on a test server.
Pick the reference for the job and read it first:

| Job | Read |
|---|---|
| MLO, interior, building cut, ymap/ytyp/ybn, removing world props, signs | `references/maps-mlo.md` |
| Textures, ytd, lag, cars, clothing/EUP, models, Blender/Sollumz, CodeWalker from PowerShell | `references/assets.md` |
| Lua resources, QBCore, ox_lib, NUI, packaging, releases | `references/scripts.md` |
| Crash, white box, "download corrupted", stuck loading, missing textures | `references/debugging.md` |

## Ground rules

- **No leaked, cracked or dumped paid assets.** Escrowed packs only run on the buyer's Cfx licence. Don't help bypass licence checks: explain, and suggest a licensed or free replacement. For free assets, respect the licence and keep the credits.
- **Don't restart a live server without the user's OK.** Restarting map or stream resources with players in game can crash clients. After changing `files {}` or stream files use `refresh` + `ensure` on an empty server, or a full restart.
- **Never print secrets** such as FTP logins, licence keys, bot tokens and database strings.
- **Ask before heavy work** such as renders or batch texture jobs on a PC that may be running the game.
- **Zips:** use forward-slash paths. Windows PowerShell 5 `Compress-Archive` writes backslashes, so use another tool. Ship one complete resource per zip.
- **Resource folder names:** no spaces, `&`, or extra brackets. `[category]` folders are fine.
- **Back up before overwriting** server files, and get the user's OK before changing a live server.
- **Check Lua syntax before upload** (see `references/scripts.md`).
- **Look at your own render before asking the user to test.** Check poses, props and clothing on a real ped body.
- Say plainly what is untested in game.

## Find out before starting

Ask only for what the job needs and you don't already have:

- GTA V install path and the server's game build
- Framework (QBCore, Qbox, ESX or standalone) and inventory
- Whether the server is local, a test server or live
- Tool paths for CodeWalker, Blender and Sollumz

Logs: client in `%LOCALAPPDATA%\FiveM\FiveM.app\logs`; server in the txAdmin console and `fxserver.log`.
