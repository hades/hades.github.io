---
layout: post
title: Git and Kompare
---
Everyone knows, that [Git](https://git-scm.com) is the best version control
system, and [KDE](https://kde.org) is the best desktop environment out there.

So recently I had a nice idea: wouldn’t it be nice if git diffs could be viewed
by KDE file comparison and diff viewer tool called
[Kompare](http://www.caffeinated.me.uk/kompare/)? Sure it would.

Still don’t know if it possible by editing Git configuration only, but I did a
little bash function that does it (put in your .bashrc):

    kg(){
      file=`mktemp`
      git "$@" > $file
      kompare $file
      rm $file
    }

Use as follows: ```kg show -v```, or ```kg diff HEAD^```.
