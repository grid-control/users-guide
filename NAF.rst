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


Backend
-------
On the NAF, the HTCondor batch system is used. This is detected automatically, if you specify that a local backend should be used:

.. code-block:: ini

   [global]
   backend = local


Submitting to CentOS7 nodes
---------------------------

At the moment (end of 2019), the batch system default is to submit to EL6 (= Scientific Linux 6, SL6) nodes only
if you submit from a EL6 workgroup server. This behaviour can be altered in grid-control using ``user requirements``.

To submit to EL7 (= CentOS7) worker nodes only, use

.. code-block:: ini

   [condor]
   user requirements = ( OpSysAndVer == "CentOS7" )

To submit to EL6 worker nodes only, use

.. code-block:: ini

   [condor]
   user requirements = ( OpSysAndVer == "SL6" )


To allow the jobs to run on EL6 *and* EL7 nodes, use

.. code-block:: ini

   [condor]
   user requirements = ( OpSysAndVer == "CentOS7" || OpSysAndVer == "SL6" )


Custom entries in condor submit file
------------------------------------

You can add custom entries to the submission file used for condor:

.. code-block:: ini

   [condor]
   jdldata =
         +MyProject="xyz"
         +MyCustomAttribute=12

Make sure that you don't use whitespaces in the attributes, because they are interpreted as list separators!


Singularity (Virtualization)
----------------------------

To run EL6 jobs on EL7 worker nodes in a
`container environment <https://confluence.desy.de/display/IS/Containers>`_,
`singularity <https://confluence.desy.de/display/IS/Singularity>`_
can be used
`within the batch system <https://confluence.desy.de/display/IS/Singularity+support+in+BIRD>`_.

If you already have a working image, you can use that.
Otherwise, store an image to DUST (not afs!), e.g. with

.. code-block:: shell-session

   $ mkdir /nfs/dust/cms/user/${USER}/singularity
   $ SINGULARITY_CACHEDIR="/nfs/dust/cms/user/${USER}/singularity" singularity pull /nfs/dust/cms/user/${USER}/singularity/slc6_latest.sif docker://cmssw/slc6:latest
   INFO:    Converting OCI blobs to SIF format
   INFO:    Starting build...
   Getting image source signatures
   [... working ...]

This will take a few seconds to minutes, but you'll only have to do it once.

In the grid-control config, set the corresponding attributes:

.. code-block:: ini

   [condor]
   user requirements = ( OpSysAndVer == "CentOS7" )
   jdl data = +MySingularityImage="/nfs/dust/cms/user/<user>/singularity/slc6_latest.sif"


Use a VOMS proxy
----------------

If your jobs need a VOMS proxy token, the easiest way is to place it on a shared file system, i.e. AFS.
For this, set the environment variable ``X509_USER_PROXY``, e.g. in your :file:`.[bash|zsh|*]rc` file:

.. code-block:: shell-session

   export X509_USER_PROXY=/afs/desy.de/user/<U>/<USER>/private/x509_voms

After this is set, create your new proxy (``voms-proxy-init``).

In the grid-control config, add

.. code-block:: ini

   [backend]
   proxy = VomsProxy
