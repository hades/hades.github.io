---
layout: page
title: "Euterpe"
---
# Euterpe #

Euterpe is a Python framework for synchronizing music files with a portable
music player.

## Status ##

This started as a script for personal uses and hasn't yet evolved into anything
bigger. You can download it and give it a spin (see Usage, but don't expect
anything extraordinary from it.)

For me the only task Euterpe does is this:

 * scan the specified source directory for files named "directory.euterpeidx",
 * check if corresponding files already exist in the specified target directory,
 * copy missing files from source directory to destination,
 * recode them to supported formats if necessary (recoded file is saved in cache).

Although at first I tried to maintain a decent level of abstraction in Euterpe,
at a certain moment I got bored and just hacked the remaining pieces together.
So there is some weird code there (see Drawbacks)

## Usage ##

 * Download the latest snapshot:

       http://git.hades.name/cgit.cgi/euterpe.git/snapshot/euterpe-master.tar.bz2

   and unpack it.
 * In a directory with your music files create file named
   "directory.euterpeidx" listing all the files you want on your media device.
   For example:

       $ cd $HOME/Music
       $ ls
       Album1 Album2
       $ cd Album1
       $ ls
       Track1.flac
       Track2.flac
       Track3.flac
       directory.euterpeidx
       $ cat directory.euterpeidx
       Track1.flac

 * Run the examples/test.py script. Like this:

       $ ls
       COPYING euterpe examples
       $ PYTHONPATH=. python examples/test.py $HOME/Music /mnt/player/music
       [ N C  ] Artist1 - Track1
       [ N    ] Artist2 - Track2
       [ N C  ] Artist2 - Track3
       Total: 3 (3 new, 2 to encode)

 * Press Enter and wait until it finishes.

## Drawbacks ##

 * There are some obvious hacks, for example the Russian letters
   transliterator,

## Future ##

 * Support for video can be implemented rather easily

## Development ##

You can get the sources with the following command:

    git clone git://git.hades.name/euterpe.git

