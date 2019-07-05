Installation
============

.. contents::
   :local:
   :backlinks: none


Requirements
------------

Requirements on the submission host:

* CPython 2.3 – 3.5 / pypy 2.4 – 5.3 with python standard libraries
* (optional) requests – for faster interactions with webservies
* (optional) cherrypy – for web based user interface
* (highly recommended) a supported submission backend (ARC, Condor, CreamCE, glite/-wms, PBS, LSF, SLURM, JMS, GridEngine)

Requirements on the worker nodes:

* bash, GNU coreutils, awk, sed
* (optional) gfal / other supported transfer tools – for storage access
* (optional) cvmfs – to allow running the LHC experiment software


Installation from PyPI (``pip``)
--------------------------------

**Note**: *Since the PyPI release is rather outdated at the moment, we recommend* :ref:`cloning from git <clone_from_git>`.

.. code-block:: shell-session

    $ pip install --user grid-control

This installs the default ``go.py`` executable. In addition the executalbe ``gridcontrol`` is provided, because of the more descriptive name.

.. _clone_from_git:

Clone from git
--------------

.. code-block:: shell-session

    # whereever you check out your projects
    $ cd <your project path>
    $ git clone https://github.com/grid-control/grid-control.git
    Cloning into 'grid-control'...
    remote: Enumerating objects: 43, done.
    remote: Counting objects: 100% (43/43), done.
    remote: Compressing objects: 100% (27/27), done.
    remote: Total 18566 (delta 26), reused 31 (delta 16), pack-reused 18523
    Receiving objects: 100% (18566/18566), 6.39 MiB | 4.70 MiB/s, done.
    Resolving deltas: 100% (14715/14715), done.


Now one option is to place a link to the executable in a location contained in your ``PATH`` environment variable.
Assuming ``~/bin`` is such a location (check with ``echo $PATH``):

.. code-block:: shell-session

    $ cd ~/bin
    $ ln -s <your project path>/grid-control/go.py grid-control

Now you can use the ``grid-control`` executable, instead of the standard ``go.py``, which is not very descriptive.

Switch to a different branch
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Different branches can be used. E.g. ``testing`` for more up-to-date changes:

.. code-block:: shell-session

    $ cd <your project path>/grid-control
    $ git checkout testing 
    Branch 'testing' set up to track remote branch 'testing' from 'origin'.
    Switched to a new branch 'testing'

Pull remote changes
^^^^^^^^^^^^^^^^^^^

Update your version with upstream changes from the current branch (make sure you are on the local branch that you want to update):

.. code-block:: shell-session

    $ cd <your project path>/grid-control
    $ git pull
    Updating 14725499..bb9b2a93
    Fast-forward
    packages/grid_control/backends/condor_wms/condor_wms.py |  3 ++-
    packages/grid_control/backends/wms_condor.py            | 13 +++++++++----
    packages/grid_control/share/help.txt                    |  4 ++--
    packages/grid_control/utils/webservice_urllib2.py       |  4 +---
    packages/grid_control_api.py                            |  4 ++--
    5 files changed, 16 insertions(+), 12 deletions(-)
