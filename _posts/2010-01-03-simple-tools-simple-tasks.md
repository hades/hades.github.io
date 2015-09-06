---
layout: post
title: Simple Tools for Simple Tasks
---
I'd like to tell you today what simple tools there are to make your life
easier. Of course, a hammered screw holds better than a screwed nail, but it is
much more fun to use appropriate tools for their jobs. In the long run at
least.

Imagine you want to create a simple photogallery for your site to tell your
readers where have you been this summer, what have you seen and what have you
done. Ideally it would be a simple page with a title (the place you visited)
and large photos with your short comments. You start with a simple html, like
this:

    <html>
     <head><title>Saint-Petersburg</title></head>
     <body>
      <h1>Saint-Petersburg</h1>
      <p>Wow, there's an awful lot of Zenith shops there.</p>
      <img src="zenith01.jpg" />
      <p>The saleswoman seems like a gifted businessman.</p>
      <img src="businessman01.jpg" />
     </body>
    </html>

But you are not a mere mortal, but a fine software developer! Or another kind
of brilliant person. So you immediately found a major design flaw: wherever you
will need to change the design or layout of this page, or add perhaps a GPS
link for each photo, you would need to edit the page (or all the individual
photo entries) manually. 

You could also use some kind of a dynamic photo gallery. But that is an obvious
overkill: web applications are much more CPU and RAM hungry. Java programmers
may stop reading at this point: efficiency problems never stop them. Also the
web galleries usually have cumbersome point-and-click interfaces which involve
too much, well, pointing and clicking for simple tasks. Enterprise programmers
may stop reading at this point and join their Java colleagues. 

So you would write a simple, relatively fast web application that would
generate something like the above code from textual descriptions of photos and
a template page. That's your only option, right? Wrong! That still might not
even be an option: some sysadmins still disable CGI and stuff for their web
servers (that includes university homepages and some of the cheapest hostings).
And even if your web-app is extremely fast, static pages still seem way better.

Here's how you can have your cake and eat it. For nearly ten years now, there
exists a technology called XSL Transformations, or XSLT. In short, it is a
language that describes how to convert one XML file into another. Imagine an
XML file, say ```peter.xml```:

    <?xml version="1.0" encoding="UTF-8"?>
    <?xml-stylesheet type="text/xsl" href="album.xslt"?>
    <album>
     <title>Saint-Petersburg</title>
     <photo>
      <image>zenith01.jpg</image>
      <comment>Wow, there's an awful lot of Zenith shops there.</comment>
     </photo>
     <photo>
      <image>businessman01.jpg</image>
      <comment>The saleswoman seems like a gifted businessman.</comment>
     </photo>
    </album>

It looks exactly like gazillions of other XML files in the world. Except for
the second line, which in English says "Oh, hi, you want to show this file to a
user? Just apply a stylesheet at ```album.xslt``` and he won't go mad.
Thanks!" Here's the ```album.xslt```:

    <?xml version="1.0" encoding="UTF-8"?>
    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
     <xsl:template match="/">
      <html>
       <head><title><xsl:value-of select="album/title" /></title></head>
       <body>
        <h1><xsl:value-of select="album/title" /></h1>
        <xsl:for-each select="album/photo">
         <p><xsl:value-of select="comment" /></p>
         <img><xsl:attribute name="src">
          <xsl:value-of select="image" />
         </xsl:attribute></img>
        </xsl:for-each>
       </body>
      </html>
     </xsl:template>
    </xsl:stylesheet>

If you look closely at it, you will see that it basically tells you (or your
browser) how to make ```peter.html``` from ```peter.xml```. And
that's it. So here are the bonuses you get:

 * both files are static, you don't need to have a muscular web server or a
   friendly (or non-muscular) sysadmin;
 * if you have hundreds of photos on a single page and at some moment decide
   that comments should go below the photo, you'd need to swap two lines, not
   hundreds of lines;
 * if you visited thousands of places and at some moment decide that every page
   should have a copyright footer or be redesigned completely, you'd need to
   change one single file, not thousands of them;
 * you can even have different styles for the same set of photos. For example,
   you may want to show smaller images when viewed from a handheld device. Or
   bigger images when viewed on your 60" plasma;
 * you don't increase the butthurterol level in your blood.

Next time I shall probably show you some of my deployment tricks. See you
later!
