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
    
    Tab                    "productively" rewrite string to left of cursor
    Shift+Backspace        "destructively" rewrite string to left of cursor
    
    Ctrl+Z                 undo
    Ctrl+Shift+Z           redo
    Ctrl+X                 cut
    Ctrl+C                 copy
    Ctrl+V                 paste
    Ctrl+A                 select all
    
    Ctrl+Q                 quit tideay

Features
--------

*   Standard yaedit features
    
    *   Continual sync of file under edit to and from disk
    *   Awesome Ctrl+P multi-line-prefix editing
    *   No dialog boxes, etc; see [yaedit][] page for more
    
*   Rewrite-based editing
    
    In the editing pane, pressing the Tab key *rewrites* the string to
    the left of the cursor, based on some internal rewriting rules.  For
    example, `--` is replaced by `—`.  See the source code for the full set
    of rules.  The rewriting rules also implement tabs, in the following way:
    
    *   four spaces are replaced by eight spaces
    *   an empty string is replaced by four spaces
    *   a tab character is replaced by two tab characters
    *   `^I` rewrites to tab, so you can otherwise insert a real tab character
    
    What Tab does we call "productive" rewriting.  There is also a set of
    rules for "destructive" rewriting, which are applied when Shift+Backspace
    is pressed.  The only rule in this set, thus far, is replacing four spaces
    with an empty string.  This is useful for unindenting, especially when
    tideay thought you might want an indent but you really didn't.
    
*   New-block indenting
    
    Typing Enter after a `{` or `(` or `[` or `:`, or on a line that begins
    with `*` or `-` followed by three spaces, indents the next line (another)
    four spaces.  Most languages benefit from the brackets, Python benefits
    from the colon, and Markdown benefits from the `*`/`-` for lists.

*   Attempted workflow integration
    
    It's definitely not an IDE, but...
    
    Only one copy of tideay can be started from any given directory.  This
    is to discourage two copies from trampling each others' files.  (It
    doesn't outright prevent it, because you can open a file like
    `../otherproj/README`, but that case is less common.)

*   Command execution
    
    Typing Ctrl+Enter interprets the line (everything left of the cursor)
    as a shell command and executes it.  Note that this is a little
    experimental, and the exact usage may vary in the future, but for now,
    there are three styles:
    
    *   Ordinarily, the command that is executed is replaced by the standard
        output produced by running the command.  This is, among other things,
        a neat way to include snippets into the file
        (e.g. `cat snippet.txt` Ctrl+Enter).
    *   If the line begins with `%`, the command is not replaced; the output is
        appended underneath it, along with another line prefaced with `% `.
        The resultant effect of is that of a pseudo-shell of sorts.  This could
        be useful for inserting shell "how-to" instructions into documentation.
    *   If the line begins with `|`, the current file is piped into the command,
        and the *entire* buffer is replaced by the result.  (This is a single
        user action, so it can be undone with Ctrl+Z.)  This can be useful
        in oh-so-many ways, for example `|indent` to indent your C source code,
        `|grep -v blah` to delete all lines from the buffer containing `blah`,
        and so forth.
    *   If the line begins with `cd `, the current directory is changed to what
        follows this (if that text names a valid directory).  Since the current
        directory is a property of the process, this cannot be done by
        executing a `cd` command in the shell.

Other changes/improvements over yaedit
--------------------------------------

*   Editor pane uses a smaller font.  Window is initially sized to show 80
    columns (on my equipment; with different fonts or whatnot, YMMV.  Window
    size can be specified with the `TIDEAY_WIDTH` and `TIDEAY_HEIGHT`
    environment variables.)
*   Notebook's tab pane (on the left) has a fixed width.  Long filenames will
    not expand it and ruin the 80-column width of the editor pane.
*   Height of each label in the tab pane is smaller.
*   Only the basename (not the directory name) of the file is displayed in
    the label.  Full (relative) path to file is shown in the window title.
