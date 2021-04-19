
# MoveWindow (a.k.a. mvwin)
This script moves windows between monitors for:
- XFCE4 (which provides no shortcuts for doing so)
- KDE/Plasma (where the built-in moves are very smart)
- other linux desktops but w/o adjustment for its panels
It requires python 3.4 or later.

## Usage
```
mvwin [-h] [-r] [-P] [--DB] [-w WIN_ID] {left,right,up,down,next,prev,here}

positional arguments:
  {left,right,up,down,next,prev,here} -  choose direction of movement

optional arguments:
  -r, --ratio           keep window ratio
  -P, --no-panel-adjust
                        defeat adjustment for desktop panels
  --DB                  print debugging info
  -w WIN_ID, --win-id WIN_ID
                        specify ID of window
```
## Description

Relocated windows are made to fit, made active, raised, and contain
the mouse.  Unless fullscreen, moved windows do not overlap XFCE4
panels (unless defeated using `-P` with you may wish to use if you use hidden panels).

The window geometry is preserved as best possible but constrained within
the new monitor. Optionally (`-r`), the window will keep the same
proportions in the new monitor instead.

By default, the active window is chosen to move, but you can specify another
window by its "window ID" (e.g., as shown by `wmctrl -l`).

The monitor is determined by specifying one of:

- `left`, `right`, `up`, or `down` - direction relative to the current monitor
- `next`, `prev` - move in an arbitrary rotation through monitors
- `here` - same screen

If you choose `here` or a monitor that does not exist (e.g., `right` of
a rightmost monitor), then the chosen window will simply be made to
fit within current monitor if it does not already.

In practice, you define keyboard shortcuts and tie them to this script.
Personally, I define four shortcuts:

- `ALT-J` - `mvwin down` # move active window down
- `ALT-K` - `mvwin up` # move active window up
- `ALT-H` - `mvwin left` # move active window left
- `ALT-L` - `mvwin right` # move active window right
    
The JKHL keys mimic the directional keys in `vim`, and `ALT-Right-Click` is used
to resize windows; so ALT works out well as the "Window" alteration key.
For debuging, run `mvwin` manually in a terminal window and use `--DB` option to print
values that it found and computed.

# Installation (Manual)

Just put `mvwin` on your PATH and define keyboard shortcuts.

# Dependencies

- python 3.4+
- xrandr
- xwininfo
- wmctrl (not present? try sudo apt-get install wmctrl )
- xdotool

## Credits / Alternatives

This script is loosely derived from the script/concepts in [calandoa/movescreen](https://github.com/calandoa/movescreen)

Use `movescreen` if python 3.4+ is not available.

Relative to `movescreen`, `mvwin` force fits the window within the new monitor,
and generally, I think, is a bit more bug free and less annoying
(e.g., avoids overlapping panels, always fits windows, less geometry "loss" with multiple moves, etc.).

This script is tested on four monitors of different sizes and orientations.

