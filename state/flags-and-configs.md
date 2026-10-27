---
description: Read element values from anywhere, and save them between sessions.
---

# Flags and configs

## Flags

Give an element a `Flag` and its current value is always available in `Library.Flags`:

```lua
Tab:Toggle({ Title = "Auto Farm", Flag = "AutoFarm" })
Tab:Slider({ Title = "Speed", Min = 16, Max = 100, Flag = "Speed" })

task.spawn(function()
    while task.wait(1) do
        if Library.Flags.AutoFarm then
            print("Farming at speed", Library.Flags.Speed)
        end
    end
end)
```

`Library.Options` holds the element objects themselves, so you can control them from anywhere:

```lua
Library.Options.AutoFarm:Set(false)
```

{% hint style="warning" %}
Every flag must be unique. Two elements with the same flag overwrite each other.
{% endhint %}

## Saving and loading configs

Configs are JSON files in the config folder (`Sylex/configs` by default). Only elements with a `Flag` are saved.

```lua
Library:SaveConfig("legit")      -- writes Sylex/configs/legit.json
Library:LoadConfig("legit")      -- applies it; returns false if it doesn't exist
Library:DeleteConfig("legit")
Library:ListConfigs()            -- { "default", "legit" }
```

These are also available on the window: `Window:SaveConfig(...)` and so on.

**Call `LoadConfig` at the end of your script**, after every element has been created:

```lua
-- ... all your tabs and elements ...

Library:LoadConfig()   -- no name: loads the autoload config, or "default"
```

Loading runs each element's callback with the saved value, so your script picks the settings up as if the player had changed them.

### Autoload

`Library:LoadConfig()` with no name loads the config chosen as **autoload**, or `"default"` if none has been chosen. Players can pick an autoload config from the [settings tab](../customisation/settings-tab.md).

### Autosave

```lua
Library:Window({
    ConfigSaving = {
        Folder = "MyHub",       -- configs go in MyHub/
        AutoSave = true,        -- save automatically after changes
        FileName = "default",   -- which config autosave writes to
    },
})
```

Autosave waits one second after the last change before writing. It only starts after the first `LoadConfig` or `SaveConfig` call, so the starting values your elements report on creation never overwrite a saved config.

## Sharing configs

Players can share their settings as a single line of text that starts with `SYLEX1:`. The [settings tab](../customisation/settings-tab.md) has buttons for this, or you can call it yourself:

```lua
local code = Library:ExportConfig()   -- returns the code and copies it to the clipboard
Library:ImportConfig(code)            -- true if it worked, false if the code is invalid
```

Importing works like `LoadConfig`. Flags the code doesn't mention are left alone, and Premium features stay locked for Freemium users.

## Reacting to a flag

```lua
local stop = Library:OnFlagChanged("AutoFarm", function(value)
    print("Auto Farm:", value)
end)
```

See [Locking and DependsOn](../elements/locking.md) for showing or locking elements based on a flag.

## What gets saved

| Element | Saved as |
| --- | --- |
| Toggle | `true` / `false` |
| Slider | number |
| RangeSlider | `[min, max]` |
| Dropdown | string, or list of strings |
| Textbox | string or number |
| Keybind | `{ "Key": "E", "Value": true }` |
| ColorPicker | `"#RRGGBB"` |

Progress bars are never saved. To leave any other element out, set `Element.NoSave = true`.

{% hint style="info" %}
Config features need the executor's file functions (`writefile`, `readfile`, `isfile`, plus `listfiles` and `delfile` for listing and deleting). Without them, the save functions return `false` and the rest of the library carries on working.
{% endhint %}
