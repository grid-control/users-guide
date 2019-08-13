Tips & tricks
=============

.. contents::
   :local:
   :backlinks: none


GNU Screen
----------

You might find it useful to run grid-control in a `GNU Screen <https://www.gnu.org/software/screen/>`_ session (see also `Wikipedia <https://en.wikipedia.org/wiki/GNU_Screen>`_) if you use ssh to connect to a submit host.
This allows you to detach the screen session and disconnect your ssh connection, while grid-control keeps running on the host and further updates and (re)submits jobs. Also, you can reconnect to this session when ssh-ing to the host from a different machine (e.g. from your office and from home).


Global configuration
--------------------

~/.grid-control.conf

.. code-block:: ini

   [global]
   workdir create= True ; don't ask if workdir should be created

