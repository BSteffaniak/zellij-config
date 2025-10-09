# Tmux to Zellij Migration Notes

## Prefix Key Migration

**Tmux Prefix:** `Ctrl-a`  
**Zellij Equivalent:** `Ctrl-a` (tmux mode)

Zellij uses a "tmux mode" that mimics tmux prefix behavior. Press `Ctrl-a` to enter tmux mode, then press a command key.

### Common Tmux Mode Commands (Ctrl-a + key):

| Key | Action | Notes |
|-----|--------|-------|
| `c` | New tab | Opens in current directory |
| `C` (Shift-c) | New session | Opens session-manager in create mode |
| `$` | Rename session | Opens session-manager, then press `Ctrl-r` |
| `"` | Split horizontal | New pane below |
| `%` | Split vertical | New pane right |
| `,` | Rename tab | Enter rename mode |
| `d` | Detach session | Return to shell |
| `z` | Zoom pane | Toggle fullscreen |
| `[` | Scroll/copy mode | Vi keybindings |
| `h/j/k/l` | Navigate panes | Vim-style movement |
| `n` | Next tab | Cycle forward |
| `p` | Previous tab | Cycle backward |
| `^` | Last tab | Toggle between two tabs |
| `x` | Close pane | Kill focused pane |
| `o` | Focus next pane | Cycle pane focus |
| `space` | Next layout | Cycle pane layouts |
| `Ctrl-a` | Send Ctrl-a | Passes through to shell |

**Notes:** 
- `Ctrl-a C` (uppercase) opens session-manager in "new session" mode, where you type the session name and press Enter to create it.
- `Ctrl-a $` opens session-manager. Select a session, then press `Ctrl-r` to rename it. (Unlike tmux, it's a 2-step process)

## Successfully Migrated Features

### Configuration
- **Scrollback buffer**: Set to 10,000 lines (matching tmux history-limit)
- **Session persistence**: Enabled via `session_serialization`, `serialize_pane_viewport`, and `serialization_interval` (300s/5min)
- **Clipboard integration**: Using OSC 52 (works over SSH and locally with any compatible terminal)
- **Mouse support**: Enabled by default in zellij

### Keybindings

#### Tab (Window) Navigation
- `Alt-1` through `Alt-9`, `Alt-0` → Direct tab switching (tabs 1-10)
- `Ctrl-h` / `Ctrl-j` / `Ctrl-Left` → Previous tab
- `Ctrl-s` / `Ctrl-Right` → Next tab
- `Ctrl-l` → Toggle to last tab

#### Pane Navigation with Auto-Zoom
- `Ctrl-k` / `Ctrl-t` → Focus previous pane + zoom
- `Alt-t` → Focus next pane + zoom

#### Utilities
- `Ctrl-q` → Clear screen
- `Ctrl-f` → **room** plugin (fuzzy tab search and switch)
- Vi-mode in scroll/search (already configured)

## Features NOT Migrated (Limitations)

### Partially Migrated tmux Plugins
- **tmux-fzf** (`Ctrl-f`): Migrated to **room** plugin for tab switching
  - ✅ Fuzzy search tabs by name
  - ✅ Case-insensitive filtering (`ignore_case true`)
  - ✅ Quick jump by number (disabled by default)
  - ❌ No session/pane/command/keybinding/clipboard/process features (only tab switching)
  
### Missing tmux Plugins
- **tmux-battery**: No battery status in status bar
- **tmux-jump**: No equivalent jump/search plugin

### Missing tmux Features
- **Pane joining** (`Ctrl-a b`/`Ctrl-a m` in tmux): Not available in zellij
- **Tab number toggle** (`Ctrl-a space` custom feature): Not implemented
- **Config reload** (`Ctrl-a r` in tmux): Not needed (zellij auto-reloads)
- **Custom status bar formatting**: Limited compared to tmux (no time/battery display)

### Workarounds & Alternatives
- **Session switching**: Use `Ctrl-o w` (session-manager plugin) instead of tmux session list
- **Tab/Window search**: Use `Ctrl-f` (room plugin) for fuzzy tab switching
- **Pane search**: Use modal keybinds (`Ctrl-p` for pane mode) or consider zellij-pane-picker plugin
- **Battery/time status**: Would require custom zellij plugin development

## Additional Notes
- Zellij session serialization runs every 5 minutes (similar to tmux-continuum)
- Home/End keys handled by terminal (tmux bindings not needed in zellij)
- **room** plugin is loaded on-demand from GitHub releases (no manual installation needed)

## Copy/Paste Workflow

**Important:** Zellij does NOT have tmux-style vi-mode copy selection.

### How to Copy Text:
1. Press `Ctrl-a [` to enter scroll mode
2. Hold `SHIFT` + drag mouse to select text
3. Text automatically copied via OSC 52 to your clipboard (works over SSH!)
4. Press `Esc` to exit scroll mode

### Alternative - Edit Scrollback:
1. Press `Ctrl-a [` to enter scroll mode
2. Press `e` to open scrollback in your `$EDITOR` (nano/vim)
3. Copy using editor commands
4. Exit editor

### Limitations:
- **No keyboard-only selection** (no vi-mode `v`/`y` like tmux)
- **Must use mouse + SHIFT** to select text
- OSC 52 clipboard works over SSH and locally (Ghostty, WezTerm, Alacritty, Windows Terminal all supported)

## Terminal Color Configuration

Zellij has been configured to match tmux's color rendering:
- **`styled_underlines false`**: Disabled to prevent color interference in applications (editors, syntax highlighting)
- **`COLORTERM=truecolor`**: Explicitly enables 24-bit RGB color support for all panes
- **Note**: Requires zellij restart to take effect. Exit all sessions and restart zellij.

## Installed Plugins
- **room** (v1.2.0+): Fuzzy tab search and switch bound to `Ctrl-f`
  - Press `Ctrl-f` to open floating fuzzy finder
  - Type to filter tabs by name (case-insensitive)
  - `Enter` to switch, `Esc` to cancel
  - Tab numbers shown for quick reference
