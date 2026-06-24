# Luan's Tmux Config

This config aims to make tmux comfortable for vim users. Below are
some highlights and possible workflows.

**Notation**:
- `C-spc` means "hold Control, and tap the spacebar". This is the **prefix**.
- `C-M-x` means "hold Control and Alt (Option on macOS), then tap x".
- `M-x` means "hold Alt, then tap x".

Many of the most-used commands are bound **without** the prefix, using
`Ctrl+Alt`, so you don't have to reach for the prefix first.

## Commands that work "normally"

Most of the time in tmux, you're in normal mode. When you type things, they go
into your shell, and your shell can run them. You can also run the following
commands.

### Unprefixed (no prefix needed)

These work straight away — just hold `Ctrl+Alt` (and `Alt` for the `M-` ones).

Panes:
- `C-M-h` -- focus the pane to the left
- `C-M-j` -- focus the pane below
- `C-M-k` -- focus the pane above
- `C-M-l` -- focus the pane to the right
- `C-M-o` -- cycle to the next pane
- `C-M-z` -- zoom the current pane in/out (toggle)
- `C-M-x` -- kill the current pane (asks to confirm)
- `C-M-\` -- toggle "synchronize panes" (type into every pane at once)
- `M-;` -- jump to the last pane you were in

Windows:
- `C-M-n` -- next window
- `C-M-p` -- previous window
- `M-Tab` -- switch to the most recent other window
- `C-M-<` -- move the current window one slot to the left
- `C-M->` -- move the current window one slot to the right

Sessions / client:
- `C-M-s` -- switch to the last client/session
- `C-M-d` -- detach from tmux

Copy / paste:
- `C-M-[` -- enter copy mode (see below)
- `C-M-]` -- paste what you last copied

Copycat searches (jump between matches with `n` / `N`, just like a vim search):
- `C-M-f` -- find file paths on screen
- `C-M-u` -- find URLs on screen
- `C-M-g` -- find files mentioned in `git status`
- `C-M-_` -- free-form regex search

Misc:
- `C-M-c` -- clear the screen and scrollback history
- `M-:` -- open the tmux command prompt
- `C-M-r` -- reload this config

### Prefixed (press `C-spc` first)

The prefix is `C-spc` (Control+Space).

Managing "windows":
- `C-spc c` -- create a new window
- `C-spc 1` -- switch to window 1
- ...
- `C-spc 9` -- switch to window 9
- `C-spc C-spc` -- switch to the most recent other window
- `C-spc ,` -- rename this window
- To close a window, close all the panes in it. Windows start with one pane. See below.

Managing "panes":
- `C-spc |` -- split into left/right panes (side-by-side)
- `C-spc -` -- split into top/bottom panes (stacked)
- `C-spc \` -- split into full-width left/right panes
- `C-spc _` -- split into full-height top/bottom panes
- `C-spc v` -- split into left/right panes (same as `|`)
- `C-spc h` -- focus the pane to the left
- `C-spc j` -- focus the pane below
- `C-spc k` -- focus the pane above
- `C-spc l` -- focus the pane to the right
- `C-spc H` / `J` / `K` / `L` -- resize the pane left / down / up / right (repeatable)
- `C-spc z` -- Zoom!
  + If you can see multiple panes, this will "zoom in" on the current pane.
  + If you're already "zoomed in", this will zoom out, so you can see multiple
    panes again.
- Panes are just regular subshells running your usual shell. If you're running `bash` you can close them with `exit` or `C-d`.

Other:
- `C-spc r` -- reload this config
- `C-spc /` -- regex search the scrollback (copycat)
- `C-spc =` -- choose a paste buffer to paste from

### Clipboard
- `C-spc [` (or `C-M-[`) -- enter copy mode
- `C-spc ]` (or `C-M-]`) -- paste text that you previously copied in copy mode

Copies go to your **system clipboard** (`pbcopy` on macOS; `wl-copy` / `xclip` /
`xsel` on Linux), so you can paste outside of tmux too.

## Commands that work in copy mode

Copy mode is like being in vim. You can move around using vim movement keys.
You can highlight things. You can copy things to a clipboard, for pasting later
(in normal mode).

### Moving around
- `h` -- go left
- `j` -- go down
- `k` -- go up
- `l` -- go right
- `0` -- go to the beginning of the line
- `$` -- go to the end of the line
- `w` -- go foreword by one Word
- `b` -- go Backward by one word
- `fx` -- go Foreword until you hit the next `x` character. Also works for any
          other character instead of `x`.
- `Fx` -- go backward until you hit the next `x` character. Also works for any
          other character instead of `x`.
- `tx` -- go foreword unTil just before you hit the next `x` character. Also
          works for any other character instead of `x`.
- `Tx` -- go backward unTil just before you hit the next `x` character. Also
          works for any other character instead of `x`.

### Searching
- `/` -- search forwards
- `?` -- search backwards
- `n` -- jump to the next thing that matches your last search (in whatever
         direction you were already searching)
- `N` -- jump to the previous thing that matches your last search (in whatever
         direction you were already searching)

### Clipboard
- `v` -- start highlighting character-by-character (you can continue to highlight by moving around)
- `V` -- start highlighting line-by-line (you can continue to highlight by moving around)
- `y` -- copy whatever is highlighted to the clipboard (so you can paste later
         in normal mode). This flashes a `✓ Copied` confirmation and **leaves
         you in copy mode** at the same spot, so you don't lose your place.
- Dragging a selection with the **mouse** copies it as soon as you release.

### Opening things
- `O` -- open the highlighted selection (a path or URL) with your system opener
- `C-o` -- open the highlighted selection in `$EDITOR`
- `S` -- search the highlighted selection on Google

### Stopping
- `ESC` -- stop highlighting / searching and exit copy mode
- `q` -- exit copy mode and go back to normal mode

## Common workflows

### The copy-paste flow

I'm in tmux, and I have a single terminal in a single pane in a single window.

I have a list of handy commands in a file called `handy-bash-commands.txt`. I
want to use one of them. I'm going to cat the file, copy the appropriate command
to clipboard, paste it into my shell, and see the results of my cool command.
Let's break that into steps:

First I cat the file:

```
$ cat handy-bash-commands.txt

