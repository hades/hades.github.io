---
layout: post
---
Have you ever wanted a script that sends an email to every your <a href="http://trac.edgewall.org/">Trac</a> user reminding them of all their open-unsolved tickets? Have no fear, here it is:

<div class="code"><pre class="hl"><span class="hl slc">#!/bin/bash</span>

ending<span class="hl sym">(){</span>
    <span class="hl kwa">case</span> <span class="hl kwb">$1</span> <span class="hl kwa">in</span>
        <span class="hl num">1</span><span class="hl sym">)</span>
            <span class="hl kwb">echo</span> open ticket
            <span class="hl sym">;;</span>
        <span class="hl sym">*)</span>
            <span class="hl kwb">echo</span> open tickets
            <span class="hl sym">;;</span>
    <span class="hl kwa">esac</span>
<span class="hl sym">}</span>

fromaddr<span class="hl sym">=</span><span class="hl str">'trac&#64;hades.name'</span>
from<span class="hl sym">=</span><span class="hl str">&quot;Trac Whiner &lt;<span class="hl kwb">$fromaddr</span>&gt;&quot;</span>
dbpath<span class="hl sym">=/</span>var<span class="hl sym">/</span>trac<span class="hl sym">/</span>main<span class="hl sym">/</span>db<span class="hl sym">/</span>trac.db
sqlite<span class="hl sym">=</span><span class="hl str">`which sqlite3`</span>
sendmail<span class="hl sym">=/</span>usr<span class="hl sym">/</span>sbin<span class="hl sym">/</span>sendmail
tracurl<span class="hl sym">=</span><span class="hl str">'https://trac.hades.name'</span>
lockfile<span class="hl sym">=/</span>tmp<span class="hl sym">/</span>notify<span class="hl sym">-</span>all.lock

<span class="hl sym">[ -</span>e <span class="hl kwb">$lockfile</span> <span class="hl sym">] &amp;&amp;</span> <span class="hl kwb">exit</span> <span class="hl num">0</span>
<span class="hl kwc">touch</span> <span class="hl kwb">$lockfile</span>

request<span class="hl sym">=</span><span class="hl str">&quot;SELECT value, id, summary</span>
<span class="hl str">          FROM ticket</span>
<span class="hl str">          JOIN session_attribute</span>
<span class="hl str">          WHERE status IN ( 'new', 'accepted' )</span>
<span class="hl str">           AND name='email'</span>
<span class="hl str">           AND sid=owner;&quot;</span>
tmpfile<span class="hl sym">=</span><span class="hl str">`mktemp`</span>

<span class="hl kwb">$sqlite</span> <span class="hl str">&quot;<span class="hl kwb">$dbpath</span>&quot;</span> <span class="hl str">&quot;<span class="hl kwb">$request</span>&quot;</span> <span class="hl sym">&gt;</span> <span class="hl kwb">$tmpfile</span>

<span class="hl kwc">cat</span> <span class="hl kwb">$tmpfile</span> <span class="hl sym">|</span> cut <span class="hl sym">-</span>d\<span class="hl sym">| -</span>f <span class="hl num">1</span>  <span class="hl sym">|</span> <span class="hl kwc">sort</span> <span class="hl sym">-</span>u <span class="hl sym">|</span> <span class="hl kwa">while</span> <span class="hl kwb">read</span> email<span class="hl sym">;</span> <span class="hl kwa">do</span>
    count<span class="hl sym">=</span><span class="hl str">`grep &quot;^<span class="hl kwb">$email</span>&quot; <span class="hl kwb">$tmpfile</span> | wc -l`</span>
    mailfile<span class="hl sym">=</span><span class="hl str">`mktemp`</span>

    <span class="hl kwc">cat</span> <span class="hl sym">&gt;&gt;</span> <span class="hl kwb">$mailfile</span> &lt;&lt;ENDOFLINE
<span class="hl str">From: &quot;<span class="hl kwb">${from}</span>&quot;</span>
<span class="hl str">To: <span class="hl kwb">${email}</span></span>
<span class="hl str">Content-Type: text/plain; charset=utf-8</span>
<span class="hl str">Subject: [Trac Whine] You have <span class="hl kwb">$count</span> $(ending <span class="hl kwb">$count</span>).</span>
<span class="hl str"></span>
<span class="hl str">Howdy there!</span>
<span class="hl str"></span>
<span class="hl str">Don't want to bother you, but you still have <span class="hl kwb">$count</span> $(ending <span class="hl kwb">$count</span>):</span>
ENDOFLINE

    <span class="hl kwc">grep</span> <span class="hl str">&quot;^<span class="hl kwb">$email</span>&quot;</span> <span class="hl kwb">$tmpfile</span> <span class="hl sym">|</span> <span class="hl kwa">while</span> <span class="hl kwb">read</span> data<span class="hl sym">;</span> <span class="hl kwa">do</span>
        number<span class="hl sym">=</span><span class="hl str">` echo &quot;<span class="hl kwb">$data</span>&quot; | cut -d\| -f 2`</span>
        summary<span class="hl sym">=</span><span class="hl str">` echo &quot;<span class="hl kwb">$data</span>&quot; | cut -d\| -f 3-`</span>

        <span class="hl kwb">echo</span> <span class="hl str">&quot;<span class="hl kwb">$tracurl</span>/ticket/<span class="hl kwb">$number</span> (<span class="hl kwb">$summary</span>)&quot;</span> <span class="hl sym">&gt;&gt;</span> <span class="hl kwb">$mailfile</span>
    <span class="hl kwa">done</span>

    <span class="hl kwc">cat</span> <span class="hl sym">&gt;&gt;</span> <span class="hl kwb">$mailfile</span> &lt;&lt;ENDOFLINE
<span class="hl str"></span>
<span class="hl str">Have a nice work day!</span>
<span class="hl str"></span>
<span class="hl str">--</span>
<span class="hl str">Automatic Trac Whiner &lt;<span class="hl kwb">$tracurl</span>&gt;</span>
<span class="hl str"></span>
ENDOFLINE

    <span class="hl kwc">cat</span> <span class="hl kwb">$mailfile</span> <span class="hl sym">|</span> <span class="hl kwb">$sendmail</span> <span
    class="hl sym">-</span>t <span class="hl sym">-</span>f <span class="hl str">&quot;<span class="hl kwb">$from</span>&quot;</span>

    <span class="hl kwc">rm</span> <span class="hl kwb">$mailfile</span>
<span class="hl kwa">done</span>

<span class="hl kwc">rm</span> <span class="hl kwb">$tmpfile</span>
<span class="hl kwc">rm</span> <span class="hl kwb">$lockfile</span>
</pre></div>

Don't forget to alter "fromaddr", "dbpath" and "tracurl" variables to your liking and enjoy!