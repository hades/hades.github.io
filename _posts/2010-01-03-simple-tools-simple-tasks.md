---
layout: post
---
I'd like to tell you today what simple tools there are to make your life easier. Of course, a hammered screw holds better than a screwed nail, but it is much more fun to use appropriate tools for their jobs. In the long run at least.

Imagine you want to create a simple photogallery for your site to tell your readers where have you been this summer, what have you seen and what have you done. Ideally it would be a simple page with a title (the place you visited) and large photos with your short comments. You start with a simple html, like this:

<div class="code"><span class="hl kwa">&lt;html&gt;</span><br />
&nbsp;<span class="hl kwa">&lt;head&gt;&lt;title&gt;</span>Saint-Petersburg<span class="hl kwa">&lt;/title&gt;&lt;/head&gt;</span><br />
&nbsp;<span class="hl kwa">&lt;body&gt;</span><br />
&nbsp;&nbsp;<span class="hl kwa">&lt;h1&gt;</span>Saint-Petersburg<span class="hl kwa">&lt;/h1&gt;</span><br />
&nbsp;&nbsp;<span class="hl kwa">&lt;p&gt;</span>Wow, there's an awful lot of Zenith shops there.<span class="hl kwa">&lt;/p&gt;</span><br />
&nbsp;&nbsp;<span class="hl kwa">&lt;img</span> <span class="hl kwb">src</span>=<span class="hl str">&quot;zenith01.jpg&quot;</span> <span class="hl kwa">/&gt;</span><br />
&nbsp;&nbsp;<span class="hl kwa">&lt;p&gt;</span>The saleswoman seems like a gifted businessman.<span class="hl kwa">&lt;/p&gt;</span><br />
&nbsp;&nbsp;<span class="hl kwa">&lt;img</span> <span class="hl kwb">src</span>=<span class="hl str">&quot;businessman01.jpg&quot;</span> <span class="hl kwa">/&gt;</span><br />
&nbsp;<span class="hl kwa">&lt;/body&gt;</span><br />
<span class="hl kwa">&lt;/html&gt;</span><br />
</div>

But you are not a mere mortal, but a fine software developer! Or another kind of brilliant person. So you immediately found a major design flaw: wherever you will need to change the design or layout of this page, or add perhaps a GPS link for each photo, you would need to edit the page (or all the individual photo entries) manually. 

You could also use some kind of a dynamic photo gallery. But that is an obvious overkill: web applications are much more CPU and RAM hungry. Java programmers may stop reading at this point: efficiency problems never stop them. Also the web galleries usually have cumbersome point-and-click interfaces which involve too much, well, pointing and clicking for simple tasks. Enterprise programmers may stop reading at this point and join their Java colleagues. 

So you would write a simple, relatively fast web application that would generate something like the above code from textual descriptions of photos and a template page. That's your only option, right? Wrong! That still might not even be an option: some sysadmins still disable CGI and stuff for their web servers (that includes university homepages and some of the cheapest hostings). And even if your web-app is extremely fast, static pages still seem way better.

Here's how you can have your cake and eat it. For nearly ten years now, there exists a technology called XSL Transformations, or XSLT. In short, it is a language that describes how to convert one XML file into another. Imagine an XML file, say <code>peter.xml</code>:

<div class="code"><span class="hl kwa">&lt;?xml</span> <span class="hl kwb">version</span>=<span class="hl str">&quot;1.0&quot;</span> <span class="hl kwb">encoding</span>=<span class="hl str">&quot;UTF-8&quot;</span><span class="hl kwa">?&gt;</span><br />
<span class="hl kwa">&lt;?xml-stylesheet</span> <span class="hl kwb">type</span>=<span class="hl str">&quot;text/xsl&quot;</span> <span class="hl kwb">href</span>=<span class="hl str">&quot;album.xslt&quot;</span><span class="hl kwa">?&gt;</span><br />
<span class="hl kwa">&lt;album&gt;</span><br />
&nbsp;<span class="hl kwa">&lt;title&gt;</span>Saint-Petersburg<span class="hl kwa">&lt;/title&gt;</span><br />
&nbsp;<span class="hl kwa">&lt;photo&gt;</span><br />
&nbsp;&nbsp;<span class="hl kwa">&lt;image&gt;</span>zenith01.jpg<span class="hl kwa">&lt;/image&gt;</span><br />
&nbsp;&nbsp;<span class="hl kwa">&lt;comment&gt;</span>Wow, there's an awful lot of Zenith shops there.<span class="hl kwa">&lt;/comment&gt;</span><br />
&nbsp;<span class="hl kwa">&lt;/photo&gt;</span><br />
&nbsp;<span class="hl kwa">&lt;photo&gt;</span><br />
&nbsp;&nbsp;<span class="hl kwa">&lt;image&gt;</span>businessman01.jpg<span class="hl kwa">&lt;/image&gt;</span><br />
&nbsp;&nbsp;<span class="hl kwa">&lt;comment&gt;</span>The saleswoman seems like a gifted businessman.<span class="hl kwa">&lt;/comment&gt;</span><br />
&nbsp;<span class="hl kwa">&lt;/photo&gt;</span><br />
<span class="hl kwa">&lt;/album&gt;</span><br />
</div>

