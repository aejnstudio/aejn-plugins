# Scripts: QBCore, ox_lib, NUI, packaging

## Resource layout
- `fxmanifest.lua`: `fx_version 'cerulean'`, `game 'gta5'`, `lua54 'yes'`, author, version, shared `@ox_lib/init.lua` and `config.lua`, server `@oxmysql/lib/MySQL.lua`, dependencies.
- Readable names in `config.lua`, wording that matches the server's country, and a short plain-English install guide.
- Persist small admin-set data (spots, names) as JSON via `SaveResourceFile`; share it with `GlobalState` + `AddStateBagChangeHandler`.
- Admin-placed spots: a permission-checked `/xxxset <thing>` command saving where the admin stands.
- Create tables with `CREATE TABLE IF NOT EXISTS`; reuse framework tables so swaps stay compatible.

## Server-side safety
- Validate every money, item and vehicle action on the server: job and grade, on duty, distance, amounts and string lengths, cooldowns. Never trust client prices.
- Effects only one player sees should be non-networked local objects. Networked props spawned in loops hit entity limits.
- Handle `playerDropped` so a paying player is never left without the item.

## QBCore
- Keys: check which key resource the server runs before calling an export such as `exports['qb-vehiclekeys']:GiveKeys(src, plate)`.
- Banking and inventory are often replaced. Read the installed resource's exports instead of assuming the defaults.
- Restarting `qb-spawn` can stop `qb-multicharacter`: ensure it afterwards. Update that dependency when swapping spawn menus.
- `um-spawn` needs a recent ox_lib (`hasLoaded`).

## NUI
- Add `[hidden]{display:none!important}`, as other display rules override `hidden`.
- Support Tab + Enter as well as the mouse.
- YouTube refuses to embed inside `nui://` (error 153): host the player page on a website and show it through a DUI.
- Preview layouts in a normal or headless browser before testing in game.

## Lua syntax check
With Python and `lupa` installed, this compiles a file without running it:

```python
import pathlib
import re
import sys
from lupa import LuaRuntime

src = pathlib.Path(sys.argv[1]).read_text(encoding="utf-8")
src = re.sub(r"`([^`\n]*)`", r'"\1"', src)  # CfxLua hash literals
check = LuaRuntime().eval(
    'function(s) local f, e = load(s, "check", "t"); if f then return "OK" end; return e end'
)
print(check(src))
```

Other CfxLua-only syntax, such as `+=` and `?.`, shows as an error here although it runs in FiveM.

## Packaging and releases
- One complete resource folder per zip, forward-slash paths, plus the install guide.
- Only release code and assets the user wrote or is licensed to redistribute.
