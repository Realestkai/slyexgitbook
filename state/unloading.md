---
description: Removing the UI cleanly and stopping your script with it.
---

# Unloading

```lua
Library:Unload()
-- or
Window:Destroy()
```

Unloading:

1. Runs every `OnUnload` callback.
2. Saves the window's position and size.
3. Disconnects every input, render and player connection the library made.
4. Stops element callbacks from firing.
5. Removes the UI.

The ✕ button in the window, and **Unload UI** in the [settings tab](../customisation/settings-tab.md), both unload the library.

## Stopping your own loops

The library can't stop loops your script started. Use `OnUnload`, or check `Library.Unloaded`:

```lua
Library:OnUnload(function()
    -- turn features off, restore WalkSpeed, remove ESP, etc.
    LocalPlayer.Character.Humanoid.WalkSpeed = 16
end)

task.spawn(function()
    while not Library.Unloaded do
        -- farm
        task.wait(1)
    end
end)
```

## Re-executing

Running a script that loads the library again unloads the previous copy first, so players never end up with two windows. Your script still needs its own guard if its loops shouldn't run twice. The simplest guard is the `Library.Unloaded` check above.