*   No tooltips (they don't show up on hover for me anyway.)  Controls are
    documented in this README for now.
*   The hashbang line of the file will be sniffed if the language cannot be
    determined from the the file extension.  Currently supports Python, Ruby,
    and shell.
*   Special read-only editor panes can show output from a periodically-run
    command.  The only one implemented so far is a pane that shows the
    output of `git diff`, created by Ctrl+D.

Protips
-------

*   To get Markdown syntax highlighting, install gtksourceview-3.0, and copy
    `markdown.lang` from its `language-specs` share directory into
    gtksourceview-2.0's `language-specs` share directory.  Also make sure your
    `/etc/mimetypes` file contains a line like this one:
    
        text/x-markdown            markdown md

Wishlist
--------

### display ###

*   When notebook tab pane exceeds height of window, you can only scroll down
    as far as the "open" entry.  You can't seem to use "find" etc properly.
    Ensuring the controls are visible, while only the buffer tabs scroll,
    would be nice (but it doesn't look like GtkNotebook supports this much.)
*   Visible whitespace (space, tab, LF) would be *really* nice.  Doesn't look
    like GtkSourceView supports changing the displayed appearance of a
    character, but can you make your own syntax highlighting rules, and could
    you make one for whitespace?

### editing ###

*   Pressing Tab or Shift-Tab with some text selected should not do whatever
    it is that GtkSourceView thinks should be done when that happens.
*   Check to see if a rewrite can happen for Tab/Shift+Bkspc.  If not, don't
    start a user action (less to undo.)
*   Ctrl+P will de-focus the editing pane, even if no lines/a single line
    is selected.  This can be annoying.  Not sure how to handle this yet.

### command execution ###

*   Replace `$1` (or something?) with the name of the current file.
*   Another variant that opens the result in a new buffer, and switches
    to it?  I'm thinking `hg diff $1`.  This would require a buffer
    temp filename strategy.  (And it would be nice if, in this case,
    the filename ended with `.diff`, to get the highlighting.)
    Maybe we could just check to see if the command string included
    a string like `>foo.txt` with spaces on either side, and use that
    as the name.
    
### find and replace ###

*   **Ctrl+F find should populate find-entry with selected text.**
*   Find should support case-sensitive and maybe whole-world-only search.
*   Replace found text with new text.  (This can be done with `|sed`,
    but I don't like `sed`, so maybe supply a command-line tool with
    a simpler interface, like `replace foo bar`.)
*   Mark and move to mark (like Ctrl+F2/F2 in SciTE)
*   Next and Prev should have keyboard shortcuts.

### buffers ###

*   **Some way to switch to a buffer by typing in part of its filename.**
*   Should not be possible to open 2 copies of the same file.
*   Should be able to populate buffers from all "interesting" files in the
    current directory tree.  (repository <=> workspace)
*   Some nice way to move/copy a buffer while inside the editor.
*   When file backing buffer is deleted by the system, close the buffer?
    (Argument against: maybe it was accidental and buffer is only remaining
    copy of what was there.  Maybe "deaden" the buffer.  Maybe recreate the
    file...)
*   When opening a new file, cursor position is at bottom of file, but
    view is scrolled to top of file.  Maybe move cursor to top, too.

### other wild ideas ###

*   One-letter commands could be dealt with specially, so `|r foo bar`
    replaces text.  Or something.
*   Workspace files, that contain a list of files, and open them all when
    the workspace file is opened.  It could also contain rewrite rules.
    And be a good place for jotting miscellaneous notes and running
    miscellaneous commands without wrecking a buffer.
*   Plugins.  These should just be Python files, in a subdirectory of your
    home directory (maybe?) which are sourced upon startup.  tideay could
    ship with some, but you'd have to install them yourself.  The rewriting
    maps could be among them, so you could override the builtin rewrite rules
    with your own.  But the builtin ones could also be plugins, in a dir in
    the distribution... which tideay also reads... or something.  But then,
    this is *my* editor, so, why?  If someone else wants to fork it, that's
    great, and they can just make the changes to their fork.  I dunno.
*   VTE widget.  For... I don't know.  The thing is, the shell commands do
    not stream their output, and if you say, like, `nano` Ctrl+Enter, it's
    your funeral.  So maybe the commands could be run in a terminal pane,
    with their output `tee`d to a file, and it's this file that's inserted
    when it's done.  This would also let you run commands which require a
    password to be entered.
*   Key combination to select URL under cursor (double/triple-clicking doesn't
    cut it; Ctrl+clicking might) and (further key combination to?) open it
    using the default browser.
*   Should destructive rewriting be the inverse map of productive?  If
    `''` is a special case in the map then this could work, because `''`
    rewrites to four spaces.
*   Changing directory should support toolshelf — look to see if `toolshelf`
    is present on the path, and if so, call `toolshelf pwd` and see if that
    returns a sensible result, and if so, change dir to that instead.

### low priority ###

*   Indicate the 80-column mark in the editing pane.
*   Some way to force source language highlighting.
*   Show column position of cursor.

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
