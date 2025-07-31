# LazyVim Hotkeys Cheat Sheet

> ⚡️ **Note:** Some hotkeys may depend on your plugins and your leader key (`<leader>`, default is `\` or `<Space>`).

## General

| Hotkey         | Action                           |
|----------------|----------------------------------|
| `:q`           | Quit                             |
| `:w`           | Save                             |
| `:wq`          | Save and quit                    |
| `u`            | Undo                             |
| `Ctrl+r`       | Redo                             |

## File Navigation

| Hotkey             | Action                                   |
|--------------------|------------------------------------------|
| `<leader>ff`       | Find file (`Telescope find_files`)        |
| `<leader>fg`       | Live grep (search in files)               |
| `<leader>fb`       | List open buffers                         |
| `<leader>fo`       | Old files                                 |
| `<leader>e`        | Toggle file explorer (Neo-tree)           |
| `<leader>fe`       | Find files in explorer                    |
| `<leader>/`        | Fuzzy search in current buffer            |

## Buffer Management

| Hotkey             | Action                                   |
|--------------------|------------------------------------------|
| `<Tab>`            | Next buffer                              |
| `<S-Tab>`          | Previous buffer                          |
| `<leader>bd`       | Close current buffer                     |
| `<leader>bD`       | Pick and close buffer                    |
| `<leader>bb`       | Switch buffer                            |

## Window Management

| Hotkey             | Action                                   |
|--------------------|------------------------------------------|
| `<C-h/j/k/l>`      | Move between splits                      |
| `<leader>wv`       | Split window vertically                  |
| `<leader>ws`       | Split window horizontally                |
| `<leader>wd`       | Close window                             |

## Code Navigation & LSP

| Hotkey             | Action                                   |
|--------------------|------------------------------------------|
| `gd`               | Go to definition                         |
| `gD`               | Go to declaration                        |
| `gi`               | Go to implementation                     |
| `gr`               | Go to references                         |
| `<leader>ca`       | Code action                              |
| `<leader>cr`       | Rename symbol                            |
| `K`                | Hover (show docs)                        |
| `<leader>cf`       | Format code                              |
| `[d` / `]d`        | Previous/next diagnostic                 |
| `<leader>cd`       | Show diagnostics                         |
| `<leader>cl`       | Show LSP log                             |
| `<leader>cs`       | Show LSP status                          |

## Git

| Hotkey             | Action                                   |
|--------------------|------------------------------------------|
| `<leader>gg`       | Open LazyGit                             |
| `<leader>gc`       | Commit                                   |
| `<leader>gp`       | Push                                     |
| `<leader>gl`       | Pull                                     |

## Terminal

| Hotkey             | Action                                   |
|--------------------|------------------------------------------|
| `<leader>tt`       | Toggle terminal                          |
| `<leader>tf`       | Floating terminal                        |
| `<leader>th`       | Terminal split horizontal                |
| `<leader>tv`       | Terminal split vertical                  |

## Editing

| Hotkey             | Action                                   |
|--------------------|------------------------------------------|
| `>>` / `<<`        | Indent right/left                        |
| `y` / `d` / `c`    | Yank (copy), Delete, Change              |
| `p` / `P`          | Paste after/before cursor                |
| `.`                | Repeat last change                       |
| `:%s/old/new/g`    | Replace all in file                      |
| `:noh`             | Clear search highlight                   |

## Misc

| Hotkey             | Action                                   |
|--------------------|------------------------------------------|
| `<leader>u`        | Toggle UI elements (e.g., statusline)    |
| `<leader>fn`       | New file                                 |
| `<leader>qq`       | Quit all                                 |
| `<leader>ss`       | Save session                             |
| `<leader>sl`       | Load session                             |

