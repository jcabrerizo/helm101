# Vim

## Navigate

* `shift` +  `o`    - Go to the end of the file
* `:1`              - Go to the beginning of the file (line 1)

## Delete

* `cc`  - Delete current line and switch to insert mode
* `dd`  - Full line
* `D`   - Delete to the end of the line
* `C`   - Delete to the end of te line and switch to insert mode

## Lines

* `:X`          - Go to line X
* `:set number` - Show number lines
* `:m+X`        - Moves line X lines above or down depending on -X of +X

## Visual mode

* `ctrl` + `v`  - Starts block selection
* `I`           - Insert in all selected lines. esc for finishing
* `esc`         - end visual mode and apply

## View

* `ctrl` + `w` + <arrow>                    - Move the focus between views. `j`, `k`, `l` or `i` can be used instead of
  arrows
* `vim -o $FILE_ONE $FILE_TWO $FILE_N`      - Open multiple files in split mode

## Open terminal

* `:term`           - Split the view and opens the terminal. Other `vim` instance can be open in that new term

## yaml tricks

### yaml setup

The default configuration can be saved the `~/.vimrc`.

These are useful for yaml:

* `tabstop`     tab press spacing size
* `expandtab`   make sure to use spaces for tabs.
* `shiftwidth`  new line extra space when new nested block is detected

```shell
set tabstop=2
set expandtab
set shiftwidth=2
```

### Show cursor markers

* `:set cursorcolumn`         - Renders a vertical line over the cursor position. Alias `:set cuc`
* `:set cursorline`           - Renders a horizontal line over the cursor position. Alias `:set cul`
* `:set nocursorcolumn`       - Removes vertical indicator. Alias `:set nocuc`
* `:set nocursorline`         - Removes horizontal indicator. Alias `:set nocul`
