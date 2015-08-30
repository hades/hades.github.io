---
layout: post
---
Everyone knows, that <a href="http://git-scm.com">Git</a> is the best version control system, and <a href="http://kde.org">KDE</a> is the best desktop environment out there.

So recently I had a nice idea: wouldn’t it be nice if git diffs could be viewed by KDE file comparison and diff viewer tool called <a href="http://www.caffeinated.me.uk/kompare/">Kompare</a>? Sure it would.

Still don’t know if it possible by editing Git configuration only, but I did a little bash function that does it (put in your .bashrc):

<div class="code">kg(){<br />
&nbsp;&nbsp;file=`mktemp`<br />
&nbsp;&nbsp;git "$@" > $file<br />
&nbsp;&nbsp;kompare $file<br />
&nbsp;&nbsp;rm $file<br />
}</div>


Use as follows: <code>kg show -v</code>, or <code>kg diff HEAD^</code>.