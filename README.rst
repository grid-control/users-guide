grid-control user's guide
=========================

grid-control user's guide written in reStructuredText to be compiled using `sphinx <http://www.sphinx-doc.org/en/master/>`_

Install sphinx and theme:

.. code-block:: shell-session

   $ pip install --user sphinx
   $ pip install --user sphinx_rtd_theme

Build the documentation:

.. code-block:: shell-session

   # build html documentation
   $ make html
   # (optional) build pdf documentation
   $ make latexpdf

Deploy (maybe clear the directory before):

.. code-block:: shell-session

   $ sphinx-build -b html . ../grid-control.github.io
