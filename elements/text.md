---
description: Sections, labels and code blocks.
---

# Text elements

## Section

A heading that groups the elements below it.

```lua
local Section = Tab:Section({
    Title = "Movement",
})

Section:SetTitle("Movement Settings")
```

***

## Label

A read-only row with a title and an optional description. Useful for status text, credits or instructions.

```lua
local Label = Tab:Label({
    Title = "Status: Idle",
    Desc = "Updates every second",
    Image = "",
})
```

| Option | Type | Default |
| --- | --- | --- |
| `Title` | string | `"null"` |
| `Desc` | string | `""` |
| `Image` | string / number | `""` |

### Update a label

```lua
Label:SetTitle("Status: Running")
Label:SetDesc("Started 5s ago")
Label:SetVisible(true)
```

Labels support RichText, so you can colour parts of the text:

```lua
Label:SetTitle('Status: <font color="#4868D6">Running</font>')
```

***

## Code

A syntax-highlighted Lua block with line numbers and a **Copy** button.

```lua
local Code = Tab:Code({
    Title = "Loader",
    Code = 'print("Hello World")',
})
```

| Option | Type | Default |
| --- | --- | --- |
| `Title` | string | `"null"` |
| `Code` | string | `'-- print("Hello World")'` |

The block grows to fit the code and scrolls sideways for long lines.

### Update a code block

```lua
Code:SetTitle("New title")
Code:SetCode('print("Updated")')
```

{% hint style="info" %}
The Copy button uses the executor's `setclipboard`. In plain Studio it throws an error.
{% endhint %}
