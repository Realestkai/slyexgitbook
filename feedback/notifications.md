---
description: Toasts in the bottom-right corner.
---

# Notifications

```lua
Window:Notify({
    Title = "Saved",
    Desc = "Your settings were saved.",
    Type = "Success",
    Time = 5,
})
```

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Title` | string | `"null"` | Bold first line. RichText supported. |
| `Desc` | string | `""` | Second line, hidden when empty. RichText supported. `Content` is an alias. |
| `Type` | string | `"Info"` | `"Info"`, `"Success"`, `"Warning"` or `"Error"`. Sets the colour of the side stripe and countdown bar. Info uses the theme's accent colour. |
| `Time` | number | `5` | Seconds before it disappears. |

* Each notification has a **×** button that closes it early.
* `Notify` **returns immediately**, so you can call it anywhere without your code waiting.
* `Library:Notify({...})` does the same thing, if you don't have the window object to hand.

## Examples

```lua
Window:Notify({ Title = "Auto Farm", Desc = "Started.", Type = "Success" })
Window:Notify({ Title = "Low health", Desc = "Retreating...", Type = "Warning", Time = 3 })
Window:Notify({ Title = "Remote missing", Desc = "This game updated.", Type = "Error", Time = 8 })

Window:Notify({
    Title = '<font color="#4868D6">Sylex</font>',
    Desc = "Press <b>Left Alt</b> to hide the menu.",
    Time = 8,
})
```
