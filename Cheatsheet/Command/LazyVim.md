NeoVim Hotkeys
================
___

_A list of my commonly used NeoVim hotkeys and setup commands_

--

### Setup & Configuration

1. **Download NeoVim latest version**
   ```bash
   sudo add-apt-repository ppa:neovim-ppa/unstable
   sudo apt update
   sudo apt install neovim
   ```
2. **Clone the LazyVim starter template**
   ```bash
   git clone https://github.com/LazyVim/starter ~/.config/nvim
   rm -rf ~/.config/nvim/.git
   nvim
   ```
3. **Disable relative line numbers**
   ```bash
   vi ~/.config/nvim/lua/config/options.lua
   ```
   Add or update:
   ```lua
   vim.opt.relativenumber = false
   vim.opt.number = true
   ```
4. **Add extra plugins (Typescript, JSON, Animations)**
   ```lua
   require("lazy").setup({
     spec = {
       { "LazyVim/LazyVim", import = "lazyvim.plugins" },
       { import = "lazyvim.plugins.extras.lang.typescript" }, -- Add here
       { import = "lazyvim.plugins.extras.lang.json" },        -- Add here
       { import = "lazyvim.plugins.extras.ui.mini-animate" },  -- Add here
       { import = "plugins" },
     },
   })
   ```
5. **Set background transparent**
   ```bash
   vi ~/.config/nvim/lua/plugins/colorscheme.lua
   ```
   Update the content:
   ```lua
   return {
     {
       "folke/tokyonight.nvim",
       opts = {
         style = "night",
         transparent = true,
         styles = {
           sidebars = "transparent",
           floats = "transparent",
         },
       },
     },
   }
   ```

--

### File/Folder

| Command | Description |
| ------- | ----------- |
| `leader r` | Create new file |
| `d` | Delete a file |

### Edit File

| Command | Description |
| ------- | ----------- |
| `yiw` | Copy a word |
| `yy` | Copy a line |
| `3yy` | Copy 3 lines |
| `:10,20y` | Copy from line 10 to 20 |
| `leader s t` | Search TODO |
| `:e!` | Recover file changes |
| `leader s r` | Replace text |

### Move

| Command | Description |
| ------- | ----------- |
| `leader + e` | Open/Close Navigation Bar |
| `leader + w + h` | Go to left Window |
| `leader + w + j` | Go to bottom Window |
| `leader + w + k` | Go to top Window |
| `leader + w + l` | Go to right Window |
| `leader + f + g` | Go to file |
| `Shift + l` | Go to right file |
| `Shift + h` | Go to left file |
| `Ctrl + 6` | Go to previous file |
| `o` | Go to first line of below function |
| `s -> [word] -> [map key]` | Go to word |
| `gd` | Link to path |
| `[ + b` | Go to previous position |
| `] + b` | Go to next position |

### Split Windows

| Command | Description |
| ------- | ----------- |
| `leader w v` | Split window vertically |
| `leader -` | Split window horizontally |
| `leader w d` | Close a window |
| `Ctrl h` | Go to left window |
| `Ctrl Shift j` | Go to down window |
| `Ctrl Shift k` | Go to top window |
| `Ctrl l` | Go to right window |


