# Button Forge for Conquest of Azeroth

Button Forge 0.9.4 (WotLK 3.3.5a) with a fix that lets it start up on the **Conquest of Azeroth** / Ascension client.

This is an unofficial compatibility fix. Button Forge is by Massiner of Nathrezim; all credit for the addon goes to them.

## Install

1. Close the game.
2. Download this repository (Code → Download ZIP).
3. Copy the `ButtonForge` folder into `<your client>\Interface\AddOns\`, replacing any existing copy.

## The problem

Every Button Forge bar and config button errored with `attempt to index global 'ButtonForgeSave' (a nil value)` (`UILibLayers.lua:17`, `Util.lua:188`, `UILibToolbar.lua:89`).

At login, Button Forge caches your mounts and pets, and gave up on the first one with no name. Ascension's mount collection has some nameless entries (21 of 861 on one character), so Button Forge never finished starting up and never created its saved settings.

To check your own character:

```
/run for _,t in ipairs({"CRITTER","MOUNT"}) do local n,b=GetNumCompanions(t),0 for i=1,n do local _,nm=GetCompanionInfo(t,i) if not nm then b=b+1 end end print(t,n,"no name",b) end
```

## The fix

`Util.CacheCompanions` in `ButtonForge/Util.lua` now skips nameless companions, which couldn't be put on a bar anyway. If **most** names are missing, it still waits and retries like the original, since that means the companion data hasn't loaded yet.

## Credits

Button Forge by Massiner of Nathrezim.

Button Forge ships without a license file, so its author keeps their rights to it. This repository only exists to share the CoA compatibility fix. If you're the author and want it taken down, please open an issue.
