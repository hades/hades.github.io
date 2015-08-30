---
layout: post
---
So, let's start with setting up [gitolite](https://github.com/sitaramc/gitolite) to host your repositories and provide SSH access to them. There is also a tool called gitosis, but it is not as supported and featurefull as gitolite. 

So what does it actually do? In essence, all the developers that will access your repositories will login to your server as user "git", run server-side git daemon and sync data with it. Gitolite will determine if a user has access to perform operations he is going to. Ok, but why bother with all this crap, and not just create a bunch of users on the server and tell them where is the repository? There is a couple of reasons.

Firstly, you may not want to give your developers shell access to your server. Why wouldn't you? Who knows, probably because they are stupid or malicious, or whatever. With gitolite they won't get a shell access, because they will only be able to login with their SSH keys and that SSH keys will be restricted to invoking gitolite only. At this point you may want to read up SSH manual on keys and command restrictions, if you do not yet know about it.

Secondly, you may want to impose certain access restrictions on them. Gitolite allows to restrict read write and rewrite down to per-branch level.

Thirdly, with local multiuser git access you have to fiddle a little with umasks or write a hook that fixes the permissions, but that's unimportant and uninteresting.

Fourthly, you may even not have a root access. That's right, gitolite allows you to setup multi-user repositories on systems where you have only one non-root account.

So let's go ahead with installation. The [INSTALL](https://github.com/sitaramc/gitolite/blob/pu/doc/1-INSTALL.mkd) doc is rather straightforward and you should in fact read it instead of this blog post. <b>I am not offering the text below as a setup instructions, but merely as a log of how I installed it on my server.</b>

First off, we require some git on our server. Please be careful to use as fresh git as possible, because it generally tends to gets even more super awesome with time. Also gitolite requires at least git 1.6.2 at the time of writing of this post. Unless you use especially sucky distribution (*ahem* debian *ahem*), you should be fine with git from your repository. I just told Portage to install me one:

<div class="code"><pre>
    emerge -av git
</pre></div>

Although gitolite has an ebuild in Gentoo, I set it up manually and will walk you along the procedure. First, lets obtain the sources:

<div class="code"><pre>
    git clone git://github.com/sitaramc/gitolite.git
</pre></div>

This will create a "gitolite" directory with gitolite sources. Let's create a system directory for gitolite. I use /opt for all non-Portage packages, so let's go ahead with:

<div class="code"><pre>
    GITOLITE=/opt/gitolite
    sudo mkdir -p ${GITOLITE}/{bin,conf,hooks}
    sudo chown -R `whoami` ${GITOLITE}/{bin,conf,hooks}
</pre></div>

You may of course use another set of directories if you want. Next, we let gitolite install itself wherever we want it:

<div class="code"><pre>
    ./src/gl-system-install ${GITOLITE}/{bin,conf,hooks}
</pre></div>

This takes three arguments: directory for binaries, directory for configs and directory for hooks. Also this is the same command you would use to update your gitolite installation after you git pull the new sources.

We would now require a user account for git. Let's create them!

<div class="code"><pre>
    sudo groupadd -r git
    sudo useradd -d /srv/git -g git -m -r -s `which bash` git
</pre></div>

This creates a system (-r) group git and a system (-r) user git with home directory (-d) /srv/git and shell (-s) bash, in group (-g) git, and creates its home directory (-m). Now login into your new git account. Don't forget to add your ${GITOLITE}/bin directory to the .bashrc if it is not in the PATH:

<div class="code"><pre>
    sudo su - git
    echo PATH='${PATH}':/opt/gitolite/bin/ >> .bashrc
    source .bashrc
    which gl-setup
</pre></div>

Now copy over your public key to a file called username.pub and finalize gitolite setup by running:

<div class="code"><pre>
    gl-setup username.pub
</pre></div>

After that, follow the instructions, they should be fairly straightforward. Congratulations, you're done! The gitolite's configuration is stored in a Git repository under gitolite, so you'd want to clone it from your computer (i.e. where you have the private key for username.pub):

<div class="code"><pre>
    git clone git@yourservername:gitolite-admin
    cd gitolite-admin
    vim conf/gitolite.conf
</pre></div>

The file gitolite.conf is in fact your gitolite config file. After you edit it, save it, commit it and push to the gitolite-admin repository on your server, gitolite checks the config and makes appropriate changes to the repositories. To add users, just place their keys into keydir directory. It's that simple! You probably would want to read [official admin doc](https://github.com/sitaramc/gitolite/blob/pu/doc/2-admin.mkd) just about now.