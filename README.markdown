tideay
======

> We don't drink yaedit here... we drink tideay.

tideay is an experimental-ish fork of [pfh][]'s [yaedit][] which may
(or may not) suit my personal editing style (and probably won't suit yours.)

It aims to keep my development environment neat and... tideay (ugh.)

See "Notes", below, for my thoughts (= rant) on editors, and why I decided
to start this project.

Requirements
------------

*   Python 2.7
*   pygtksourceview2

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

Changes from yaedit
-------------------

### display ###

*   Smaller font.
*   Notebook's tab pane (on the left) has a fixed width.  Long filenames will
    not expand it and ruin the 80-column width of the editor pane.
*   Height of each label in the tab pane is smaller.
*   Only the basename (not the directory name) of the file is displayed in
    the label.  Full (relative) path to file is shown in the window title.
*   No tooltips (they don't show up on hover for me anyway.)  Controls are
    documented in this README for now.

### editing ###

*   The hashbang line of the file will be sniffed if the language cannot be
    determined from the the file extension.  Currently supports Python, Ruby,
    and shell.
*   In the editing pane, pressing the tab key *rewrites* the string to
    the left of the cursor, based on some internal rewriting rules.  For
    example, `--` is replaced by `—`.  See the source code for the full set
    of rules.  The rewriting rules implement tabs in the following way:
    *   four spaces are replaced by eight spaces
    *   an empty string is replaced by four spaces
    *   a tab character is replaced by two tab characters
    *   `^I` rewrites to tab, so you can otherwise insert a real tab character
*   Typing Enter after a `{` or `(` or `[` or `:`, or on a line that begins
    with `*   ` or `-   `, indents the next line (another) four spaces.
    Most languages benefit from the brackets, Python benefits from the colon,
    and Markdown benefits from the `*`/`-` for lists.
*   Typing Ctrl+Enter interprets the line as a shell command and executes it,
    replacing the line with the standard output produced by the command.
    If the line begins with `%`, the command is not replaced; the output is
    appended underneath it, along with another line prefaced with `% ` — the
    effect being a pseudo-shell of sorts.
    Note that Ctrl+Enter is a little experimental, and the exact usage may vary
    in the future.  For now, it is, among other things, a neat way to include
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

*   Shift+Bkspc should delete four spaces, or rather, do a "destructive
    rewrite", which would just be like Tab except a different set of rules.
*   tab with selection should not do GtkSourceView's tab with selection

### find and replace ###

*   Find should support case-sensitive and maybe whole-world-only search.
*   Replace found text with new text.
*   Ctrl+F find should populate find-entry with selected text.
*   Mark and move to mark (like Ctrl+F2/F2 in SciTE)
*   Next and Prev should have keyboard shortcuts.

### buffers ###

*   Should not be possible to open 2 copies of the same file.
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

Notes
-----

I guess this fork was precipitated by
[this article](http://delvarworld.github.io/blog/2013/03/16/just-use-sublime-text/)
in a roundabout way.

OK, I guess I can't talk about editing without first talking about typing.
There are developers who consider typing ability to be a paramount skill.  I'm
not one of them.  I'm only a touch-typist in the loosest sense; the words "home
row" mean nothing to me; I favour my right hand while my left hand is sometimes
close to useless.  For a software developer, being able to type quickly is a
"nice-to-have".  Here are some skills that are more valuable than typing
quickly: ability to conceptualize, ability to explain things clearly, ability
to break large problems into smaller parts, foresight, mathematical maturity,
ability to step back and see the bigger picture, time management, insight into
how humans deal with change... (point made yet, or should I go on?)

And I also guess I can't talk about editors without talking about Those Two.

My first exposure to Emacs was its port to the Amiga that came with Amiga OS
1.3.  Consider that, wow, this new computer is one of the *next generation*:
it has a *mouse*, and a *GUI*, with *windows* and *menus* and... and you want
me to edit with... what's this terminal-based thing?  This makes no sense.
Amiga Basic was naff, but at least its program editor was comprehensible.

My first exposure to Vim was `vi`, on the servers at the university.  I failed
to see the attraction (but, of course, did not fail to meet people who *had*
seen the attraction, and hear them talk about the attraction.)  If I had to
edit something on the server, I would use `pico` (these days it's `nano` or,
on FreeBSD and friends, `easyedit`).  If I didn't have to edit something on the
server, I wouldn't.  To this day, `:q!` is pretty much all the `vi` I know.

I've never seen the attraction of editors that insist on putting you into one
of several "modes" and which only grudgingly accept that we're now in the age
of the GUI.

When I first switched to FreeBSD as my development platform, I used [nedit][].
It was OK, but I wasn't a fan of its mass of dependencies.

At some point, I switched to [SciTE][], for two reasons:

*   It supports code folding — although I don't remember if I switched to it
    before or after reading Nikolai Bezroukov's paper on [Orthodox Editors][]
*   I was switching between Windows and FreeBSD a lot, and wanted an editor
    that was the same on both of them

At some other point, I also read Raskin's [The Humane Interface][] and had
some thoughts about some of his points about UI's.

A few years ago, Elliott Hird pointed me at yaedit, not really as a
recommendation so much as a "this is interesting, this is how one guy has
approached building *his* own editor."  I took it for a spin and couldn't
really get the hang of it.  When you've built up that Ctrl+S habit to save
your work, and it's no longer needed, *bleah*, it feels so awkward; and it
rubs that inner engineer of yours wrong for any program to be constantly
writing to the disk like that when it doesn't "have" to.

But then, fast forward a few years to now, and I come across that article I
mentioned, and I start to think about how I use SciTE.  Taking inventory, I
realize I only use a fraction of its features.  Code folding is nice, but I
only use it once in a blue moon.  I seriously under-use its Lua scripting.
I don't use any of its IDE-like features (except by accident, at which point
they annoy me.)  I always seem to be losing track of its configuration files,
so features I'd like to use, like abbreviation expansion, I haven't always
taken the time to set up.

And I have developed some bad habits with SciTE.  The worst, which surely
predates SciTE itself, is probably that I'll start and close the editor in a
very ad-hoc fashion.  I want to edit some files, OK, `scite *.py` in the shell.
OK, I'm done editing, oh, should I close SciTE, or suspend it and `bg` and keep
it up?  But then I might forget that it's running and run `scite foo.py` again
and have two copies of `foo.py` open and out of sync and OK, I better just
close SciTE, I'll open it again for the next edit...

So — all in all, I thought, you know, maybe I could just find a bare-bones
editor, and add the things I *do* use to it, in ways that suit *me*.  And I
remembered yaedit, and that it was good enough, and small enough to hack on.
And, in the age of version control, maybe saving constantly — treating the
disk as if it were the main volatile store, the "core memory" for works of
text — is not a bad paradigm.  Maybe, in fact, it's a better one.  And maybe
with it, I can figure out a way to keep my editor running all the time instead
of starting and closing it randomly.

As a closing note, I have to say, it's a somewhat odd feeling, both acclimating
yourself to a new editor (oh but that urge to press Ctrl+S will die hard!), and
at the same time hacking on it, to acclimate *it* to *you*.

[nedit]: http://www.nedit.org/
[Orthodox Editors]: http://www.softpanorama.org/Articles/orthodox_editors.shtml
[pfh]: http://www.logarithmic.net/pfh/
[SciTE]: http://www.scintilla.org/SciTE.html
[The Humane Interface]: http://en.wikipedia.org/wiki/The_Humane_Interface
[yaedit]: http://www.logarithmic.net/pfh/yaedit 
