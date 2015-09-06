---
layout: post
title: "Your Own Git Hosting: Prologue"
---
As you might know, Git itself does not deal with collaboration, letting you
choose any adequate model. So how does one go about publishing his repository?
Or letting someone push into their repository? The answers are multiple and
confusing. [Official Git
Manual](http://www.kernel.org/pub/software/scm/git/docs/user-manual.html#sharing-development)
gives a dry summary of the basic options, which you may read now, or at your
leisure. I'll just note the few key points.

Limiting the discussion to network exchange, Git supports three basic
protocols: git protocol, HTTP and SSH. All of them support pulling and pushing,
however each of them has drawbacks. Git protocol does not support any kind of
authentication, so it is mostly used for read-only public access. HTTP is slow
and stupid (which means that it allows only file transfer, so you can't use Git
hooks). SSH is great in all respects, but it implies that every person that has
access to your repository has an account on the server, where it is hosted.

In the majority of the cases, two protocols are set up for Git repository:
public read-only git protocol access and private read/write SSH access. However
I can name you upon request at least one real-world case, when git protocol was
successfully used for unauthenticated read/write access :). HTTP protocol is
usually used in those morbid cases where you can in no conceivable way get rid
of it (imagine a firewall that blocks everything except port 80 or 443).

So how do you setup a Git hosting? There are a lot of web Git hostings
([gitorious.org](http://gitorious.org), [github.com](http://github.com)), and
project hostings with Git support ([sf.net](http://sf.net)) out there. Mostly
they provide free hosting for open-source projects and charge a fee for private
repositories.

However, public Git hostings can not satisfy everyone. There are closed
projects, that could not be trusted to a third-party. There are closed projects
that do not have spare money for Git hosting. Also, it is hard to integrate
hosted Git repository with other development tools (buildbot, bugzilla, etc.),
mainly because custom hooks are not supported. So that brings us to the
following problem.

Given:

- a server.

Required:

- hosting of any number of Git repositories;
- public read-only access to a subset of them via git protocol;
- read/write access for authorized users via SSH;
- web interface to the repositories.

In the following posts, I will tell you how I solved the problem for myself
using the following tools:

- [gitolite](http://github.com/sitaramc/gitolite) for SSH access management;
- [git-daemon](http://www.kernel.org/pub/software/scm/git/docs/git-daemon.html)
  for anonymous git protocol access;
- [cgit](http://hjemli.net/git/cgit/) for web interface.

Please stay tuned!

P.S: I've heard some concerns regarding cgit not working properly in some
cases. I can neither confirm this nor disprove (since I rarely use web
frontends for code browsing), but I'll look into it.
