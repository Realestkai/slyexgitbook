---
description: What changed in Sylex UI v2, and what to check when upgrading.
---

# Changelog and upgrading

## v2.1

* `Midnight` is now a flat dark midnight blue, with no gradient.
* Eight new themes: Onyx, Slate, Ocean, Emerald, Crimson, Rose, Amber and Mocha.
* The theme dropdown is gone from the top bar. Players pick a theme in the settings tab.
* The open button and the window title show the Sylex logo. Set `CloseUIButton.Image` or `Icon` to change them.

## v2

### New

* **Flags and configs:** `Flag` on any element, `Library.Flags` / `Library.Options`, save/load/delete/list configs, autoload and autosave.
* **Shared element API:** every element has `Get`, `Set`, `SetVisible`, `Destroy`, and `Tooltip`. Button now returns an object.
* **New elements:** Paragraph, Divider, ProgressBar, RangeSlider, PlayerDropdown, plus a working Image element.
* **Sections:** collapsible sections.
* **Search:** per-tab search.
* **Dropdowns:** `Refresh`, `GetOptions`.
* **Sliders:** `Suffix`.
* **Textboxes:** `Numeric`.
* **Keybinds:** `Mode = "Press"`, and Escape cancels rebinding.
* **Notifications:** `Type` (Success, Warning, Error) and a close button.
* **Dialogs:** any number of buttons, `Content` and `Input`.
* **Themes:** `Library:AddTheme`, `Window:SetTheme`, `Window:SetAccent`.
* **Built-ins:** `Window:SettingsTab()`, `Window:Watermark()`, `Window:KeybindList()`.
* **Unloading:** `Library:Unload()`, `Library:OnUnload()`, `Library.Unloaded`.
* **Window behaviour:** remembers its position and size, scales automatically on small screens, and `Config` / `CloseUIButton` are now optional.

### Fixed

* Code blocks: multi-line strings and block comments now highlight correctly.
* Theme switching is faster, and destroyed elements are cleaned up.
* Textbox icon and text now follow the theme.
* Multi-select dropdown callbacks always receive a list.
* Keybinds no longer fire while typing in chat, and clicking the row now runs the callback.
* Dropdown and colour-picker popups close when the window moves, hides or changes tab.
* `SetVisible(false)` no longer leaves an empty gap.
* Toggles, keybinds and dropdowns no longer get their starting value overwritten by a late initial callback after `LoadConfig`.
* Colour pickers no longer run their callback twice on creation.
* Searching a dropdown with symbols like `(` or `%` no longer errors.
* Tab switching no longer replays its animation once per tab.
* The icon download can't break loading any more.
* No global variables leak out of the library.

## Upgrading from v1

Most scripts work unchanged. Check these:

| Change | What to do |
| --- | --- |
| The ScreenGui is now called `Sylex` (was `Dummy Kawaii`). | If your script searches for the GUI by name, use `Library:Unload()` instead. |
| Textbox `ClearText` now means what it says: `true` clears on click. The default no longer clears. | If you passed `ClearText = true` to *keep* text, remove it. |
| `Textbox:SetValue` and `Keybind:SetValue` now run the callback, like every other element. | Pass `true` as a second argument to skip it: `Textbox:Set("x", true)`. |
| `Window:Notify` no longer waits before returning. | Nothing, unless you relied on the delay. |
| Multi-select dropdowns start with nothing selected unless you pass `Value`. | Pass `Value = { ... }` to preselect. |
| A Slider's default `Value` is now `Min` (was `Min + 1`). | Pass `Value` explicitly if you relied on the old default. |
| Unknown icon names show a blank icon instead of a broken image. | Nothing. |
| Callback errors are now printed as warnings. | Fix any errors that show up. Before, they were silently ignored. |
