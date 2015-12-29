---
layout: post
title: "Cookbook"
---
Here I gather some valuable code snippets and recipes.

Despite it being 2012 outside, Python interactive interpreter still does not
save command history and does not Tab-complete.

However, all the gears are inside, you just need to enable them.

Here is how.

You need to create a "python startup" file, which contains the following:


    def wow_history_and_tabs():
        import atexit
        import os.path
        import readline
     
        histfile = os.path.expanduser('~/.python_history')
        try:
            readline.read_history_file(histfile)
        except IOError:
            pass
        readline.parse_and_bind('tab: complete')
        atexit.register(readline.write_history_file, histfile)
    
    wow_history_and_tabs()
    del wow_history_and_tabs

What it does is basically four things:

 * Load history from your ".python_history" file in home dir.
 * Enable tab-completion.
 * At exit, write history back to the history file.
 * Leave clean globals for your interactive session.

Next, you would want this to be parsed and executed at every python startup. To
achieve this, set the following environment variable (in your ~/.bashrc or
whatever):

    PYTHONSTARTUP=$HOME/.pythonrc

(supposing, of course, that you've saved the previous file under ~/.pythonrc)

That's it, next time your python will understand arrows and Tab.
