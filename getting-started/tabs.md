---
description: Tabs group your elements into pages; sections group them inside a tab.
---

# Tabs and sections

## Tabs

```lua
local Main = Window:Tab({
    Title = "Main",
    Icon = "house",
    Search = true,
})
```

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Title` | string | `"null"` | Text in the sidebar. |
| `Icon` | string / number | `"house"` | An icon name or an asset ID. |
| `Search` | boolean | window's `Search` | Adds a search box at the top of this tab. |

`Window:Tab` returns a **tab object**. Every element is created on a tab, and elements appear in the order you create them.

```lua
Main:Select()             -- switch to this tab
Main:SetTitle("Home")     -- rename it in the sidebar
```

{% hint style="info" %}
**Icons.** Icon names are looked up in the library's icon sheet. A number, or a string of digits, is treated as an asset ID, and full `rbxassetid://` links work too. Unknown names show a blank icon instead of breaking.
{% endhint %}

### Search

With search on, typing filters the tab's elements by their title and description. Section headings are hidden while searching, and elements inside collapsed sections are searched too.

***

## Sections

A section is a heading. Every element created after it belongs to it, up to the next section.

```lua
local Movement = Player:Section({ Title = "Movement" })
```

### Collapsible sections

```lua
local Advanced = Player:Section({
    Title = "Advanced",
    Collapsible = true,
    Collapsed = true, -- start closed
})

Player:Slider({ Title = "Gravity", Min = 0, Max = 400, Value = 196 })
Player:Toggle({ Title = "No clip" })
```

Clicking the heading opens or closes it. Passing `Collapsed` also makes the section collapsible.

| Option | Type | Default |
| --- | --- | --- |
| `Title` | string | `"null"` |
| `Collapsible` | boolean | `false` |
| `Collapsed` | boolean | `false` |

```lua
Advanced:SetTitle("Advanced options")
Advanced:SetCollapsed(false)
Advanced:IsCollapsed()
```

Sections also have the [shared methods](../elements/shared.md) `SetVisible` and `Destroy`. Note that `Destroy` removes only the heading, not the elements under it.
