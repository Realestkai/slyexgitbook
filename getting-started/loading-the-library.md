---
description: One line to load the library into your script.
---

# Loading the library

## From the official URL

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Realestkai/uilib/refs/heads/main/sylex.lua"))()
```

## From the executor workspace

If `sylex.lua` is in your executor's `workspace` folder:

```lua
local Library = loadstring(readfile("sylex.lua"))()
```

## What happens on load

* **Re-running replaces the old copy.** If the library is already loaded, the old copy is unloaded first: its window is removed and its connections are disconnected. You never end up with two windows on screen.
* **Where the UI goes.** The UI goes into a `ScreenGui` named `Sylex`. It's parented to `gethui()` when the executor has it, otherwise to `CoreGui`, or to `PlayerGui` inside Roblox Studio. If `syn.protect_gui` exists, it's used too.
* **Icons.** The icon sheet is downloaded once and cached in `Sylex/cache/icons.lua`. If the download fails, the library uses the cache. If there's no cache either, it still loads, but named icons are blank.

{% hint style="info" %}
File features (configs, window position, icon cache) need the executor's `writefile` / `readfile` / `isfile`. Without them, the library still works and those features are skipped.
{% endhint %}

## Next

Now [create a window](windows.md).