It looks exactly like gazillions of other XML files in the world. Except for the second line, which in English says "Oh, hi, you want to show this file to a user? Just apply a stylesheet at <code>album.xslt</code> and he won't go mad. Thanks!" Here's the <code>album.xslt</code>:

<div class="code"><span class="hl kwa">&lt;?xml</span> <span class="hl kwb">version</span>=<span class="hl str">&quot;1.0&quot;</span> <span class="hl kwb">encoding</span>=<span class="hl str">&quot;UTF-8&quot;</span><span class="hl kwa">?&gt;</span><br />
<span class="hl kwa">&lt;xsl:stylesheet</span> <span class="hl kwb">version</span>=<span class="hl str">&quot;1.0&quot;</span> xmlns:<span class="hl kwb">xsl</span>=<span class="hl str">&quot;http://www.w3.org/1999/XSL/Transform&quot;</span><span class="hl kwa">&gt;</span><br />
&nbsp;<span class="hl kwa">&lt;xsl:template</span> <span class="hl kwb">match</span>=<span class="hl str">&quot;/&quot;</span><span class="hl kwa">&gt;</span><br />
&nbsp;&nbsp;<span class="hl kwa">&lt;html&gt;</span><br />
&nbsp;&nbsp;&nbsp;<span class="hl kwa">&lt;head&gt;&lt;title&gt;&lt;xsl:value-of</span> <span class="hl kwb">select</span>=<span class="hl str">&quot;album/title&quot;</span> <span class="hl kwa">/&gt;&lt;/title&gt;&lt;/head&gt;</span><br />
&nbsp;&nbsp;&nbsp;<span class="hl kwa">&lt;body&gt;</span><br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="hl kwa">&lt;h1&gt;&lt;xsl:value-of</span> <span class="hl kwb">select</span>=<span class="hl str">&quot;album/title&quot;</span> <span class="hl kwa">/&gt;&lt;/h1&gt;</span><br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="hl kwa">&lt;xsl:for-each</span> <span class="hl kwb">select</span>=<span class="hl str">&quot;album/photo&quot;</span><span class="hl kwa">&gt;</span><br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hl kwa">&lt;p&gt;&lt;xsl:value-of</span> <span class="hl kwb">select</span>=<span class="hl str">&quot;comment&quot;</span> <span class="hl kwa">/&gt;&lt;/p&gt;</span><br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hl kwa">&lt;img&gt;&lt;xsl:attribute</span> <span class="hl kwb">name</span>=<span class="hl str">&quot;src&quot;</span><span class="hl kwa">&gt;</span><br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hl kwa">&lt;xsl:value-of</span> <span class="hl kwb">select</span>=<span class="hl str">&quot;image&quot;</span> <span class="hl kwa">/&gt;</span><br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hl kwa">&lt;/xsl:attribute&gt;&lt;/img&gt;</span><br />
&nbsp;&nbsp;&nbsp;&nbsp;<span class="hl kwa">&lt;/xsl:for-each&gt;</span><br />
&nbsp;&nbsp;&nbsp;<span class="hl kwa">&lt;/body&gt;</span><br />
&nbsp;&nbsp;<span class="hl kwa">&lt;/html&gt;</span><br />
&nbsp;<span class="hl kwa">&lt;/xsl:template&gt;</span><br />
<span class="hl kwa">&lt;/xsl:stylesheet&gt;</span></div>

If you look closely at it, you will see that it basically tells you (or your browser) how to make <code>peter.html</code> from <code>peter.xml</code>. And that's it. So here are the bonuses you get:

<ul><li>both files are static, you don't need to have a muscular web server or a friendly (or non-muscular) sysadmin;
</li><li>if you have hundreds of photos on a single page and at some moment decide that comments should go below the photo, you'd need to swap two lines, not hundreds of lines;
</li><li>if you visited thousands of places and at some moment decide that every page should have a copyright footer or be redesigned completely, you'd need to change one single file, not thousands of them;
</li><li>you can even have different styles for the same set of photos. For example, you may want to show smaller images when viewed from a handheld device. Or bigger images when viewed on your 60" plasma;
</li><li>you don't increase the butthurterol level in your blood.</li></ul>

Next time I shall probably show you some of my deployment tricks. See you later!