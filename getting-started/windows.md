---
description: Set up the main window that holds your tabs.
---

# Creating a window

```lua
local Window = Library:Window({
    Title = "Sylex",
    Desc = "v1.0.0",
    Icon = "door-open",
    Theme = "Dark",
    Config = {
        Keybind = Enum.KeyCode.LeftControl,
        Size = UDim2.new(0, 530, 0, 400),
    },
    CloseUIButton = {
        Enabled = true,
        Text = "Open",
    },
})
```

## Options

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Title` | string | `"null"` | Name shown in the top bar. |
| `Desc` | string | `""` | Small subtitle under the title. Hidden when empty. |
| `Icon` | string / number | `"door-open"` | An icon name from the library's icon set, or an asset ID. |
| `Theme` | string | `"Dark"` | `"Dark"` or `"Midnight"`. Unknown names fall back to `"Dark"`. |
| `Config` | table | **required** | Window settings (see below). |
| `CloseUIButton` | table | **required** | Floating open/close button (see below). |

### `Config`

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Keybind` | `Enum.KeyCode` | `LeftControl` | Hides and shows the window. Ignored while a textbox is focused. |
| `Size` | `UDim2` | `UDim2.new(0, 530, 0, 400)` | Starting window size. |

### `CloseUIButton`

A small draggable button that toggles the window, mainly for mobile players who have no keyboard.

| Option | Type | Description |
| --- | --- | --- |
| `Enabled` | boolean | Show the button. |
| `Text` | string | Label on the button. |

{% hint style="danger" %}
`Config` and `CloseUIButton` must both be passed, even if they're empty tables. Leaving either out throws an error.
{% endhint %}

## What the player can do

| Control | Action |
| --- | --- |
| Drag the top bar | Move the window |
| Drag the bottom-right corner | Resize (minimum 450 × 220) |
| First top-bar button | Hide the window (same as the keybind) |
| Second top-bar button | Maximise / restore |
| ✕ button | Asks to confirm, then destroys the whole UI |
| Theme dropdown in the top bar | Switch between Dark and Midnight |

The first time the window is hidden, a notification tells the player which key brings it back.

## Window methods

### Pick the starting tab

```lua
Window:SelectTab(2) -- opens the second tab on load
```

Call this right after creating your tabs. Out-of-range numbers fall back to tab 1.

### Add a divider between tabs

```lua
Window:Tab({ Title = "Main", Icon = "house" })
Window:Line()
Window:Tab({ Title = "Settings", Icon = "settings" })
```

The window object also has `Notify` and `Dialog`. See [Notifications](../feedback/notifications.md) and [Dialogs](../feedback/dialogs.md).
