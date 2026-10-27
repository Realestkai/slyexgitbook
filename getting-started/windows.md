---
description: Set up the main window that holds your tabs.
---

# Creating a window

```lua
local Window = Library:Window({
    Title = "My Hub",
    Desc = "v1.0.0",
    Theme = "Midnight",
    Search = true,
    Config = {
        Keybind = Enum.KeyCode.LeftControl,
        Size = UDim2.new(0, 530, 0, 400),
    },
    CloseUIButton = {
        Enabled = true,
    },
    ConfigSaving = {
        Folder = "MyHub",
        AutoSave = true,
        FileName = "default",
    },
})
```

## Options

Every option is optional.

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Title` | string | `"null"` | Name shown in the top bar. |
| `Desc` | string | `""` | Small subtitle under the title. |
| `Theme` | string | `"Dark"` | Any [built-in theme](../customisation/themes.md) such as `"Midnight"`, or one added with `AddTheme`. |
| `Search` | boolean | `false` | Adds a search box to the top of every tab. Tabs can override this. |
| `Scale` | `"Auto"` / number | `"Auto"` | `"Auto"` shrinks the window to fit small screens such as phones. Pass a number like `0.8` for a fixed scale. |
| `SaveWindowState` | boolean | `true` | Remembers the window's position and size between runs. |
| `Premium` | boolean | `false` | The user's tier. See [Freemium and Premium](../state/premium.md). |
| `PremiumCheck` | string | none | URL of a tier server; sets Premium automatically. See [Freemium and Premium](../state/premium.md). |
| `PremiumRefresh` | number / `false` | `30` | Seconds between Premium re-checks while the script runs (minimum 15). `false` checks only once. |
| `Profile` | boolean | `true` | Shows the avatar, display name and tier at the bottom of the sidebar. |
| `Debug` | boolean | `false` | Turns on [debug mode](../reference/debug-mode.md). |
| `Config` | table | `{}` | `Keybind` and `Size`, see below. |
| `CloseUIButton` | table | disabled | A floating open/close button for mobile players, see below. |
| `ConfigSaving` | table | `{}` | Where configs go and whether to autosave. See [Flags and configs](../state/flags-and-configs.md). |

### `Config`

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Keybind` | `Enum.KeyCode` | `LeftControl` | Hides and shows the window. Ignored while typing in a textbox. |
| `Size` | `UDim2` | `530 × 400` | Starting size. |

### `CloseUIButton`

| Option | Type | Description |
| --- | --- | --- |
| `Enabled` | boolean | Show the button. |
| `Text` | string | Fallback label, shown only if the logo can't load. Defaults to `"Open"`. |

The window title and the open button always show the Sylex logo; they can't be changed. The logo is downloaded once and cached in `Sylex/assets`. On executors without `getcustomasset`, the title shows a plain icon and the button shows `Text`.

## What the player can do

| Control | Action |
| --- | --- |
| Drag the top bar | Move the window |
| Drag the bottom-right corner | Resize (minimum 450 × 220) |
| First top-bar button | Hide the window (same as the keybind) |
| Second top-bar button | Maximise / restore |
| ✕ button | Asks to confirm, then [unloads](../state/unloading.md) the UI |

Any open dropdown or colour picker closes when the window is moved, resized, hidden or switched to another tab.

## Window methods

```lua
Window:SelectTab(2)                       -- open the 2nd tab (works before or after load)
Window:Line()                             -- divider between tabs in the sidebar
Window:Toggle()                           -- hide / show
Window:IsVisible()                        -- true when shown
Window:SetKeybind(Enum.KeyCode.RightShift)
Window:GetKeybind()
Window:SetTheme("Midnight")
Window:GetTheme()
Window:SetAccent(Color3.fromRGB(255, 90, 140))
Window:Destroy()                          -- same as Library:Unload()
```

The window also has:

* `Notify` and `Dialog`: see [Notifications](../feedback/notifications.md) and [Dialogs](../feedback/dialogs.md).
* `SaveConfig`, `LoadConfig`, `DeleteConfig` and `ListConfigs`: see [Flags and configs](../state/flags-and-configs.md).
* `SettingsTab`, `Watermark` and `KeybindList`: see the [Customisation](../customisation/settings-tab.md) pages.

{% hint style="info" %}
The library doesn't show any notification of its own. To tell players which key opens the menu, send one with `Window:Notify`.
{% endhint %}
