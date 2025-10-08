# Tmux to Zellij Migration Notes

## Successfully Migrated Features

### Configuration
- **Scrollback buffer**: Set to 10,000 lines (matching tmux history-limit)
- **Session persistence**: Enabled via `session_serialization`, `serialize_pane_viewport`, and `serialization_interval` (300s/5min)
- **Clipboard integration**: Using `xclip -selection clipboard` (matching tmux config)
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
- Vi-mode in scroll/search (already configured)

## Features NOT Migrated (Limitations)

### Missing tmux Plugins
- **tmux-fzf** (`Ctrl-f`): No equivalent fuzzy finder for windows/panes
- **tmux-battery**: No battery status in status bar
- **tmux-jump**: No equivalent jump/search plugin

### Missing tmux Features
- **Pane joining** (bind `b`/`m` in tmux): Not available in zellij
- **Tab number toggle** (bind `space` in tmux): Not implemented
- **Ctrl-a prefix**: Zellij uses modal keybindings instead (Ctrl-b for tmux mode)
- **Custom status bar formatting**: Limited compared to tmux (no time/battery display)

### Workarounds & Alternatives
- **Session switching**: Use `Ctrl-o w` (session-manager plugin) instead of tmux session list
- **Window/pane search**: Use modal keybinds (`Ctrl-t` for tab mode, `Ctrl-p` for pane mode)
- **Battery/time status**: Would require custom zellij plugin development

## Additional Notes
- Zellij session serialization runs every 5 minutes (similar to tmux-continuum)
- Home/End keys handled by terminal (tmux bindings not needed in zellij)
- Copy mode uses vi-keybindings (already configured in existing config)
