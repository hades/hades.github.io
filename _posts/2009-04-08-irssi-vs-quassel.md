---
layout: post
title: Irssi vs. Quassel, or There and Back Again
---
Once there was [irssi](http://irssi.org/). And it was good. I liked the way I
could leave it running in screen and use it wherever there was a working
ssh-client. I liked the plain and simple Perl scripting and tons of scripts
available. I liked the way other people reacted seeing IRC client in terminal.
It even tab-completed file names.

But there wasn’t everything fine with it. For once, I tended to forget I have
something running in terminal. Highlights, being confined to konsole window,
were unable to attract any of my attention. Also copying and pasting text
sucked a little, but that’s more of a nit-picking.

> ’… I am looking for someone to share in an adventure that I am arranging,
> and it’s very difficult to find anyone.’

Then I heard of [Quassel](http://quassel-irc.org/), and it seemed perfect. It
ran fine,  supported many stuff like SSL, automatic NickServ identification,
had a nice concepts of “Buffer views” and “Monitors”.  Scripts were not
available until very recently, but I experienced no special need for them.
Tab-completion worked only alphabetically and sucked at non-english nicks, but
that was fixed. 

First time I noticed something was wrong was when Quassel core [crashed having
ran out of disk space](http://bugs.quassel-irc.org/issues/show/358). The
developers’ position on this was that Quassel relying heavily on database
backend can’t work when the database is unwritable. In short: whenever Quassel
can’t log your message, it dies. That struck me as not quite reasonable, but
it’s manageable. 

Quassel’s database-orientedness makes it also eat a lot of disk I/O and space.
And I don’t even  mention RAM usage. So it is hardly usable on slow hardware at
all. 

I can also mention a hard [Qt](http://www.qtsoftware.com/) dependency of all
the Quassel’s internals. Can’t say that is bad, since Qt is a great framework,
and I suppose it made developing Quassel more fun and decreased PITA
coefficient. Still, you can hardly imagine implementing Quassel-compatible
client or core without Qt. So you lose all the devices, Qt hasn’t been ported
on. 

> To the end of his days Bilbo could never remember how he found himself
> outside, without a hat, a walking stick or any money, or anything that he
> usually took when he went out; leaving his second breakfast half-finished and
> quite unwashed-up, pushing his keys into Gandalf’s hands, and running as fast
> as his furry feet could carry him down the lane, past the great Mill, across
> The Water, and then on for a mile or more.

Summing up, I am back to irssi now. It uses about five times less RAM, it
doesn’t eat up disk I/O, logs take substantially less than 100 megabytes per
month (and can be grepped and gzipped and cleaned).  I can run it on my router,
I can connect to it having just ssh, or Putty. I can even connect to it [using
any other IRC client](http://www.irssi.org/documentation/proxy), which solves
most of the old problems.

Of course it doesn’t download logs when I scroll up, it doesn’t have nice
search with nice highlights, it doesn’t have nice web previews. But I think
I’ll live with that. Or even hack into xchat to make it do nice things Quassel
can do.

Can’t blame Quassel developers for anything. They are great devoted guys that
made a great IRC client, and they continue working on it, so if you don’t care
about bad things I’ve said here (or if you have powerful server, tons of disk
space, love to use SQL to grep logs and own only Qt 4.4 powered devices) then
Quassel is for you.

> You are a very fine person, Mr Baggins, and I am very fond of you; but you
> are only quite a little fellow in a wide world after all!’
>
> ‘Thank goodness!’ said Bilbo laughing, and handed Gandalf the
> tobacco-jar.
