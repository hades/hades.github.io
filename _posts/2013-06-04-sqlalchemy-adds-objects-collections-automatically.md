---
layout: post
title: "SQLAlchemy Adds Objects to Collections Automatically"
---
So I've been using [SQLAlchemy](http://www.sqlalchemy.org/) for a while for
[Chukchi](https://github.com/hades/chukchi). Here's a WTF I've got recently.

Imagine you have a basic foreign key relationship in your SQLAlchemy
declarative models. For example, let's take one from Chukchi's
[models.py](https://github.com/hades/chukchi/blob/85c92aeda0a47af3bd78a01a83565c8bdf1d6ccf/chukchi/db/models.py)
lines 83-121:

    class Entry(Base):
        id = Column(Integer, primary_key=True)
        # <...>

    class Content(Base):
        id = Column(Integer, primary_key=True)
        entry_id = Column(Integer, ForeignKey(Entry.id), nullable=False)
        # <...>
        entry = relationship(Entry, backref='content')

(note that I've skipped irrelevant parts)

Now let's import and play with it for a bit:

    >>> from chukchi.db.models import *
    >>> from chukchi.db import Session
    >>> db = Session()
    >>> entry = db.query(Entry).order_by(Entry.id.desc()).first()
    INFO sqlalchemy.engine.base.Engine SELECT <...>
    FROM entry ORDER BY entry.id DESC 
     LIMIT %(param_1)s
    INFO sqlalchemy.engine.base.Engine {'param_1': 1}

(note that I've got `echo=True` enabled so that we see all the SQL)

Now let's see what child objects are there in the database:

    >>> print ", ".join(str(c.id) for c in entry.content)
    INFO sqlalchemy.engine.base.Engine SELECT <...>
    FROM content 
    WHERE %(param_1)s = content.entry_id
    INFO sqlalchemy.engine.base.Engine {'param_1': 2662}
    25223, 25224

Okay, so we have an Entry object and two Content objects that refer to it.

Now let's create a new Content object:

    >>> c = Content()
    >>>

As you probably know, SQLAlchemy executes write queries whenever the session is
flushed. But if we flush the session now, nothing happens:

    >>> db.flush()
    >>>

That's because the state of our new object is "transient". As long as we don't
add it explicitely to the Session, it won't be inserted into the database.

Now let's create a Content object with some data filled:

    >>> c = Content(entry=entry,
    ...             type='text/plain',
    ...             hash="doesn't matter",
    ...             data='')

Now what would happen if we flushed the session now? Nothing, right? Wrong!

    INFO sqlalchemy.engine.base.Engine INSERT INTO content (<...>) VALUES (<...>) RETURNING content.id
    INFO sqlalchemy.engine.base.Engine {'hash': "doesn't matter", 'entry_id': 2961, 'type': 'text/plain', 'data': '', <...>}

This happens because when you initialize a relationship with a persistent
object (that is, an object within a Session), it gets added to its parent
object's backref collection:
 
    >>> print ", ".join(str(c.id) for c in entry.content)
    25223, 25224, 25225
    c = Content(entry=entry)
    >>> print ", ".join(str(c.id) for c in entry.content)
    25223, 25224, 25225, None

(new Content object having no ID yet)

Interestingly enough, this doesn't happen, if you initialize the table field,
instead of a relationship:

    >>> c = Content(entry_id=2961)
    >>> print ", ".join(str(c.id) for c in entry.content)
    25223, 25224, 25225, None

So watch out if you initialize a relationship attribute: you might get some new
unexpected objects in your database.
