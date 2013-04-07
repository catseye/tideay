tideay
======

> We don't drink yaedit here... we drink tideay.

tideay is an experimental-ish fork of [pfh][]'s [yaedit][] which may
(or may not) suit my personal editing style (and probably won't suit yours.)

It aims to keep my development environment neat and... tideay (ugh.)

Controls
--------

    Ctrl+O                 open (or create) a file into a buffer
    Alt+1, Alt+2...Alt+0   switch to buffer #1, #2... #10
    Ctrl+W                 close buffer
    
    Ctrl+I                 jump to line
    Ctrl+F                 find text
    Ctrl+P                 alter common prefix of selected lines
    Ctrl+Enter             execute shell command and insert result
    
    Ctrl+Z                 undo
    Ctrl+Shift+Z           redo
    Ctrl+X                 cut
    Ctrl+C                 copy
    Ctrl+V                 paste
    Ctrl+A                 select all
    
    Ctrl+Q                 quit tideay
    
Notes
-----

A full, in-depth analysis of my history with text editors will appear here
shortly.  In the meantime, know that I currently use [SciTE][] (go ahead, laugh)
and I'm trying to start using yaedit... or rather tideay... because it's
easier to hack.  (probably.)

Not having to press Ctrl+S to save feels awkward, but in the age of version
control, it probably makes more sense, so I'm going to try to get used
to it.

Changes
-------

### display ###

*   Smaller font.
*   Notebook's tab pane (on the left) has a fixed with.  Long filenames will
    not expand it and ruin the 80-column width of the editor pane.
*   Height of each label in the tab pane is smaller.
*   Only the basename (not the directory name) of the file is displayed in
    the label.
*   No tooltips (they don't show up on hover for me anyway.)  Controls are
    documented in this README for now.

### editing ###

*   Will sniff the hashbang line of the file if it cannot determine the
    language from the file extension.  Currently supports Python, Ruby, and
    shell.
*   In the editing pane, pressing the tab key *rewrites* the string to
    the left of the cursor, based on some internal rewriting rules.  For
    example, `--` is replaced by `—`.  See the source code for the full set
    of rules.  The rewriting rules implement tabs in the following way:
    *   four spaces are replaced by eight spaces
    *   an empty string is replaced by four spaces
    *   a tab character is replaced by two tab characters
    *   `^I` rewrites to tab, so you can otherwise insert a real tab character
*   Typing Enter after a `{` or `(` or `[` or `:` indents the next line
    another four spaces.
*   Typing Ctrl+Enter interprets the line as a shell command and executes it,
    replacing the line with the standard output produced by the command.
    Note that this is a little experimental, and the exact usage may vary in
    the future.  For now, it is, among other things, a neat way to include
    snippets into the file (`cat snippet.txt` Ctrl+Enter).

Wishlist
--------

### display ###

*   Show column position of cursor.
*   When notebook tab pane exceeds height of window, you can only scroll down
    as far as the "open" entry.  You can't seem to use "find" etc properly.
    Ensuring the controls are visible, while only the buffer tabs scroll,
    would be nice (but it doesn't look like GtkNotebook supports this much.)
*   Visible whitespace (space, tab, LF) would be *really* nice.

### editing ###

*   Shift+Bkspc should delete four spaces.
*   Hitting Enter on a line that starts with `*   ` or `-   ` should also
    indent four spaces (Markdown lists.)

### find and replace ###

*   Find should support case-sensitive and maybe whole-world-only search.
*   Replace found text with new text.
*   Ctrl+F find should populate find-entry with selected text.
*   Mark and move to mark (like Ctrl+F2/F2 in SciTE)
*   Next and Prev should have keyboard shortcuts.

### buffers ###

*   Should not be possible to open 2 copies of the same file.
*   Show full path to file in window title.
*   Should be able to populate buffers from all "interesting" files in the
    current directory tree.  (repository <=> workspace)
*   Some nice way to move/copy a buffer while inside the editor.
*   Some way to switch to a buffer by typing in part of its filename.

### other ###

*   Running a shell command — there should there be some way to retain the
    command you ran.  Maybe by default, but it gets selected, so that you
    can immediately delete it?  Not sure.
*   Loose integration with version control, along those lines.  Make it easy
    to see what the hg/git/&c diff of the current buffer looks like.  Make it
    easy to commit.  These might be external to the editor itself, but there
    should be an easy way to *do* them from the editor.
*   You maybe shouldn't, by default, be allowed to start multiple instances
    of tideay... trying to should just open the tab in the currently-
    running tideay.

### low priority ###

*   Indicate the 80-column mark in the editing pane.
*   Some way to force source language highlighting.

[pfh]: http://www.logarithmic.net/pfh/
[yaedit]: http://www.logarithmic.net/pfh/yaedit 
[SciTE]: http://www.scintilla.org/SciTE.html
