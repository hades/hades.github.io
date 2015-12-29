---
layout: page
title: "Bird"
---
bird is a simple interactive console Twitter client.

## Status ##

This is a fully-functional modular Twitter client, used by me
([dr_lepper](https://twitter.com/dr_lepper)).


## Features ##

 * Displaying tweets and sending own tweets,
 * Color highlighting of replies, own tweets and replies to you,
 * Full retweets support,
 * Executing arbitrary Python statements,
 * (module autostatus) Setting your IM status to your last tweet or retweet,
 * (module blacklist) Blacklisting tweets by author (useful to filter out
   unwanted retweets) or any other field (useful to filter out girls' tweets
   about Apple and Starbucks),
 * (module browser) Opening URLs from tweets in your browser.
 * (module delete) Deleting own tweets,
 * (module follow) Following/unfollowing users,
 * (module longtweets) Automatic splitting of long tweets,
 * (module threads) Unfolding conversation threads,
 * (module translate) Translating tweets using Google Translate,
 * (module unshorten) Automatic unshortening of shortened URLs,
 * (module userpics_aa) Display of userpics in ASCII-art using libaa.


## Usage ##

bird comes in one single file bird.py. You can download it here:

    https://git.hades.name/cgit.cgi/bird.git/plain/bird.py
    
Then just simply run it.


### Prerequisites ###

bird has one dependency:

 * twython ([github](https://github.com/ryanmcgrath/twython)).

### Built-in commands ###

Commands start with symbol '/'. **Everything else will be posted to your
Twitter.**

 * `/exec <expression>` — evaluate an arbitrary Python expression. Don't forget to use print() if you want something to be output.
 * `/help` — show the list of available commands
 * `/load <modulename>` — load external module `modulename`
 * `/quit` — quit
 * `/re <key> <text>` — reply to a status with key `key` (keys are those
   two-character codes in braces before the tweets)
 * `/rt <key>` — retweet a status
 * `/save` — save changed configuration variables
 * `/set <variable> <value>` — set a variable to a given value. Omit the new
   value to show current value


### Built-in variables ###

Variables are stored (by default) in `~/.config/birdrc` in an INI-file. To
access them in runtime use `/set`. To save changed values use `/save`.

 * `mentions` (on/off) — include mentions in the timeline
 * `modules` — list of modules to load at startup
 * `colors` (on/off) — colorize output
 * `keylen` — length of tweet keys (default is 2), increase if you have too
   many new tweets at startup

Various colors can be also changed in the config:

 * `color.author` — author of the status (the text between angle braces)
 * `color.error` — error messages
 * `color.own` — tweets by YOU
 * `color.reply` — replies to another status
 * `color.tweet` — ordinary tweets
 * `color.warn` — warning messages


## Modules ##

### autostatus ###

Retrieves your last tweet and sets your current status in IM clients to its
text.

#### Supported IM clients ####

 * [gajim](http://gajim.org) (`/set autostatus.client gajim`) (note that
   D-Bus support is required),
 * [Psi](http://psi-im.org) (`/set autostatus.client psi`),
 * test client (`/set autostatus.client test`) — does nothing but print what
   would autostatus set your status to, had you configured it properly.

#### Variables ####

 * `autostatus.client` — type of IM client (see above),
 * `autostatus.ignorereplies` — if True, autostatus will not set your
   status to your last tweet if it is a reply to another Twitter user.


### blacklist ###

Filters tweets based on user ID or any other tweet field.

#### Commands ####

 * `/blacklist <username>` — add given user to the blacklist. Omit
   username to show current blacklist.
 * `/blackfield <field> <regexp>` — add given list to the blacklist
   entries for field `field`. Omit `regexp` to remove all blacklist
   entries.

*Blacklist is stored in config.* Don't forget to save it with `/save`


### browser ###

**Author:** Tatiana Gornak

Opens the URL from the given tweet with your browser.

#### Commands ####

 * `/browse <key> <url numbers>` — open URLs with given
   numbers from tweet with key `key`. If there is only one
   URL in tweet, you can omit the numbers

#### Variables ####

 * `browser.agent` — executable name to open URLs with,
 * `browser.foreground` (on/off) — when on, Bird is suspended until your
   browser quits,
 * `browser.verify` (on/off) — when on, Bird will first try to open URL itself
   to check if it exists.


### delete ###

**Author:** Tatiana Gornak

Allows you to delete your tweets.

#### Commands ####

 * `/delete <key>` — delete tweet with key `key`


### follow ###

Allows you to edit your followers list.

#### Commands ####

 * `/follow <username>` — start following `username`
 * `/unfollow <username>` — stop following `username`


### longtweets ###

Splits long tweets into several shorter tweets.


### threads ###

Unfold the conversation threads.

#### Commands ####

 * `/thread <key>` — trace the conversation that preceeds the given tweet
   `key`.


### translate ###

**Author:** Tatiana Gornak

Translates the tweets.


### unshorten ###

Replaces short URLs in domains like bit.ly and goo.gl into the full URLs.

#### Variables ####

 * `unshorten.allow` — a space-separated list of additional domains that are
    URL shorteners

#### Commands ####

 * `/translate <key> <from>-<to>` — translates tweet with key `key` from
    language `from` to `to`.

#### Variables ####

 * `translate.from` — the default language to translate from. If you set this,
    you can omit it in `/translate` command.
 * `translate.to` — the default language to translate to. If you set both these
    variables, you can omit the language spec as a whole.


### unshorten ###

Replaces short URLs in domains like bit.ly and goo.gl into the full URLs.

#### Variables ####

 * `unshorten.allow` — a space-separated list of additional domains that are
    URL shorteners


### userpics_aa ###

Draws userpics of your friends in ASCII-art using Aalib.

#### Prerequisites ####

 * python-aalib
   ([http://jwilk.net/software/python-aalib](http://jwilk.net/software/python-aalib))
 * Python Imaging Library
   ([http://www.pythonware.com/products/pil/](http://www.pythonware.com/products/pil/))

#### Variables ####

 * `userpics_aa.width`, `userpics_aa.height` (integer) — dimensions (in
    characters) of the drawings


### verify ###

Prints you everything bird is going to do, so that you can double-check it.

#### Commands ####

 * `/commit` — send everything bird just asked you to verify


## Acknowledgements ##

The project is largely a Python rewrite of
[ttytter](http://www.floodgap.com/software/ttytter/) by Floodgap.

## Development ##

You can get the sources with the following command:

    git clone git://git.hades.name/bird.git
