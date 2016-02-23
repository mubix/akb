# TMUX and Screen

Source: http://www.dayid.org/os/notes/tm.html

| Action | tmux | screen |
|--------|------|--------|
| start a new session | `tmux` OR `tmux new` OR `tmux new-session` | `screen` |
| re-attach a detached session | `tmux attach` OR `tmux attach-session` | `screen -r` |
| re-attach an attached session (detaching it from elsewhere) | `tmux attach -d` OR `tmux attach-session -d` | `screen -dr` |
| re-attach an attached session (keeping it attached elsewhere) | `tmux attach` OR `tmux attach-session` | `screen -x` |
| detach from currently attached session | `^b d` OR `^b :detach` | `^a ^d` OR `^a :detach` |
| rename-window to newname | `^b , <newname>` OR `^b :rename-window <newname>` | `^a A <newname>` |
| list windows | `^b w` | `^a w` |
| list windows in chooseable menu | ------ | `^a "` |
| go to window # | `^b #` | `^a #` |
| go to last-active window | `^b l` | `^a l` |
| go to next window | `^b n` | `^a n` |
| go to previous window | `^b p` | `^a p` |
| see keybindings | `^b ?` | `^a ?` |
| enter "scroll mode" | `^b [` | `^a [` |
| scroll up in "scroll mode" | page up and up arrow | `^b` for page up or `k` for one line |
| scroll down in "scroll mode" | page down and down arrow | `^f` for page down or `j` for one line
| exit "scroll mode" | `q` | `ESC` |
| list sessions | `^b s` OR `tmux ls` OR `tmux list-sessions` | `screen -ls` |
| toggle visual bell | ------ | `^a ^g` |
| create another shell | `^b c` | `^a c` |
| exit current shell | `^d` | `^d` |
| split pane horizontally | `^b "` | `^a |` |
| split pane vertically | `^b %` | `^a S` |
| switch to another pane | `^b o` | `^a tab` |
| kill the current pane | `^b x` OR (logout/`^D`) | `^a X` |
| close other panes except the current one | `^b !` | ------ |
| swap location of panes | `^b ^o` | N/A |
| show time | `^b t` | ------ |
| show numeric values of panes | `^b q` | ------ |
