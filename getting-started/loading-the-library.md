---
description: One line to load the library into your script.
---

# Loading the library

## From a URL

Host `SylexUI` somewhere that serves raw text (a GitHub repo's raw link works well), then load it:

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Realestkai/uilib/refs/heads/main/sylex"))()
```

## Next

Now [create a window](windows.md).
