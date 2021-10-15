
# Shortcurts


    From Command Mode
  
    k    Up one line
  
    j    Down one line
  
    h    Left one character
  
    l    Right one character (or use <Spacebar>)
  
    w    Right one word
  
    b    Left one word
  
  * press ``dd`` in command mode to delete the current line
  * `G` to go to the end of the line
  * `d100d` to delete everything bellow the current line
  * `:undo N` undo n changes




## terminla 
471

On Mac OS X - the following keyboard shortcuts work by default. Note that you have to make Option key act like Meta in Terminal preferences (under keyboard tab)

alt (⌥)+F to jump Forward by a word
alt (⌥)+B to jump Backward by a word
I have observed that default emacs key-bindings for simple text navigation seem to work on bash shells. You can use

alt (⌥)+D to delete a word starting from the current cursor position
ctrl+A to jump to start of the line
ctrl+E to jump to end of the line
ctrl+K to kill the line starting from the cursor position
ctrl+Y to paste text from the kill buffer
ctrl+R to reverse search for commands you typed in the past from your history
ctrl+S to forward search (works in zsh for me but not bash)
ctrl+F to move forward by a char
ctrl+B to move backward by a char
ctrl+W to remove the word backwards from cursor position