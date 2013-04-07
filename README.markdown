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

*   Smaller font.
*   Assumes any file with `python` in its first line is Python (this is a
    bit of a hack, and hashbang-sniffing will improve in the future.)
*   No tooltips (they don't show up on hover for me anyway.)  Controls are
    documented in this README for now.
*   In the editing pane, pressing the tab key *rewrites* the string to
    the left of the cursor, based on some internal rewriting rules.  For
    example, `--` is replaced by `—`.  See the source code for the full set
    of rules.  The rewriting rules implement tabs in the following way:
    *   four spaces are replaced by eight spaces
    *   an empty string is replaced by four spaces
    *   a tab character is replaced by two tab characters
    *   `^I` rewrites to tab, so you can otherwise insert a real tab character

Wishlist
--------

### viewing ###

*   Left pane should have a fixed width if possible.
*   If possible, indicate the 80-column mark in the editing pane.
    (If not possible, just default the pane to that size.)

In case you're wondering:  
12345678901234567890123456789012345678901234567890123456789012345678901234567890

*   Show column position of cursor.
*   Left pane seems to want to be able to scroll when many files open,
    but does it?  Is this a bug?  Ensuring the controls are visible,
    while only the buffer tabs scroll, would be nice.  Shorter (less
    height) buffer tabs might be nice.
*   Better source language detection via sniffing the hashbang line.
*   Possibly force source language highlighting.
*   Visible whitespace (space, tab, LF) would be *really* nice.

### editing ###

*   Typing `{<enter>` or `:<enter>` should indent the next line four spaces.
*   Find should support case-sensitive and maybe whole-world-only search.
*   Replace found text with new text.
*   Ctrl+F find should populate find-entry with selected text.
*   Mark and move to mark (like Ctrl+F2/F2 in SciTE)

### buffers ###

*   Should not be possible to open 2 copies of the same file.
*   Show full path to file (in window title? in buffer tab tooltip?)
*   Should be able to populate buffers from all "interesting" files in the
    current directory tree.  (repository <=> workspace)
*   Some nice way to move/copy a buffer while inside the editor.

### other ###

*   Typing `<shift+enter>` should interpret the line as a shell command
    and run it.  It should (maybe) replace the shell command text with the
    result of running it.  Or... other creative permutations of this idea.
*   Loose integration with version control, along those lines.  Make it easy
    to see what the hg/git/&c diff of the current buffer looks like.  Make it
    easy to commit.  These might be external to the editor itself, but there
    should be an easy way to *do* them from the editor.
*   You maybe shouldn't, by default, be allowed to start multiple instances
    of tideay... trying to should just open the tab in the currently-
    running tideay.

[pfh]: http://www.logarithmic.net/pfh/
[yaedit]: http://www.logarithmic.net/pfh/yaedit 
[SciTE]: http://www.scintilla.org/SciTE.html
