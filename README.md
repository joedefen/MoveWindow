# MoveWindow (a.k.a. mvwin)
This script moves windows between monitors for these Linux desktop environments (DEs):
- **XFCE4** (which provides no shortcuts for doing so)
- **KDE/Plasma** (where the built-in moves are not too smart, but this script
  looks rather lousy when Compositor is running)
- **Mate**
- **Cinnamon**
- *perhaps* other linux desktops but w/o adjustment for its panels

When `mvwin` moves a window, it is force fit into the destination monitor
w/o overlapping its panels (unless fullscreen);
this will reduce its size if needed.

If the window is fullscreen or maximized horizotonally or vertically,
`mvwin` preserves those attributes in the move;
so, sometimes windows are enlarged when moved.

**Caveats.** The underlying commands (i.e., xrandr, xwininfo, wmctrl and xdotool) are all
hacks that only mostly work, and their defects vary between window managers.
If the window is maximized horizontally and/or vertically,
the movement is more likely to be faulty than when not maximized.
Imperfect movements are minimized for each DE with custom workarounds, but
there are a few imperfections w/o a known workaround.


## X-Windows is Required
Your DE must be **running under X** (not Wayland).

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
the mouse.
* Unless fullscreen, moved windows do not overlap XFCE4 and Mate panels
  (unless defeated using `-P` with you may wish to use if you use hidden panels).
* Similarly, for KDE/Plasma, but it is a guessing game which are panels;
  the heuristic rule is that a panel must be on an edge of a display
  and take up at 70% of the length of that edge.

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
Put `mvwin` on your PATH and define keyboard shortcuts.

# Dependencies
- python 3.4+
- X-Windows tools:
  - `xrandr`
  - `xwininfo`
  - `wmctrl`
  - `xdotool`

If the tool is missing, try `sudo apt-get install {tool}` or the equivalent for your distro.

Again, Wayland is not supported.

## Credits / Alternatives

This script is loosely derived from the script/concepts in [calandoa/movescreen](https://github.com/calandoa/movescreen)

Use `movescreen` if python 3.4+ is not available.

Relative to `movescreen`, `mvwin` force fits the window within the new monitor,
and generally, I think, is a bit more bug free and less annoying
(e.g., avoids overlapping panels, always fits windows, less geometry "loss" with multiple moves, etc.).

This script is tested on four monitors of different sizes and orientations and with 
all the mentioned DEs in the introductory section.

This script is only tested for non-autohide panels.
