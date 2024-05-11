# Coolant

[![basher install](https://www.basher.it/assets/logo/basher_install.svg)](https://www.basher.it/package/)

Coolant is a utility to suspend a game process, or any other program. It uses 
the existing `SIGSTOP` and `SIGCONT` signals under the hood.

Currently Coolant supports X11 (excluding suspending the active window), 
Hyprland and Sway. Pull requests (e.g. for other Wayland window managers) 
welcome!

https://github.com/Zerodya/hyprfreeze/assets/73220426/541318e2-441a-485a-91c5-f58d4f65926a

Based on [Zerodya/hyprfreeze](https://github.com/Zerodya/hyprfreeze).

**Useful to:**

- Pause single-player games that can't normally be paused (Elden Ring, Baldur's 
  Gate 3, ...)
- Pause cutscenes to read the subtitles or if you suddenly need to leave your 
  desk
- Save system resources (excluding RAM) if you need them for another computer 
  task, or if the game's pause menu uses too many

**Caveats:**

Some processes, especially games, might not react well to being suspended and 
resumed later. Be sure to save your progress/work before trying it!

**Note:** Running games in 
[**gamescope**](https://github.com/ValveSoftware/gamescope) is highly 
recommended. (See 
[Zerodya/hyprfreeze#1](https://github.com/Zerodya/hyprfreeze/issues/1).)

## Installation

### Basher

Coolant is compatible with shell script package manager 
[Basher](https://basher.it).

```bash
basher install git.alternerd.tv/alterNERDtive/coolant
```
(Github mirror: `basher install alterNERDtive/coolant`.)

### Manual

Clone this repo and symlink the `coolant` script to a directory in your `PATH`:

```bash
git clone https://git.alternerd.tv/alterNERDtive/coolant.git Coolant
ln -s $(pwd)/Coolant/coolant $HOME/.local/bin
```

### Dependencies

- JQ to parse json
- pstree which is required to list child processes
- (Optional) xprop on X11, 
  [hyprprop](https://github.com/vilari-mickopf/hyprprop) on Hyprland or 
  [swayprop](https://git.alternerd.tv/alterNERDtive/swayprop) on Sway to get the 
  pid of a window by selecting it with your mouse.
- (Optional) Systemd for detecting session type / desktop via `loginctl`.

## Usage

Add a bind in your Hyprland or Sway config to pause the current active window:

```bash
# ~/.config/hypr/hyprland.conf
...
# Toggle freeze on active window
bind = , PAUSE, exec, coolant -a
```

### Available flags

```
-h, --help              show help message

-a, --active            toggle suspend by active window
-p, --pid               toggle suspend by process id
-n, --name              toggle suspend by process name/command
-r, --prop              toggle suspend by clicking on window (requires xprop
                        / hyprprop / swayprop)

-s, --silent            don't send notification
-t, --notif-timeout     notification timeout in milliseconds (default 5000)
--info                  show information about the process
--dry-run               doesn't actually perform an action, useful with --info
--debug                 enable debug mode
```

### Examples:

```bash
# Pause game by process name
coolant -n eldenring.exe
```

```bash
# Get info about a process by clicking on its window, without suspending it
coolant -r --info --dry-run
```
