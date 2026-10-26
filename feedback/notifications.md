---
description: Small toasts in the bottom-right corner.
---

# Notifications

```lua
Window:Notify({
    Title = "Saved",
    Desc = "Your settings were saved.",
    Time = 5,
})
```

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `Title` | string | `"null"` | Bold first line. RichText supported. |
| `Desc` | string | `""` | Second line. Hidden when empty. RichText supported. |
| `Time` | number | `5` | Seconds before it disappears. A bar along the bottom counts down. |

Notifications stack upwards from the bottom-right of the screen, and they're themed like the rest of the window.

{% hint style="warning" %}
`Notify` **yields** for about 0.15 seconds while it animates in. If you call it in the middle of other code, wrap it so the rest of your code doesn't wait:

```lua
task.spawn(function()
    Window:Notify({ Title = "Hi" })
end)
```
{% endhint %}

## RichText example

```lua
Window:Notify({
    Title = '<font color="#4868D6">Midnight Hub</font>',
    Desc = "Press <b>Left Ctrl</b> to hide the menu.",
    Time = 8,
})
```
