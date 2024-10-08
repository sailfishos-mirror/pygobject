.. currentmodule:: gi.repository

Text View
=========

The :class:`Gtk.TextView` widget can be used to display and edit large amounts
of formatted text. Like the :class:`Gtk.ListView`, it has a model/view
design.
In this case the :class:`Gtk.TextBuffer` is the model which represents the text
being edited. This allows two or more :class:`Gtk.TextView` widgets to share the
same :class:`Gtk.TextBuffer`, and allows those text buffers to be displayed
slightly differently. Or you could maintain several text buffers and choose to
display each one at different times in the same :class:`Gtk.TextView` widget.

.. seealso:: `Text Widget Overview`_ in the GTK documentation.

The View
--------

The :class:`Gtk.TextView` is the frontend with which the user can add, edit and
delete textual data. They are commonly used to edit multiple lines of text.
When creating a :class:`Gtk.TextView` it contains its own default
:class:`Gtk.TextBuffer`, which is kept in the :attr:`Gtk.TextView.props.buffer`
property.

By default, text can be added, edited and removed from the :class:`Gtk.TextView`.
You can disable this by changing :attr:`Gtk.TextView.props.editable`.
If the text is not editable, you usually want to hide the text cursor with
:attr:`Gtk.TextView.props.cursor_visible` as well.
In some cases it may be useful to set the justification of the text with
:attr:`Gtk.TextView.props.justification`.
The text can be displayed at the left edge, (:attr:`Gtk.Justification.LEFT`),
at the right edge (:attr:`Gtk.Justification.RIGHT`), centered
(:attr:`Gtk.Justification.CENTER`), or distributed across the complete
width (:attr:`Gtk.Justification.FILL`).

Another default setting of the :class:`Gtk.TextView` widget is long lines of
text will continue horizontally until a break is entered. To wrap the text and
prevent it going off the edges of the screen set
:attr:`Gtk.TextView.props.wrap_mode` similar to :doc:`/tutorials/gtk4/display-widgets/label`.

The Model
---------

The :class:`Gtk.TextBuffer` is the core of the :class:`Gtk.TextView` widget, and
is used to hold whatever text is being displayed in the :class:`Gtk.TextView`.
Setting and retrieving the contents is possible with
:attr:`Gtk.TextBuffer.props.text`.
However, most text manipulation is accomplished with *iterators*, represented by
a :class:`Gtk.TextIter`. An iterator represents a position between two
characters in the text buffer.
Iterators are not valid indefinitely; whenever the buffer is modified in a way
that affects the contents of the buffer, all outstanding iterators become
invalid.

Because of this, iterators can't be used to preserve positions across buffer
modifications. To preserve a position, use :class:`Gtk.TextMark`.
A text buffer contains two built-in marks; an "insert" mark (which is the
position of the cursor) and the "selection_bound" mark. Both of them can be
retrieved using :meth:`Gtk.TextBuffer.get_insert` and
:meth:`Gtk.TextBuffer.get_selection_bound`, respectively.
By default, the location of a :class:`Gtk.TextMark` is not shown.
This can be changed by calling :meth:`Gtk.TextMark.set_visible`.

Many methods exist to retrieve a :class:`Gtk.TextIter`. For instance,
:meth:`Gtk.TextBuffer.get_start_iter` returns an iterator pointing to the first
position in the text buffer, whereas :meth:`Gtk.TextBuffer.get_end_iter` returns
an iterator pointing past the last valid character. Retrieving the bounds of
the selected text can be achieved by calling
:meth:`Gtk.TextBuffer.get_selection_bounds`.

To insert text at a specific position use :meth:`Gtk.TextBuffer.insert`.
Another useful method is :meth:`Gtk.TextBuffer.insert_at_cursor` which inserts
text wherever the cursor may be currently positioned. To remove portions of
the text buffer use :meth:`Gtk.TextBuffer.delete`.

In addition, :class:`Gtk.TextIter` can be used to locate textual matches in the
buffer using :meth:`Gtk.TextIter.forward_search` and
:meth:`Gtk.TextIter.backward_search`.
The start and end iters are used as the starting point of the search and move
forwards/backwards depending on requirements.

Tags
----

Text in a buffer can be marked with tags.
A tag is an attribute that can be applied to some range of text. For example, a
tag might be called "bold" and make the text inside the tag bold.
However, the tag concept is more general than that; tags don't have to affect
appearance.
They can instead affect the behavior of mouse and key presses, "lock" a range of
text so the user can't edit it, or countless other things. A tag is represented
by a :class:`Gtk.TextTag` object.
One :class:`Gtk.TextTag` can be applied to any number of text ranges in any
number of buffers.

Each tag is stored in a :class:`Gtk.TextTagTable`.
A tag table defines a set of tags that can be used together.
Each buffer has one tag table associated with it; only tags from that tag table
can be used with the buffer.
A single tag table can be shared between multiple buffers, however.

To specify that some text in the buffer should have specific formatting, you
must define a tag to hold that formatting information, and then apply that tag
to the region of text using :meth:`Gtk.TextBuffer.create_tag` and
:meth:`Gtk.TextBuffer.apply_tag`:

.. code:: python

    tag = textbuffer.create_tag('orange_bg', background='orange')
    textbuffer.apply_tag(tag, start_iter, end_iter)

The following are some of the common styles applied to text:

* Background colour ("background" property)
* Foreground colour ("foreground" property)
* Underline ("underline" property)
* Bold ("weight" property)
* Italics ("style" property)
* Strikethrough ("strikethrough" property)
* Justification ("justification" property)
* Size ("size" and "size-points" properties)
* Text wrapping ("wrap-mode" property)

You can also delete particular tags later using :meth:`Gtk.TextBuffer.remove_tag`
or delete all tags in a given region by calling :meth:`Gtk.TextBuffer.remove_all_tags`.

Example
-------

.. image:: images/textview.png

.. literalinclude:: examples/textview.py
    :linenos:


.. _Text Widget Overview: https://docs.gtk.org/gtk4/section-text-widget.html
