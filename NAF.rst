NAF specifics
=============

If you are a user of the National Analysis Facility (`NAF <https://naf.desy.de>`_) and want to submit to the
`BIRD <http://bird.desy.de>`_ batch farm, this page provides some specific instructions and features for you.

.. contents::
   :local:
   :backlinks: none

.. _NAF_central_inst:

Central installation
--------------------

You don't need to install grid-control on your own, if you're a NAF user. We provide a central installation (actually different versions) using the usual environment module system:

.. code-block:: shell-session

   $ module avail grid-control

   --------------- /afs/desy.de/group/cms/modulefiles/ ---------------
   grid-control/current(default) grid-control/testing
   grid-control/previous

   $ module load grid-control/current 

The default version is ``current``, which is the general recommendation. Whenever ``current`` is updated, the last version will be made available as ``previous`` for the case that you rely on that specific version for some time still (e.g. for existing tasks).
If you need a newer version containing recent changes, you can use the ``testing`` version.
   
After you loaded the module (you might want to do this in your :file:`.[bash|zsh|*]rc` file), you can use the ``grid-control`` executable.


Submitting to CentOS7 nodes
---------------------------

At the moment (end of 2019), the batch system default is to submit to EL6 (= Scientific Linux 6, SL6) nodes only
if you submit from a EL6 workgroup server. This behaviour can be altered in grid-control using ``user requirements``.

To submit to EL7 (= CentOS7) worker nodes only, use

.. code-block:: ini

   [condor]
   user requirements = ( OpSysAndVer == "CentOS7")


To allow the jobs to run on the EL6 *and* EL7 nodes, use

.. code-block:: ini

   [condor]
   user requirements = ( OpSysAndVer == "CentOS7" || OpSysAndVer == "SL6")

