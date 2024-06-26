.. image:: https://gitlab.gnome.org/GNOME/pygobject/-/raw/master/docs/images/pygobject.svg?ref_type=heads
   :align: center
   :width: 400px
   :height: 98px

|

**PyGObject** is a Python package which provides bindings for `GObject
<https://docs.gtk.org/gobject/>`__ based libraries such as `GTK
<https://www.gtk.org/>`__, `GStreamer <https://gstreamer.freedesktop.org/>`__,
`WebKitGTK <https://webkitgtk.org/>`__, `GLib
<https://docs.gtk.org/glib/>`__, `GIO
<https://docs.gtk.org/gio/>`__ and many more.

It supports Linux, Windows, and macOS and works with **Python 3.9+** and
**PyPy3**. PyGObject, including this documentation, is licensed under the
**LGPLv2.1+**.

Homepage
--------

https://pygobject.gnome.org

Default branch renamed to ``main``
----------------------------------

The default development branch of PyGObject has been renamed
to ``main``. To update your local checkout, use::

    git checkout master
    git branch -m master main
    git fetch
    git branch --unset-upstream
    git branch -u origin/main
    git symbolic-ref refs/remotes/origin/HEAD refs/remotes/origin/main
