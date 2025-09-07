# Nvim + NvChad Quick Guide

This short document explains how to install Neovim and configure it to use the [NvChad](https://github.com/NvChad/NvChad) configuration framework on Ubuntu.

## 1. Install build tools
Many plugins require a C compiler for native extensions. Install Ubuntu's build tools so `gcc` is available:

```bash
sudo apt install -y build-essential
```

## 2. Install Nerd Font
NvChad uses icons from Nerd Fonts to display file types and UI elements properly. Install a Nerd Font and configure your terminal to use it:

```bash
# 0) Prereqs
sudo apt update
sudo apt install -y unzip fontconfig   

# 1) Create a target directory
sudo mkdir -p /usr/local/share/fonts/NerdFonts/FiraCode  

# 2) Download and extract in one go
cd /tmp
wget -q https://github.com/ryanoasis/nerd-fonts/releases/download/v3.1.1/FiraCode.zip
sudo unzip -q FiraCode.zip -d /usr/local/share/fonts/NerdFonts/FiraCode 
rm FiraCode.zip

# 3) Refresh font cache
sudo fc-cache -fv

# 4) Smoke-test
fc-list | grep "FiraCode Nerd Font"  
```

After installing the font, configure your terminal emulator to use "FiraCode Nerd Font" or another Nerd Font you installed.

## 3. Install Neovim

Remove any distro‑build leftovers and enable the unstable PPA for the latest nightly:

```bash
sudo apt remove -y neovim
sudo add-apt-repository ppa:neovim-ppa/unstable
sudo apt update
sudo apt install neovim
```

## 4. Remove any existing configuration
NvChad expects an empty `~/.config/nvim` directory. If you have a previous configuration, back it up and remove the directory:

```bash
mv ~/.config/nvim ~/.config/nvim.backup 2>/dev/null || true
```

## 5. Install NvChad
Clone the NvChad starter template into `~/.config/nvim`:

```bash
git clone https://github.com/NvChad/starter ~/.config/nvim --depth 1
```

## 6. Launch Neovim
Run `nvim` for the first time to let NvChad install its plugins:

```bash
nvim
```

NvChad automatically installs its dependencies on first launch. When the installation completes, restart Neovim.

After restarting Neovim, confirm NvChad is active:

```vim
:NvCheatsheet
```

or

```vim
:Nvdash
```


If you see `E492: Not an editor command: NvCheatsheet`, the configuration didn't load correctly. Ensure the repository resides in `~/.config/nvim` and try again.

## 7. Updating NvChad

Use the built‑in Lazy manager:

```vim
:Lazy sync    " Install new plugins and remove unused ones
:Lazy update  " Update all plugins to their latest commit
```

## 8. Custom configuration

Place personal tweaks in `~/.config/nvim/lua/custom`. See the [NvChad docs](https://nvchad.com/docs) for themes, plugins and key mappings.

## 9. Leader key

NvChad maps **`<Space>`** as the leader. Verify with:

```vim
:echo g:mapleader
```

It should print a single space. Seeing `E121: Undefined variable: mapleader` means NvChad didn't load.

## 10. IDE‑like workflow

NvChad ships with plugins that give a lightweight‑IDE feel:

| Feature                  | Key                 | Notes                     |
| ------------------------ | ------------------- | ------------------------- |
| **File tree**            | `<leader> e`        | Nvim‑tree toggle          |
| **Buffer cycle**         | `Tab` / `Shift‑Tab` | Switch between open files |
| **Maximise split**       | `Ctrl + w _` / `Ctrl + w \|` | Expand height or width (`Ctrl + w =` to reset) |
| **Integrated terminals** | See table below     | Fast in‑editor shells     |
| **New tab**              | `:tabnew`           | Open a fresh workspace    |
| **Close tab**            | `:tabclose` or `:tabc` | Shut the current tab      |
| **Change tab**           | `gt` / `gT`         | Next / previous tab (`:tabnext` / `:tabprevious`)
| **Reorder tab**          | `:tabmove N`        | Move current tab to position `N`

Open a tab when you need another terminal session. Each tab can hold its
own vertical or horizontal terminal, so you can keep multiple shells open at
once. Use `:tabmove` to reorder your open tabs as needed.

### Integrated terminal cheatsheet

| Action                                           | Keys                                                                                |
| ------------------------------------------------ | ----------------------------------------------------------------------------------- |
| **Vertical terminal**                            | `<leader> v`                                                                        |
| **Horizontal terminal**                          | `<leader> h`                                                                        |
| **Leave terminal‑insert mode**                   | `Ctrl + x` (NvChad shortcut) <br>or <br>`Ctrl + \` then `Ctrl + n` (vanilla Neovim) |
| **Move focus**                                   | `Ctrl + h / j / k / l`                                                              |
| **Toggle hide/show current vertical terminal**   | `Alt + v`                                                                           |
| **Toggle hide/show current horizontal terminal** | `Alt + h`                                                                           |
| **Close buffer (any)**                           | `<leader> x`                                                                        |

That sequence — **`Ctrl + x`** → **`Ctrl + h/j/k/l`** — gets you straight back to your code after using the embedded shell.

## 11. Common file tasks

| Task                        | Key(s)        |
| --------------------------- | ------------- |
| **Fuzzy‑find file**         | `<leader> ff` |
| **Create file** (in tree)   | `a`           |
| **Delete file** (in tree)   | `d`           |
| **Create folder** (in tree) | `A`           |
| **Delete folder** (in tree) | `D`           |

| **Copy file to another folder** (in tree) | `c`, move to destination, `p` |

To copy using the file tree, highlight the file and press `c`.
Move to the target folder and press `p` to paste the copy.
Use `x` instead of `c` if you want to move it.

## 12. Change workspace folder

To open a new project in the same Neovim session, change the working directory:

```vim
:cd /path/to/project
```

Check your current location with `:pwd`. After switching directories, toggle the
file tree (`<leader> e`) to view the new folder contents.

## 13. Replace strings

Use the substitute command to search and replace text across the current file.
To replace every instance of `foo` with `bar`:

```vim
:%s/foo/bar/g
```

Add `c` to confirm each change before it happens:

```vim
:%s/foo/bar/gc
```

You can restrict the replacement to a visual selection with:

```vim
:'<,'>s/old/new/g
```

See `:h :substitute` for all options.
