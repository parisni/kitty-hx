# kitty-hx

Git URL generation for Helix editor with Kitty terminal integration.

## Features
- **space-g-b**: Git blame for current line
- **space-g-f**: Git file browser with commit history  
- **space-g-u**: Copy Git URL to clipboard (cursor line or selection range)
- **space-g-o**: Open Git URL in browser (cursor line or selection range)
- **space-/**: Live grep search

## URL Generation
- **Normal mode**: Generates URL for current cursor line
- **Select mode**: Generates URL for selected line range  
- **Single line**: Creates clean URL without range notation
- **Multi-line**: Creates range URL with platform-specific format

## Supported Platforms
- **GitHub**: `#L42` (single) / `#L42-L45` (range)
- **GitLab**: `#L42` (single) / `#L42-45` (range)  
- **Bitbucket**: `#lines-42` (single) / `#lines-42:45` (range)
- **Gerrit**: `#42` (single) / `#42,45` (range)

```
[keys.normal.space]
"g" = { "b" = ":sh blame %{buffer_name} %{cursor_line}", "f" = ":sh kittyx-git-file-tab %{buffer_name}", "u" = ":sh kittyx-git-url-copy %{buffer_name} %{cursor_line}", "o" = ":sh kittyx-git-url-open %{buffer_name} %{cursor_line}" }
"/" = ":sh kittyx-live-grep-tab"

[keys.select.space]
"g" = { "u" = ":sh kittyx-git-url-copy %{buffer_name} %{cursor_line}", "o" = ":sh kittyx-git-url-open %{buffer_name} %{cursor_line}" }
"/" = ":sh kittyx-live-grep-tab"
```
