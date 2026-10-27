---
description: Labels, paragraphs, dividers, progress bars, images and code blocks.
---

# Text and display elements

For sections, see [Tabs and sections](../getting-started/tabs.md#sections).

## Label

A read-only row with a title and an optional description.

```lua
local Status = Tab:Label({
    Title = "Status: Idle",
    Desc = "Updates every second",
})

Status:SetTitle('Status: <font color="#4868D6">Running</font>')
Status:SetDesc("Started 5s ago")
```

***

## Paragraph

A label meant for longer text. The content wraps over as many lines as it needs.

```lua
local About = Tab:Paragraph({
    Title = "How it works",
    Content = "Turn on Auto Farm, pick a target, and leave the game running.\nThe counter below updates after every run.",
})

About:SetContent("New text")
```

***

## Divider

A thin line between elements.

```lua
Tab:Divider()
```

***

## Progress bar

Shows progress towards a maximum. It's display-only, so it isn't saved in configs.

```lua
local Progress = Tab:ProgressBar({
    Title = "Fossils this session",
    Max = 50,
    Value = 0,
    Suffix = "fossils", -- leave out to show a percentage
})

Progress:Increment()     -- +1
Progress:Increment(5)    -- +5
Progress:Set(20)
Progress:SetMax(100)
```

***

## Image

```lua
local Banner = Tab:Image({
    Image = 111362591084511,
    Height = 150,
    ScaleType = "Crop", -- "Crop", "Fit", "Stretch"
    CornerRadius = 6,
})

Banner:SetImage("rbxassetid://123456")
Banner:SetHeight(200)
```

***

## Code

A syntax-highlighted Lua block with line numbers and a **Copy** button.

```lua
local Code = Tab:Code({
    Title = "Loader",
    Code = 'loadstring(game:HttpGet("https://raw.githubusercontent.com/Realestkai/uilib/refs/heads/main/sylex.lua"))()',
})

Code:SetCode('print("Updated")')
Code:GetCode()
```

Multi-line strings and `--[[ block comments ]]` are highlighted properly. The Copy button uses the executor's `setclipboard` (or `toclipboard`). If neither exists, it prints a warning instead of erroring.
