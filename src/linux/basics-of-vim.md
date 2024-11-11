# Basics of Vim

I just put this here so I can for future reference. Of course, there are many cheatsheets and guides on how to use vim. This is just my personal cheatsheets. I am not planning on adding every command, only just some useful ones.

If you find any mistakes here, please let me know.

## Additional Reference
- [How to Use Vim – Tutorial for Beginners](https://www.freecodecamp.org/news/vim-beginners-guide/)
- [Vim Cheat Sheet](https://vim.rtorr.com/)
- [Vim Tutorial for Beginners(video)](https://www.youtube.com/watch?v=RZ4p-saaQkc)

## Start

Vim has basically four modes:
1. Normal Mode (Command Mode) - the default mode.
2. Command Line Mode - Play around with commands
3. Insert Mode - for editing the contents of the file
4. Visual Mode - for selecting the text.

## How to enter each modes?
 - Press `Esc` to enter normal mode.
 - Press `i` to enter insert mode.
 - Press `v` for visual mode.
 - Press `:` to enter command line mode.
## Saving and Quitting

*Enter command mode*
 - Use `:w` to save the file
 - Use `wq` to save and quit
 - Use `q!` to exit the file without saving changes made to the file
 - Use `q` if no changes were made.
 
 ## Cursor Movements
 - `h` - to move left
 - `l` - to move right
 - `j` - move cursor down
 - `k`- move cursor up
 - `G` - to move to the last line
 - `$` - to move to the end of a line

 ## More commands for insert mode
 - `o` - insert new line after the current line
 - `O` - insert line before the current line
 - `a` - insert after cursor
 - `i` - insert before cursor
 - `A` - insert at end of line
 - `I` - insert at beginning of line
## Undo Redo
- `u` - Undo
- `Ctrl` + `R` - for redo
 ## Set line numbers or relative line numbers
 - `set number` - to display line numbers
 - `set relativenumber` - to display relative line number
