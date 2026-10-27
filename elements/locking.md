---
description: Grey elements out, or show them only when another option is on.
---

# Locking and DependsOn

## Locking from code

A locked element is greyed out, ignores clicks and its keybind, and shows a short reason on the right.

```lua
local Fly = Tab:Toggle({ Title = "Fly", Flag = "Fly" })

Fly:Lock("Coming soon")
Fly:Unlock()
Fly:IsLocked()   -- true / false
```

Or start it locked:

```lua
Tab:Button({ Title = "Teleport", Locked = true, LockReason = "Join a server first" })
```

Every element supports locking, including sections, buttons and keybinds.

## DependsOn

`DependsOn` ties an element to another element's flag. By default the element is **hidden** until that flag is on.

```lua
Tab:Toggle({ Title = "Auto Farm", Flag = "AutoFarm" })

Tab:Slider({
    Title = "Farm speed",
    Min = 1, Max = 10,
    DependsOn = "AutoFarm",   -- only shown while Auto Farm is on
})
```

"On" means: `true`, a non-empty string, a number, or a multi-select dropdown with something picked.

### Options

Pass a table for more control:

| Key | Type | Description |
| --- | --- | --- |
| `Flag` | string | The flag to watch. |
| `Value` | any / function | Match this exact value, or pass `function(value) return ... end`. |
| `Lock` | boolean | Lock instead of hiding. |
| `Reason` | string | Lock text. Defaults to `"Requires <Flag>"`. |

```lua
-- shown only when Mode is "Hard"
Tab:Toggle({ Title = "Hard-mode helper", DependsOn = { Flag = "Mode", Value = "Hard" } })

-- locked (not hidden) until Auto Farm is on
Tab:Slider({
    Title = "Farm speed",
    Min = 1, Max = 10,
    DependsOn = { Flag = "AutoFarm", Lock = true, Reason = "Turn on Auto Farm" },
})

-- any condition you like
Tab:Button({
    Title = "Boost",
    DependsOn = { Flag = "WalkSpeed", Value = function(v) return v >= 50 end },
})
```

The element you depend on can be created before or after the dependent one.

## Listening to flags yourself

`DependsOn` is built on `Library:OnFlagChanged`, which you can use directly:

```lua
local stop = Library:OnFlagChanged("AutoFarm", function(value)
    print("Auto Farm is now", value)
end)

stop()   -- stop listening
```

## Which lock wins

An element can be locked for more than one reason at once. The text shown follows this order:

1. **Premium:** `Premium = true` and the user is Freemium. See [Freemium and Premium](../state/premium.md).
2. **Code:** `Lock()` or `Locked = true`.
3. **DependsOn:** with `Lock = true`.

`Unlock()` only removes a lock you made with `Lock()`. Premium and DependsOn locks stay until their condition is met.
