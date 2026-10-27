---
description: Lock features to Premium users, and give Premium automatically from a Discord role.
---

# Freemium and Premium

Every player is either **Freemium** (the default) or **Premium**.

* Elements made with `Premium = true` are **locked for Freemium players**. They're greyed out with a gold crown and **Premium only**, and can't be clicked or triggered with a keybind.
* The **profile card** at the bottom of the sidebar shows the player's avatar, display name and tier.
* With `PremiumCheck`, players get Premium **automatically from the Premium role** in the Sylex Discord server.

## Quick start

For most scripts, this is all you need:

```lua
local Window = Library:Window({
    Title = "My Script",
    PremiumCheck = "https://sylex-premium.sylex.workers.dev/tier",
})

local Main = Window:Tab({ Title = "Main", Icon = "house" })

Main:Toggle({ Title = "Auto Farm", Flag = "AutoFarm" })            -- everyone
Main:Toggle({ Title = "Auto Collect", Flag = "AutoCollect", Premium = true })   -- Premium only

Window:SettingsTab()   -- includes the Link Discord button
```

## Marking Premium features

Add `Premium = true` to any element: toggles, buttons, sliders, dropdowns, keybinds, sections, and so on.

```lua
Main:Slider({
    Title = "Farm speed",
    Min = 1, Max = 10,
    Premium = true,
    PremiumReason = "Premium: faster farming",   -- optional; default "Premium only"
})
```

What happens for a Freemium player:

| Situation | Behaviour |
| --- | --- |
| Clicking the element | Nothing; the lock blocks it. |
| Its keybind | Ignored. |
| Loading a config, or importing a config code from a Premium player | Premium elements are skipped, so they can't be switched on that way. |
| Premium ends while the script is open | Premium toggles that were on are switched **off** (their callback runs with `false`), then locked. |

{% hint style="info" %}
Write Premium toggle callbacks so that `false` really stops the feature. That's what runs when someone's Premium ends mid-session.
{% endhint %}

## Premium from a Discord role

### How it works

```
Script ── "is Roblox user 123 Premium?" ──▶ Sylex Premium worker ──▶ Discord: does the linked member have the role?
       ◀──────────── { premium: true } ────────────────┘
```

1. A player **links** their Roblox account to their Discord account once (below).
2. The script asks the Sylex Premium worker about the player when it starts, then **every 30 seconds** while it's open.
3. The worker checks the linked Discord member's roles **live**, so giving or removing the role takes effect within about 30 seconds.
4. When the tier changes, the profile card updates, Premium elements lock or unlock, and the player gets a **"Premium unlocked"** or **"Premium ended"** notification.

### Turning it on

```lua
local Window = Library:Window({
    Title = "My Script",
    PremiumCheck = "https://sylex-premium.sylex.workers.dev/tier",
    PremiumRefresh = 30,   -- optional: seconds between re-checks (default 30, minimum 15, false = only at start)
})
```

Add `Window:SettingsTab()` so players have the **Link Discord** button.

### How players link (send this to players)

1. Open the script and click **Link Discord**, either on the profile card at the bottom-left or in **Settings → Premium**.
2. A code appears, and `/link ABC-234` is copied to your clipboard. The code works for 10 minutes.
3. In the Sylex Discord server, paste and send it. The bot replies (only you can see it): *Linked to YourName*.
4. Press **I've linked it** in the script. If you have the Premium role, the card turns gold.

They only do this once. After that, Premium follows their Discord role automatically.

### Rules

| Rule | Why |
| --- | --- |
| One Roblox account per Discord account | One Premium role = one Premium Roblox account. |
| Switching to a different Roblox account has a **7-day cooldown** | Stops one buyer passing Premium around to friends. |
| A Roblox account can only be linked to one Discord account | Someone can't hijack another player's link. |
| Leaving the Discord server removes Premium | The worker can't see the role any more. |

### Discord commands

| Command | Who | What it does |
| --- | --- | --- |
| `/link <code>` | Everyone | Links the Roblox account that made the code. |
| `/linked` | Everyone | Shows your linked Roblox account and tier. |
| `/unlink @member` | Admins | Removes a member's link and resets their 7-day cooldown. Use it when someone needs to switch accounts early, or when a Roblox account is stuck linked to the wrong Discord account. |

The Sylex bot shows as **offline** in Discord. That's normal; it answers commands without staying connected.

## Setting the tier yourself

You can also set the tier from your own code, without the worker:

```lua
local Window = Library:Window({ Title = "My Script", Premium = true })

Library:SetPremium(true)   -- change at any time; everything updates straight away
Library:SetPremium(false)
```

## Reading the tier

```lua
Library:IsPremium()      -- true / false
Library:GetTier()        -- "Premium" or "Freemium"
Library.Linked           -- true once this Roblox account is linked (only with PremiumCheck)

Library:CheckPremium()   -- ask the worker again now; yields, returns true / false
Library:GetLinkCode()    -- a fresh link code, or nil and an error message
Window:LinkDiscord()     -- opens the Link Discord dialog
```

To run your own code when the tier changes:

```lua
Library:OnPremiumChanged(function(isPremium)
    if not isPremium then
        stopPremiumLoops()   -- your own cleanup
    end
end)
```

It runs once with the first answer, then only when the tier actually changes, not on every 30-second check. It returns a function that stops listening.

## Profile card

The bottom of the sidebar shows:

| Text under the name | Meaning |
| --- | --- |
| **Premium** (gold) | Premium. |
| **Freemium** (grey) | Not Premium. |
| **Link Discord** (accent colour) | `PremiumCheck` is on, but this Roblox account isn't linked yet. Clicking the card opens the link dialog. |

To hide the card: `Library:Window({ ..., Profile = false })`.

## Keeping Premium code safe

{% hint style="warning" %}
**The lock is only on screen.** Freemium players can't use Premium features through the UI, but someone who edits the script on their own machine could unlock them. For features worth protecting, keep their code out of the script everyone downloads. Put it in a separate LuaProtect script that's whitelisted for Premium players only. The worker can keep that whitelist in sync automatically (ask the owner to set it up).
{% endhint %}
