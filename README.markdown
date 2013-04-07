tideay
======

> We don't drink yaedit here... we drink tideay.

tideay is an experimental-ish fork of [pfh][]'s [yaedit][] which may
(or may not) suit my personal editing style (and probably won't suit yours.)

It aims to keep my development environment neat and... tideay (ugh.)

Notes
-----

A full, in-depth analysis of my history with text editors will appear here
shortly.  In the meantime, know that I currently use SciTE (go ahead, laugh)
and I'm trying to start using yaedit... or rather tideay... because it's
easier to hack.  (probably.)

Not having to press ^S to save feels awkward, but in the age of version
control, it probably makes more sense, so I'm going to try to get used
to it.

Changes
-------

*   Smaller font.
*   Assumes any file called `tideay` is Python (this is a temporary hack!)

Wishlist
--------

### viewing ###

*   Left pane should have a fixed width if possible.
*   If possible, indicate the 80-column mark in the editing pane.
    (If not possible, just default the pane to that size.)
*   Show column position of cursor.
*   Left pane seems to want to be able to scroll when many files open,
    but does it?  Is this a bug?  Ensuring the controls are visible,
    while only the buffer tabs scroll, would be nice.  Shorter (less
    height) buffer tabs might be nice.
*   A way to detect source language via the hashbang line and/or force
    the source language.
*   No tooltips (why do I need them? it's *my* editor)

### editing ###

*   Typing `{<enter>` or `:<enter>` should indent the next line four spaces.
*   Typing `<tab>` should "tab-complete" (rewrite) the text to the left.
    This never results in a tab character, unless a tab character is what the
    rewrite rule says to substitute in.
*   Find should support case-sensitive and maybe whole-world-only search.
*   Replace found text with new text.
*   ^F find should populate find-entry with selected text.

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
