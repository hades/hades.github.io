---
layout: post
title: Trac Whine Script
---
Have you ever wanted a script that sends an email to every your
[Trac](http://trac.edgewall.org/) user reminding them of all their
open-unsolved tickets? Have no fear, here it is:

    #!/bin/bash

    ending(){
      case $1 in
        1)
          echo open ticket
          ;;
        *)
          echo open tickets
          ;;
        esac
    }

    fromaddr='trac@hades.name'
    from="Trac Whiner <$fromaddr>"
    dbpath=/var/trac/main/db/trac.db
    sqlite=`which sqlite3`
    sendmail=/usr/sbin/sendmail
    tracurl='https://trac.hades.name'
    lockfile=/tmp/notify-all.lock

    [ -e $lockfile ] && exit 0
    touch $lockfile

    request="SELECT value, id, summary
              FROM ticket
              JOIN session_attribute
              WHERE status IN ( 'new', 'accepted' )
               AND name='email'
               AND sid=owner;"
    tmpfile=`mktemp`

    $sqlite "$dbpath" "$request" > $tmpfile

    cat $tmpfile | cut -d\| -f 1  | sort -u | while read email; do
        count=`grep "^$email" $tmpfile | wc -l`
        mailfile=`mktemp`

        cat >> $mailfile <<ENDOFLINE
    From: "${from}"
    To: ${email}
    Content-Type: text/plain; charset=utf-8
    Subject: [Trac Whine] You have $count $(ending $count).

    Howdy there!

    Don't want to bother you, but you still have $count $(ending $count):
    ENDOFLINE

        grep "^$email" $tmpfile | while read data; do
            number=` echo "$data" | cut -d\| -f 2`
            summary=` echo "$data" | cut -d\| -f 3-`

            echo "$tracurl/ticket/$number ($summary)" >> $mailfile
        done

        cat >> $mailfile <<ENDOFLINE

    Have a nice work day!

    --
    Automatic Trac Whiner <$tracurl>

    ENDOFLINE

        cat $mailfile | $sendmail -t -f "$from"

        rm $mailfile
    done

    rm $tmpfile
    rm $lockfile

Don't forget to alter "fromaddr", "dbpath" and "tracurl" variables to your liking and enjoy!
