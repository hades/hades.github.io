---
layout: post
title: "This client is too old to work with working copy"
---
Did you know, that in subversion you get this error message if you create a
working copy with a new client and then try to do something with it using an
old client? Seems screwed up but pretty reasonable, doesn't? But get this:
subversion will update your working copy even if you do something like "svn
diff" or "svn ls" there with a newer client. And then your older client will
bail out.

That's pretty damn fucked up. That's one of the design decisions you should not
be allowed to make. I mean, subversion is actually pretty dumb by itself. So
what was so special needed to be changed in those crappy .svn directories, so
that old clients cannot understand it?

By the way, if you accidentally came here googling the error message: sorry,
you'll have to deal with this shit until you move to a less crappy version
control software.
