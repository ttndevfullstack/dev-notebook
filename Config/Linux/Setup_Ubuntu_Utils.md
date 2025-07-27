# 🚀 Ubuntu 24.04 WSL Developer Setup Guide

This guide helps you set up a powerful developer environment inside Ubuntu 24.04 running on WSL (Windows Subsystem for Linux). It includes essential tools for productivity, navigation, file handling, and version control.

---

## 📦 Step 1: Install Essential CLI Tools

Run the following command to install all tools at once:

```bash
sudo apt update && sudo apt install git curl wget fzf bat ripgrep eza lsd tmux btop -y
```

---

## 🔧 Tool Overview and Useful Commands

### 1. `wget` - Network file downloader
- Download file:
  ```bash
  wget https://example.com/file.zip
  ```
- Download in background:
  ```bash
  wget -b https://example.com/largefile.zip
  ```

---

### 2. `fzf` - Fuzzy Finder
- Fuzzy search in command history:
  ```bash
  CTRL + R
  ```
- Search file names:
  ```bash
  fzf
  ```

---

### 3. `bat` (or `batcat`) - Enhanced `cat` with syntax highlighting

- View file with highlighting:
  ```bash
  bat file.py
  ```
- Compare two files visually:
  ```bash
  diff file1.py file2.py | bat --paging=always
  ```

> **Note:** Use `batcat` if `bat` is aliased that way in Ubuntu.

#### 🔍 Interactive Search & Navigation in `batcat`

##### 🔎 Search
- `/search_term` – Search forward for the term  
- `?search_term` – Search backward  
- `n` – Next match  
- `N` – Previous match  
- `ESC u` – Clear highlighting  

##### 🧭 Navigation
- `g` – Go to beginning of file  
- `G` – Go to end of file  
- `10g` – Go to line 10  
- `gg` – Also goes to top  
- `:42` – Go to line 42  

##### 🖱️ Paging
- `Space` – Next page  
- `b` – Previous page  
- `Enter` – Scroll one line down  
- `k / j` – Scroll up/down one line (vim-style)  

##### ❌ Exit
- `q` – Quit viewer

---

### 4. `ripgrep (rg)` - Super-fast text search in files
- Search keyword in directory:
  ```bash
  rg "search_term"
  ```
- Case-insensitive search:
  ```bash
  rg -i "term"
  ```

---

### 5. `eza` - Modern replacement for `ls`
- List with icons:
  ```bash
  eza --icons
  ```
- Long list with permissions:
  ```bash
  eza -l --icons
  ```
- Tree view:
  ```bash
  eza -T --icons
  ```
- Git-aware listing:
  ```bash
  eza --git --long --icons
  ```

---

### 6. `tmux` - Terminal multiplexer (split window, manage sessions)
- Start tmux:
  ```bash
  tmux
  ```
- Split pane vertically:
  ```bash
  CTRL + B, then "
  ```
- Split pane horizontally:
  ```bash
  CTRL + B, then %
  ```
- Detach session:
  ```bash
  CTRL + B, then D
  ```
- Resume session:
  ```bash
  tmux attach
  ```

---

## ✅ Recommended Aliases

Add these to your `~/.zshrc` or `~/.bashrc`:

```bash
alias ls="eza --icons"
alias ll="eza -lah --icons"
alias lt="eza -T --icons"
alias cat="batcat"
```

Reload config:
```bash
source ~/.zshrc
```

---

## 🎉 You're All Set!
Enjoy your modern, efficient Ubuntu WSL terminal environment!