handy-command 1
handy-command 2
super cool command
sudo super cool command
another command
more stuff

$
```

Now my cursor is at the shell prompt as I would expect. I hit `C-spc [` to get
into copy mode, then hit `kkkk0` to move my cursor to the beginning of the
line that reads `sudo super cool command`. To highlight the whole line, I hit
`V`. To copy it to clipboard, I hit `y`. I see a `✓ Copied` flash, and I'm still
in copy mode, so I hit `q` to get back to normal mode. To paste and run the
command, I hit `C-spc ]`. Note that the reason the command runs as soon as I
paste is because I copied a newline to clipboard when I highlighted and copied
the whole line earlier.

Here's the result:


```
$ cat handy-bash-commands.txt

handy-command 1
handy-command 2
super cool command
sudo super cool command
another command
more stuff

$ sudo super cool command

SUPER COOL OUTPUT!

$
```

Now I want to run something similar to `handy-command 2`, but with a slight
difference.

My cursor is at the shell prompt as I would expect. I hit `C-spc [` to get
into copy mode, then hit `?handy<ENTER>` to move my cursor to the beginning of the
line that reads `handy-command 2`. I'm only interested in the beginning of this
command, so I hit `v` to start highlighting character-by-character. I hit `ww`
to highlight the words `handy-command`. I hit `y` to copy those words to the
clipboard (`✓ Copied`), then `q` to return to normal mode. To paste the command,
I hit `C-spc ]`. Because I didn't copy any newlines, the command doesn't run
immediately, and I can edit it.

Here's what my terminal looks like now:

```
$ cat handy-bash-commands.txt

handy-command 1
handy-command 2
super cool command
sudo super cool command
another command
more stuff

$ sudo super cool command

SUPER COOL OUTPUT!

$ handy-command 
```

Now I'm free to complete the command as I wish, and run it as normal:

```
$ cat handy-bash-commands.txt

handy-command 1
handy-command 2
super cool command
sudo super cool command
another command
more stuff

$ sudo super cool command

SUPER COOL OUTPUT!

$ handy-command 65537

Wow, that's a really cool number. Are you a big fan of regular polygons?

$
```
