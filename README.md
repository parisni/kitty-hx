# kitty-hx

```
[keys.normal.space]
"g" = { "b" = ":sh blame %{buffer_name} %{cursor_line}", "f" = ":sh kittyx-git-file-tab %{buffer_name}" }
#B = ":echo %sh{git show --no-patch --format='%%h (%%an: %%ar): %%s' $(git blame -p %{buffer_name} -L%{cursor_line},+1 | head -1 | cut -d' ' -f1)}"
# see https://www.reddit.com/r/HelixEditor/comments/13x9a3z/integrating_fuzzylive_grepping_into_helix_my/
"/" = ":pipe-to kitty @ launch --type=tab --cwd=current --title=live-grep -- kittyx-live-grep"


[keys.select.space]
"/" = ":pipe-to kitty @ launch --type=tab --cwd=current --title=live-grep -- kittyx-live-grep"
```
