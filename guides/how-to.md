---
description: Step-by-step recipes for the things you'll do in almost every script.
---

# Step-by-step guides

Each guide is short and copy-pasteable. For a whole script that combines them, see the [full example](../reference/full-example.md).

* [Start a new script](#start-a-new-script)
* [Add tabs and sections](#add-tabs-and-sections)
* [Add a feature that runs a loop](#add-a-feature-that-runs-a-loop)
* [Make a feature Premium only](#make-a-feature-premium-only)
* [Turn on Premium from the Discord role](#turn-on-premium-from-the-discord-role)
* [Show options only when a feature is on](#show-options-only-when-a-feature-is-on)
* [Lock something for now](#lock-something-for-now)
* [Save the player's settings](#save-the-players-settings)
* [Tell the player something](#tell-the-player-something)
* [Ask the player to confirm](#ask-the-player-to-confirm)
* [Clean up when the UI closes](#clean-up-when-the-ui-closes)
* [Find out why something doesn't work](#find-out-why-something-doesnt-work)
* [Checklist before you ship](#checklist-before-you-ship)

***

## Start a new script

Every script follows the same order: **load → window → tabs → elements → settings tab → loops → load config**.

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Realestkai/uilib/refs/heads/main/sylex.lua"))()

local Window = Library:Window({
    Title = "Sylex",
    Desc = "Game name",
    Theme = "Midnight",
    PremiumCheck = "https://sylex-premium.sylex.workers.dev/tier",
    Config = { Keybind = Enum.KeyCode.LeftAlt },
    CloseUIButton = { Enabled = true },
    ConfigSaving = { Folder = "Sylex-GameName", AutoSave = true },
})

local Main = Window:Tab({ Title = "Main", Icon = "house" })

-- your elements here

Window:SettingsTab()
Library:LoadConfig()   -- always last
```

* Give every script its own `ConfigSaving.Folder`, so two scripts' settings don't mix.
* The Sylex logo in the title and on the open button is fixed and can't be changed.

## Add tabs and sections

```lua
local Main = Window:Tab({ Title = "Main", Icon = "house" })
local Player = Window:Tab({ Title = "Player", Icon = "user" })
Window:Line()                                  -- divider in the sidebar
local Misc = Window:Tab({ Title = "Misc", Icon = "settings" })

Main:Section({ Title = "Farming" })            -- heading; elements below belong to it
Main:Section({ Title = "Advanced", Collapsed = true })   -- starts folded; click to open
```

Icons are names like `"house"`, `"user"` and `"settings"`, or a Roblox asset ID. An unknown name shows a blank icon.

## Add a feature that runs a loop

Use a **flag** and **one loop** that reads it. Don't start a new loop inside the callback, or toggling quickly starts several loops.

```lua
Main:Toggle({ Title = "Auto Farm", Flag = "AutoFarm" })

task.spawn(function()
    while not Library.Unloaded do        -- stops when the UI is unloaded
        if Library.Flags.AutoFarm then
            -- one step of farming
        end
        task.wait(0.5)
    end
end)
```

`Library.Flags.AutoFarm` is always the toggle's current value, including after a config loads.

## Make a feature Premium only

1. Add `Premium = true` to the element:

   ```lua
   Main:Toggle({ Title = "Auto Collect", Flag = "AutoCollect", Premium = true })
   ```
2. Make sure the callback with `false` really **stops** the feature. If a player's Premium ends while the script is open, Sylex switches the toggle off for them.
3. Read it the same way as any flag: `if Library.Flags.AutoCollect then ... end`.

Freemium players see it greyed out with a gold crown and **Premium only**. See [Freemium and Premium](../state/premium.md) for the details.

## Turn on Premium from the Discord role

1. Add `PremiumCheck` to the window:

   ```lua
   PremiumCheck = "https://sylex-premium.sylex.workers.dev/tier",
   ```
2. Add `Window:SettingsTab()`. It contains the **Link Discord** button.
3. That's it. Players link once (**Link Discord** → send `/link CODE` in Discord), and Premium follows their role. Role changes apply within about 30 seconds, even while the script is open.

To test it yourself: link your own account, give yourself the Premium role, and wait for the card to turn gold. Remove the role and it turns grey again, with a "Premium ended" notification.

## Show options only when a feature is on

```lua
Main:Toggle({ Title = "Auto Farm", Flag = "AutoFarm" })

Main:Slider({
    Title = "Farm speed",
    Min = 1, Max = 10,
    DependsOn = "AutoFarm",          -- hidden until Auto Farm is on
})

Main:Toggle({
    Title = "Hard-mode helper",
    DependsOn = { Flag = "Mode", Value = "Hard" },   -- only for one dropdown value
})

Main:Button({
    Title = "Boost",
    DependsOn = { Flag = "AutoFarm", Lock = true, Reason = "Turn on Auto Farm" },   -- locked instead of hidden
})
```

See [Locking and DependsOn](../elements/locking.md).

## Lock something for now

```lua
local Event = Main:Button({ Title = "Event Teleport", Locked = true, LockReason = "Coming soon" })

-- later, for example when the event starts:
Event:Unlock()
```

## Save the player's settings

1. Give every element you want saved a **unique** `Flag`.
2. Set `ConfigSaving = { Folder = "Sylex-GameName", AutoSave = true }` on the window.
3. Call `Library:LoadConfig()` **after** creating every element.

Players can also save named configs, set one to load automatically, and share settings as a code from the settings tab. See [Flags and configs](../state/flags-and-configs.md).

{% hint style="warning" %}
Two elements with the same flag print a warning. Only one of them is saved, so give each element its own flag.
{% endhint %}

## Tell the player something

```lua
Window:Notify({ Title = "Done", Desc = "Collected 20 coins.", Type = "Success", Time = 4 })
```

`Type` is `"Info"`, `"Success"`, `"Warning"` or `"Error"`. See [Notifications](../feedback/notifications.md).

## Ask the player to confirm

```lua
Window:Dialog({
    Title = "Reset all settings?",
    Content = "This can't be undone.",
    Buttons = {
        { Title = "Reset", Color = Color3.fromRGB(226, 39, 6), Callback = function()
            -- reset here
        end },
        { Title = "Cancel" },
    },
})
```

See [Dialogs](../feedback/dialogs.md).

## Clean up when the UI closes

When a player unloads the UI, Sylex removes the window and its connections, but **your** changes to the game stay unless you undo them:

```lua
Library:OnUnload(function()
    getHumanoid().WalkSpeed = 16     -- put things back
end)
```

And every loop checks `Library.Unloaded`, as in [Add a feature that runs a loop](#add-a-feature-that-runs-a-loop). See [Unloading](../state/unloading.md).

## Find out why something doesn't work

1. Add `Debug = true` to the window, or call `Library:SetDebug(true)`.
2. Run the script and open the console (**F9**, or your executor's console).
3. Lines starting with `[Sylex debug]` show every flag change, config load, tier check and lock. Callback errors show the full traceback with line numbers.

Common problems:

| Symptom | Cause |
| --- | --- |
| `[Sylex] callback error: ...` | Your callback errored; the message says where. |
| An element with `DependsOn` never appears | The flag name is misspelled. Debug mode warns: *depends on flag X but no element has that flag*. |
| Settings don't come back after rejoining | `LoadConfig()` ran before the elements were created, or the elements have no `Flag`. |
| Premium doesn't unlock | The player hasn't linked. The card says **Link Discord**. Or they don't have the role; ask them to run `/linked`. |

See [Debug mode](../reference/debug-mode.md).

## Checklist before you ship

* [ ] `Debug = true` is removed.
* [ ] Every saved element has a unique `Flag`.
* [ ] `Library:LoadConfig()` is the last line.
* [ ] Every loop checks `Library.Unloaded`.
* [ ] `Library:OnUnload` puts back anything your script changed in the game.
* [ ] Premium features have `Premium = true`, and their callback with `false` stops them.
* [ ] `Window:SettingsTab()` is added.
* [ ] `ConfigSaving.Folder` is unique to this script.
* [ ] You tested it as Freemium **and** as Premium.
