# TrinketMenu 3.5

A World of Warcraft addon for easy trinket management. Displays your two equipped trinkets in a bar with a mouseover menu for quick swapping from up to 30 trinkets in your bags.

## Features

- **Quick Trinket Swapping** - Left click to equip to top slot, right click for bottom slot
- **Auto Queue System** - Automatically swap trinkets based on priority and cooldowns
- **Merged Queue Mode** - Use a single priority list for both trinket slots
- **Profile System** - Save and load trinket priority configurations
- **Cooldown Tracking** - Visual cooldown displays with optional numeric countdown
- **Combat Queuing** - Queue trinket swaps during combat, auto-swap when combat ends
- **Customizable Display** - Scalable, rotatable, dockable windows

---

## Installation

1. Extract the `TrinketMenu` folder to `Interface/AddOns/`
2. Restart WoW or `/reload` if in-game

---

## Basic Usage

### Swapping Trinkets
- **Left click** a trinket in the menu to equip it to the **top** trinket slot
- **Right click** a trinket in the menu to equip it to the **bottom** trinket slot
- **Left/Right click** equipped trinkets to use them, or set up key bindings

### Window Customization
While windows are unlocked:
- **Drag the edge** to move windows
- **Right-click the edge** to rotate windows
- **Drag the lower-right corner** to resize windows
- Hold **Shift** to keep the menu open while adjusting

---

## Auto Queue System

The auto queue automatically swaps trinkets based on priority and cooldown status.

### Enabling Auto Queue
- **Alt+Click** a trinket slot on the bar, OR
- Check the enable box on the trinket's tab in options

### Queue Rules
1. Trinkets at 30 seconds or less cooldown are considered "ready"
2. If equipped trinket is on cooldown, swap to highest priority available trinket
3. Passive trinkets (no cooldown) will be replaced when a higher priority trinket is ready
4. Trinkets marked **Priority** will swap in even if current trinket is ready
5. Trinkets with **Delay** set will wait that duration before being swapped out

### Queue Icon Colors
| Icon Color | Meaning |
|------------|---------|
| Gold gear | Auto queue is active |
| Grey gear | Equipped trinket has a delay, waiting to swap |
| Red gear | Queue is paused (trinket flagged "Pause Queue") |

### Trinket Settings
- **Priority** - Swap this trinket in ASAP, even over ready trinkets
- **Delay** - Wait X seconds before swapping out (e.g., 20 for Earthstrike)
- **Pause Queue** - Suspend auto queue while this trinket is equipped

---

## Merged Queue Mode

**New in 3.5!** Use a single priority list for both trinket slots instead of managing them separately.

### How It Works
1. Enable **Merged Queue Mode** in Options
2. Both slot queues are automatically enabled
3. Configure one unified priority list on the **Merged** tab
4. The addon processes the slot with the longest cooldown first, maximizing uptime on high-priority trinkets

### Benefits
- Simpler configuration with one list
- Optimal trinket rotation across both slots
- Higher priority trinkets get equipped to whichever slot is available

---

## Profiles

Save and load trinket priority configurations:

1. Click the **Profiles** button (folder icon) in the queue tab
2. Enter a profile name and click **Save**
3. Profiles store all three lists (Top, Bottom, Merged) and the merged mode state

---

## Slash Commands

| Command | Description |
|---------|-------------|
| `/trinket` or `/trinketmenu` | Toggle the trinket window |
| `/trinket opt` | Open options window |
| `/trinket reset` | Reset window positions |
| `/trinket lock` / `unlock` | Toggle window lock |
| `/trinket scale main <n>` | Set main window scale |
| `/trinket scale menu <n>` | Set menu window scale |
| `/trinket load top\|bottom <name>` | Load a profile via macro |
| `/trinket help` | List available commands |

---

## Combat Queue

When you try to swap trinkets during combat or while dead:
- A small icon appears on the slot showing the queued trinket
- The swap happens automatically when combat ends or you revive
- Click the same trinket again to cancel the queue
- Click a different trinket to change what's queued

