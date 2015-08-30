---
layout: post
---
Once there was <a href="http://irssi.org/">irssi</a>. And it was good. I liked the way I could leave it running in screen and use it wherever there was a working ssh-client. I liked the plain and simple Perl scripting and tons of scripts available. I liked the way other people reacted seeing <span  class="caps">IRC</span> client in terminal. It even tab-completed file names.

But there wasn’t everything fine with it. For once, I tended to forget I have something running in terminal. Highlights, being confined to konsole window, were unable to attract any of my attention. Also
copying and pasting text sucked a little, but that’s more of a nit-picking.

<div style="margin: auto; width: 80%; font-size: 80%;">‘ … I am looking for someone to share in an adventure that I am arranging, and it’s very difficult to find anyone.’</div>

Then I heard of <a href="http://quassel-irc.org/">Quassel</a>, and it seemed perfect. It ran fine,  supported many stuff like <span class="caps">SSL</span>, automatic NickServ identification, had a nice concepts of “Buffer views” and “Monitors”. Scripts were not available until very recently, but I experienced no special need for them. Tab-completion worked only alphabetically and sucked at non-english nicks, but that was fixed. 

First time I noticed something was wrong was when Quassel core <a href="http://bugs.quassel-irc.org/issues/show/358">crashed having ran out of disk space</a>. The developers’ position on this was that Quassel relying heavily on database backend can’t work when the database is unwritable. In short: whenever Quassel can’t log your message, it dies. That struck me as not quite reasonable, but it’s manageable. 

Quassel’s database-orientedness makes it also eat a lot of disk I/O and space. And I don’t even  mention <span class="caps">RAM</span> usage. So it is hardly usable on slow hardware at all. 

I can also mention a hard <a href="http://www.qtsoftware.com/">Qt</a> dependency of all the Quassel’s internals. Can’t say that is bad, since Qt is a great framework, and I suppose it made developing Quassel more fun and decreased <span class="caps">PITA</span> coefficient. Still, you can hardly imagine implementing Quassel-compatible client or core without Qt. So you lose all the devices, Qt hasn’t been ported on. 

<div style="margin: auto; width: 80%; font-size: 80%;">To the end of his days Bilbo could never remember how he found himself outside, without a hat, a walking stick or any money, or anything that he usually took when he went out; leaving his second breakfast half-finished and quite unwashed-up, pushing his keys into Gandalf’s hands, and running as fast as his furry feet could carry him down the
lane, past the great Mill, across The Water, and then on for a mile or more.</div>

Summing up, I am back to irssi now. It uses about five times less <span class="caps">RAM</span>, it doesn’t eat up disk I/O, logs take substantially less than 100 megabytes per month (and can be grepped and gzipped and cleaned). I can run it on my router, I can connect to it having just ssh, or Putty. I can even connect to it <a href="http://www.irssi.org/documentation/proxy">using any other  <span class="caps">IRC</span> client</a>, which solves most of the old problems.

Of course it doesn’t download logs when I scroll up, it doesn’t have nice search with nice highlights, it doesn’t have nice web previews. But I think I’ll live with that. Or even hack into xchat to make it do nice things Quassel can do.

Can’t blame Quassel developers for anything. They are great devoted guys that made a great <span  class="caps">IRC</span> client, and they continue working on it, so if you don’t care about bad things I’ve said here (or if you have powerful server, tons of disk space, love to use <span class="caps">SQL</span> to grep logs and own only Qt 4.4 powered devices) then Quassel is for you.

<div style="margin: auto; width: 80%; font-size: 80%;">‘You are a very fine person, Mr Baggins, and I am very fond of you; but you are only quite a little fellow in a wide world after all!’<br />

‘Thank goodness!’ said Bilbo laughing, and handed Gandalf the tobacco-jar.</div>