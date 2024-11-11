# 🚀 Vim Commands

**🌐 A list of my commonly used Vim commands 🌐**

### Copying Lines

| Command  | Description                    |
| -------- | ------------------------------ |
| `:5,10y` | Copy range from 5 through 10   |
| `:.,+5y` | Copy range from current to 5   |
| `:.,$y`  | Copy range from current to end |
| `:%y`    | Copy all                       |

### Pasting Lines

| Command | Description |
| ------- | ----------- |
| `p`     | Paste below |
| `P`     | Paste above |

### Deleting Lines

| Command  | Description                      |
| -------- | -------------------------------- |
| `:5,10d` | Delete range from 5 through 10   |
| `:.,+5d` | Delete range from current to 5   |
| `:.,$d`  | Delete range from current to end |
| `:%d`    | Delete all                       |

### Comment lines

| Command           | Description                       |
| ----------------- | --------------------------------- |
| `:s/^/# /`        | Comment current line              |
| `:5,10s/^/# /`    | Comment range from 5 through 10   |
| `:s/^#\s\?//`     | Uncomment current line            |
| `:5,10s/^#\s\?//` | Uncomment range from 5 through 10 |

### Moving Lines

| Command     | Description                                   |
| ----------- | --------------------------------------------- |
| `:5,10m 15` | Move range from 5 through 10 to after line 15 |
| `:5m 0`     | Move line 5 to the top                        |
| `:5m $`     | Move line 5 to the end                        |

### Indenting Lines

| Command  | Description                            |
| -------- | -------------------------------------- |
| `:5,10>` | Indent lines 5 through 10 to the right |
| `:5,10<` | Indent lines 5 through 10 to the left  |

### Changing Case

| Command  | Description                 |
| -------- | --------------------------- |
| `:5,10u` | Lowercase from 5 through 10 |
| `:5,10U` | Uppercase from 5 through 10 |

### Substituting Text in a Range

| Command            | Description                                                        |
| ------------------ | ------------------------------------------------------------------ |
| `:5,10s/foo/bar/g` | Substitute "foo" with "bar" in lines 5 through 10                  |
| `:.,+5s/foo/bar/g` | Substitute "foo" with "bar" from the current line to 5 lines below |

### Joining Lines

| Command  | Description                                 |
| -------- | ------------------------------------------- |
| `:5,10j` | Join lines 5 through 10 into a single line  |
| `:.,+3j` | Join the current line with the next 3 lines |

### Copying & Pasting with Line Numbers (Visual Mode)

| Command  | Description                                                    |
| -------- | -------------------------------------------------------------- |
| `V`      | Start linewise visual mode, select lines, and then `y` to yank |
| `:5,10y` | Yank lines 5 through 10; then use `p` to paste                 |

### Saving and Exiting

| Command               | Description                            |
| --------------------- | -------------------------------------- |
| `:w`                  | Save the file                          |
| `:q`                  | Quit Vim                               |
| `:wq`                 | Save and quit                          |
| `:5,10w filename.txt` | Write lines 5 through 10 to a new file |

### Another

| Command     | Description                               |
| ----------- | ----------------------------------------- |
| `:5,10!`    | Run a shell command on lines 5 through 10 |
| `:set nu`   | Show line numbers                         |
| `:set nonu` | Hide line numbers                         |
