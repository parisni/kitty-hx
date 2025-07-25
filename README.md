# kittyx

Advanced Git integration and search utilities for Helix editor with Kitty terminal integration.

## Features

| Keybinding | Mode | Description | Script |
|------------|------|-------------|---------|
| `space-g-b` | Normal | Git blame for current line | `blame` |
| `space-g-f` | Normal | Interactive git file browser with commit history | `kittyx-git-file-tab` |
| `space-g-g` | Normal | Interactive browser for git modified files | `kittyx-git-tab` |
| `space-g-u` | Normal | Copy Git URL to clipboard (cursor line) | `kittyx-git-url-copy` |
| `space-g-o` | Normal | Open Git URL in browser (cursor line) | `kittyx-git-url-open` |
| `space-/` | Normal | Live grep search in new tab | `kittyx-live-grep-tab` |
| `space-/` | Select | Live grep with selected text as query | `kittyx-live-grep-tab` (piped) |

## Core Features

### Git File Browser
- Interactive commit history browser with fzf
- Preview commit details and changes
- Direct PR/commit URL opening with `ctrl-o`
- Launches in dedicated Kitty tab

### Git Modified Files Browser
- Interactive browser for git modified/staged/untracked files
- Multi-selection support with fzf
- File preview with syntax highlighting
- Copy file paths with `ctrl-y`
- Automatic Helix integration for opening selected files
- Launches in dedicated Kitty tab

### Live Grep Search
- Interactive ripgrep with fzf interface
- Copy file:line references with `ctrl-y`
- Selection mode prepopulates search query
- Automatic Helix integration for opening results
- Rich syntax highlighting in preview

### Git URL Generation
- Context-aware URL generation for multiple platforms
- Direct browser opening or clipboard copying
- Supports current cursor line positioning

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
"g" = { "b" = ":sh blame %{buffer_name} %{cursor_line}", "f" = ":sh kittyx-git-file-tab %{buffer_name}", "g" = ":sh kittyx-git-tab", "u" = ":sh kittyx-git-url-copy %{buffer_name} %{cursor_line}", "o" = ":sh kittyx-git-url-open %{buffer_name} %{cursor_line}" }
"/" = ":sh kittyx-live-grep-tab"

[keys.select.space]
"/" = ":pipe kittyx-live-grep-tab"
```

## Supported Git Platforms
- **GitHub**: `#L42` (line anchors)
- **GitLab**: `#L42` (line anchors)  
- **Bitbucket**: `#lines-42` (line anchors)
- **Gerrit**: `#42` (line anchors)

## Requirements
- [Helix Editor](https://helix-editor.com/)
- [Kitty Terminal](https://sw.kovidgoyal.net/kitty/)
- [fzf](https://github.com/junegunn/fzf)
- [ripgrep](https://github.com/BurntSushi/ripgrep)
- [bat](https://github.com/sharkdp/bat) (for syntax highlighting in preview)
- [jq](https://github.com/jqlang/jq) (for JSON processing)
