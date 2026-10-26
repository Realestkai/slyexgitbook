---
description: One line to load the library into your script.
---

# Loading the library

## From a URL

Host `DummyKawaii_Midnight.lua` somewhere that serves raw text (a GitHub repo's raw link works well), then load it:

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/YOUR_NAME/YOUR_REPO/main/DummyKawaii_Midnight.lua"))()
```

## From the executor workspace

If the file is in your executor's `workspace` folder:

```lua
local Library = loadstring(readfile("DummyKawaii_Midnight.lua"))()
```

{% hint style="warning" %}
The library downloads its icon sheet with `game:HttpGet` and `loadstring` when it loads. Normal game scripts in Roblox Studio can't use either, so the library is meant to run under an executor.
{% endhint %}

The UI is parented to `CoreGui`, or to `PlayerGui` when running inside Studio.

{% hint style="info" %}
Load the library **once** per script. Each load creates a new `ScreenGui` called `Dummy Kawaii`.
{% endhint %}

## Next

Now [create a window](windows.md).
