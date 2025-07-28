# kittyx

Seamless workflow integration for Helix editor: Git operations, live search, file browsing, and text replacement with automatic file opening via Kitty terminal tabs.

Helix's design philosophy embraces simplicity without plugins, following Unix principles of composing specialized tools. Rather than constraining functionality within Helix's limited buffer system and shared keybindings, this project leverages Kitty terminal's powerful windowing to integrate best-in-class tools like ripgrep, fzf, yazi, and lazygit. Each tool excels in its domain, providing capabilities no plugin could match while maintaining clean separation of concerns.

## Features

| Keybinding | Mode | Description | Script |
|------------|------|-------------|---------|
| `space-g-b` | Normal | Git blame with PR/commit URLs | `kittyx-git-blame` |
| `space-g-f` | Normal | Interactive git file browser with commit history | `kittyx-tab git-file` |
| `space-g-g` | Normal | Interactive git browser (lazygit) | `kittyx-tab git` |
| `space-g-l` | Normal | Interactive git log browser | `kittyx-tab git-log` |
| `space-g-u` | Normal | Copy Git URL to clipboard (cursor line) | `kittyx-git-url-copy` |
| `space-g-o` | Normal | Open Git URL in browser (cursor line) | `kittyx-git-url-open` |
| `space-/` | Normal | Live grep search - opens selected file | `kittyx-live-grep-tab` |
| `space-/` | Select | Live grep with selected text as query - opens selected file | `kittyx-live-grep-tab` |
| `space-e` | Normal | File browser (yazi) - opens selected file | `kittyx-tab tree` |
| `space-r` | Normal | Replace in current file | `kittyx-replace` |
| `space-R` | Normal | Replace across project | `kittyx-replace` |
| `space-r` | Select | Replace selected text in current file | `kittyx-replace` |
| `space-R` | Select | Replace selected text across project | `kittyx-replace` |
| `ctrl-space` | Kitty | View scrollback buffer in Helix overlay | `kittyx-scrollback` |
| `alt-space` | Kitty | View last command output in Helix overlay | `kittyx-scrollback` |

## Core Features

### Enhanced Git Blame
- **Smart PR/commit URL detection**: Automatically detects and shows PR URLs when available, falls back to commit URLs
- **Multi-platform support**: GitHub, GitLab, Bitbucket, Azure DevOps, and Gerrit
- **Platform-specific patterns**: Recognizes different PR formats (`#123` for GitHub, `!123` for GitLab, Change-Id for Gerrit)
- **Rich output**: Shows author, time since commit, commit message, and clickable URL
- **URL-only mode**: `--url-only` flag for integration with other tools

### Git Browser Integration
- **Lazygit integration**: Full-featured git interface in dedicated tab
- **Git log browser**: Interactive commit history with fzf
- **Modified files browser**: Interactive browser for git status files

### Live Grep Search
- Interactive ripgrep with fzf interface in dedicated tab
- Copy file:line references with `ctrl-y`
- Selection mode prepopulates search query
- **Seamless file opening**: Selected files automatically open in Helix after tab closes
- Rich syntax highlighting in preview
- Single file selection only (no multi-select)

### File Browser (Yazi Integration)
- **Interactive file browser**: Full-featured yazi file manager in dedicated tab
- **Smart directory detection**: Opens from current buffer's directory or specified path
- **Seamless file opening**: Selected files automatically open in Helix after tab closes
- **Live grep integration**: Press `Shift+Ctrl+F` in yazi to search within the current directory
- **Unified workflow**: Both yazi selection and live-grep from yazi work seamlessly together

### Text Replacement (Scooter Integration)
- **File-scoped replacement**: Replace in current file with interactive preview
- **Project-wide replacement**: Replace across entire project with scooter integration
- **Selection-aware**: Works with selected text in Helix

### Scrollback Viewer
- **Terminal scrollback integration**: View terminal scrollback buffer in Helix overlay
- **Command output viewer**: View last command output in Helix overlay  
- **Smart cursor positioning**: Positions at cursor location for vim/less windows, jumps to end for other content
- **Custom keybindings**: Inherits your Helix config with added `q` key for force quit
- **Fast temporary files**: Uses `/dev/shm` for optimal performance

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/parisni/kittyx.git
   ```

2. Add the `bin/` directory to your PATH:
   ```bash
   export PATH="$PATH:/path/to/kittyx/bin"
   ```

3. Add the key mappings to your Helix `config.toml`:

## Configuration

Add the following key mappings to your Helix configuration file (`~/.config/helix/config.toml`):

```toml
[keys.normal.space]
# Live grep search with automatic file opening
"/" = [":open %sh{kittyx-live-grep-tab}"]

# Git operations
g = { "b" = ":sh kittyx-git-blame %{buffer_name} %{cursor_line}", "f" = ":sh kittyx-tab git-file %{buffer_name}", "l" = ":sh kittyx-tab git-log", "u" = ":sh kittyx-git-url-copy %{buffer_name} %{cursor_line}", "o" = ":sh kittyx-git-url-open %{buffer_name} %{cursor_line}", "g" = ":sh kittyx-tab git" }

# File browser with automatic file opening
e = ":open %sh{kittyx-tab tree '%{buffer_name}'}"


# Text replacement
R = [ ":write-all", ":insert-output kittyx-replace >/dev/tty", ":redraw", ":reload-all" ]
r = [ ":write-all", ":insert-output kittyx-replace %{buffer_name} >/dev/tty", ":redraw", ":reload-all" ]

[keys.select.space]
# Live grep with selected text as query  
"/" = [":pipe tee /tmp/kittyx-live-grep-query", ":open %sh{kittyx-live-grep-tab}"]

# Text replacement with selection
r = [ ":pipe tee /tmp/kittyx-replace.tmp", ":insert-output kittyx-replace %{buffer_name} >/dev/tty", ":redraw", ":reload-all" ]
R = [ ":pipe tee /tmp/kittyx-replace.tmp", ":insert-output kittyx-replace >/dev/tty", ":redraw", ":reload-all" ]
```

### Kitty Configuration

Add the following to your Kitty configuration file (`~/.config/kitty/kitty.conf`):

```bash
# Scrollback viewer with Helix
map ctrl+space launch --type=overlay --title=current --stdin-source=@screen_scrollback kittyx-scrollback
map alt+space launch --type=overlay --title=current --stdin-source=@last_cmd_output kittyx-scrollback
```

### Yazi Configuration

For enhanced yazi integration, add the following to your yazi keymap configuration file (`~/.config/yazi/keymap.toml`):

```toml
[[manager.prepend_keymap]]
on   = "<S-C-f>"
run  = 'shell -- kittyx-live-grep-tab --filepath "$@"'
desc = "Live grep on the given folder"
```

This enables:
- **`Shift+Ctrl+F` in yazi**: Launch live grep search in the current directory
- **Automatic integration**: Search results open directly in Helix
- **Smart cleanup**: Yazi automatically closes after file selection

## Requirements
- [Helix Editor](https://helix-editor.com/)
- [Kitty Terminal](https://sw.kovidgoyal.net/kitty/)
- [fzf](https://github.com/junegunn/fzf)
- [ripgrep](https://github.com/BurntSushi/ripgrep)
- [bat](https://github.com/sharkdp/bat) (for syntax highlighting in preview)
- [jq](https://github.com/jqlang/jq) (for JSON processing)
- [lazygit](https://github.com/jesseduffield/lazygit)
- [scooter](https://github.com/thomasschafer/scooter)
- [yazi](https://github.com/sxyazi/yazi)
