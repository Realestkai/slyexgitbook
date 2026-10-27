---
description: Print what the library is doing while you build a script.
---

# Debug mode

Turn it on while you're building a script:

```lua
local Window = Library:Window({ Title = "Sylex", Debug = true })
-- or at any time:
Library:SetDebug(true)
```

The executor console then shows lines starting with `[Sylex debug]` for:

* **Flag changes:** `flag AutoFarm = true`
* **Configs:** saving, loading, importing and exporting, and why a load or import failed.
* **Tier changes:** `tier = Premium`
* **Locks:** `Toggle Auto Farm locked (premium)`, `... unlocked`
* **Broken DependsOn:** a warning when an element depends on a flag that no element has, usually a typo.

**Callback errors get a full traceback** with line numbers, instead of the one-line warning.

## Always on

Some warnings print even with debug mode off:

* `[Sylex] callback error: ...` when a callback errors.
* `[Sylex] two elements use the flag 'X'` when two elements share a flag. Only the newer one is saved in configs, so rename one.

{% hint style="info" %}
Turn debug mode off before you ship a script. Players don't need the extra console output.
{% endhint %}
