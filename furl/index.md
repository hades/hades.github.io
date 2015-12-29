---
layout: page
title: "furl"
---
furl is a set of libraries and tools to collect and dispatch URLs mentioned in
various media (i.e. IM, IRC, identi.ca, etc.).

## Introduction

When you talk to people over the internet, they often talk in URLs. On average,
every twentieth line contains a URL*. Most of the IRC and XMPP clients
highlight URLs and allow you to click on them. Some even have a URL list, which
is populated by every URL your client sees.

Of course, nobody likes clicking because mouse is a stupid and slow device. So
it would be much better, if such a list of URLs could be invoked with a key
combination. And if every client used the same list, so all URLs end up in the
same place.

This is what furl is for. It is a library that stores URLs from whichever
application decided to give one to it. It then allows any application to access
this list of URLs to do whatever it wants with it. The natural thing to do is
to show it to user and/or open a browser.

\* — I totally made that up.

## Development

You can get the sources with the following command:

    git clone git://git.hades.name/furl.git
