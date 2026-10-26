---
description: Tabs group your elements into pages in the sidebar.
---

# Tabs

```lua
local Main = Window:Tab({
    Title = "Main",
    Icon = "house",
})
```

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Title` | string | `"null"` | Text in the sidebar. |
| `Icon` | string / number | `"house"` | An icon name from the library's icon set, or an asset ID. |

`Window:Tab` returns a **tab object**. Every element is created on a tab:

```lua
Main:Button({ ... })
Main:Toggle({ ... })
```

Elements appear in the order you create them, and the page scrolls once it's full.

{% hint style="info" %}
**Icons.** Names are looked up in the library's icon sheet. A plain number like `"1234567"` is treated as `rbxassetid://1234567`. If a name isn't in the sheet the icon shows up blank, so swap it for one that exists or use an asset ID.
{% endhint %}

## Organising a tab

Use a [Section](../elements/text.md#section) as a heading inside a tab, and [`Window:Line()`](windows.md#add-a-divider-between-tabs) to separate groups of tabs in the sidebar.

```lua
local Player = Window:Tab({ Title = "Player", Icon = "user" })

Player:Section({ Title = "Movement" })
Player:Slider({ Title = "WalkSpeed", Min = 16, Max = 100, Value = 16 })

Player:Section({ Title = "Visuals" })
Player:Toggle({ Title = "Fullbright" })
```