---

## Scripting API

### TrinketMenu.SetQueue
For advanced users who want to script queue behavior:

```lua
TrinketMenu.SetQueue(slot, command, ...)
-- slot: 0 (top) or 1 (bottom)
-- command: "ON", "OFF", "PAUSE", "RESUME", or "SORT"

-- Examples:
TrinketMenu.SetQueue(1, "PAUSE")  -- Pause bottom trinket queue
TrinketMenu.SetQueue(1, "RESUME") -- Resume bottom trinket queue
TrinketMenu.SetQueue(0, "SORT", "Earthstrike", "Diamond Flask") -- Set sort order
TrinketMenu.SetQueue(1, "SORT", "My Profile Name") -- Load a profile
```

### TrinketMenu.GetQueue
```lua
local enabled, trinketList = TrinketMenu.GetQueue(0) -- Get top slot queue info
```

---

## Optional Integrations

### Nampower Support
When Nampower is available, TrinketMenu uses enhanced cooldown tracking:
- Precise millisecond cooldown remaining
- Individual vs shared cooldown detection

### SuperWoW Support
Enhanced item info when SuperWoW is detected.

---

## Tips & Tricks

- **Shift+Click** trinkets to link them in chat
- **Drag** the minimap icon to reposition it around the minimap
- Hold **Shift** while moving items in the sort list to keep the view in place
- Set **Menu Columns** to control menu wrap behavior
- Enable **Tiny Tooltips** for minimal trinket info display

---

## Shared Cooldown Timers

Many trinkets share cooldown timers. For example, using Diamond Flask puts Cannonball Runner on a 60-second cooldown. This is normal WoW behavior.

If this happens frequently, consider:
- Only auto-queuing one trinket slot
- Organizing trinkets that don't trigger each other's cooldowns

---

## Removing Auto Queue

If you prefer the lightweight version without auto queue:
1. Exit WoW completely
2. Delete `TrinketMenuQueue.xml` and `TrinketMenuQueue.lua`
3. The remaining addon functions like the classic TrinketMenu 2.7

---

## FAQ

**Q: How do I equip to the other trinket slot?**
A: Left click = top slot, Right click = bottom slot.

**Q: Can you add other items besides trinkets?**
A: See ItemRack for full inventory slot management.

**Q: My trinket isn't swapping even though it's higher priority?**
A: By default, ready trinkets won't be replaced. Mark the incoming trinket as **Priority** to override this.

**Q: Windows won't move/resize?**
A: They're probably locked. Use `/trinket unlock`.

**Q: Why is there a tiny trinket icon on my equipped slot?**
A: You're in combat. That trinket will swap in when combat ends.

---

## Changelog

### 3.5
- Added Merged Queue Mode for unified priority list across both slots
- Improved profile system to save all lists (Top, Bottom, Merged)
- Auto-migration of old profiles to new format
- Nampower/SuperWoW integration with automatic fallback
- Fixed operator precedence bugs
- Replaced deprecated API calls
- Various code improvements and bug fixes

### 3.41
- `/trinket reset` works properly again
- `/trinket load top|bottom profilename` for macro support
- Fixed nil error if TrinketMenuQueue deleted
- Options window shrinks if TrinketMenuQueue deleted

### 3.4
- Auto queue profiles to save/load trinket priority sets
- Trinkets can be removed from list

### 3.3
- Added 1.12 Floating Combat Text support
- Trinkets coming off cooldown clear combat queue

### 3.2
- Stop Queue On Swap option
- Global cooldown spinners
- Alt+click to stop queue cancels combat queue

### 3.1
- Pause Queue attribute for trinkets
- Cooldown-enabled trinkets swapped manually won't stop queue

### 3.0
- Complete rewrite
- Auto trinket queues
- New timer and cooldown/notification system
- Menu columns, tiny tooltips, large cooldowns, key bindings display

---

## Credits

Original addon by Gello. Enhanced with Nampower/SuperWoW support.
