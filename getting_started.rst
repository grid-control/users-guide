Getting started
===============

grid-control needs a configuration file written in a simple ini-style language. It can also be used with python configs or by directly calling using the python API, which is not covered here.

In the following, it is assumed that the executable is called ``grid-control``. You might need to replace this with ``go.py`` or ``gridcontrol`` depending on your :doc:`installation <installation>`.

First configuration (run jobs on the local machine)
---------------------------------------------------

This most basic example is not using a batch system, but running jobs on your local machine (``backend = host``). This can be useful for testing new configurations.

.. literalinclude:: code/host.conf
   :caption: host.conf

The configuration is divided into sections specified in brackets. The task specified in the ``[global]`` section is ``UserTask``,
while the configuration of the specific user task is specified in the ``[UserTask]`` section.
Comments start with a ``;``.

Now we create a task by calling ``grid-control`` and passing the configuration file as an argument. We use the ``-s`` command line flag to disable job sumbission for now. This is always a good idea to see how many jobs are generated etc. and check if there are any obvious errors.

.. code-block:: shell-session

   $ grid-control -s host.conf
   
   *****************************************************************
   *               _     _                      _             _    *
   *     __ _ _ __(_) __| |      ___ ___  _ __ | |_ _ __ ___ | |   *
   *    / _` | '__| |/ _` |____ / __/ _ \| '_ \| __| '__/ _ \| |   *
   *   | (_| | |  | | (_| |____| (_| (_) | | | | |_| | | (_) | |   *
   *    \__, |_|  |_|\__,_|     \___\___/|_| |_|\__|_|  \___/|_|   *
   *    |___/                                                      *
   *                 The Swiss Army knife of job submission tools  *
   *                                             arXiv:1707.03198  *
   *                                                               *
   * https://github.com/grid-control/grid-control                  *
   *****************************************************************
   Revision: 1.9.89 (aa054081)
   Starting initialization of /home/jolange/playground/gc/work.host!
   Do you want to create the working directory /home/jolange/playground/gc/work.host? [YES/no]: yes
   Current task ID: GC5aeb5265a4fe
   Current workdir: /home/jolange/playground/gc/work.host
   Task started on: 2019-06-26
   Using batch system: HOST
   
   -----------------------------------------------------------------
   REPORT SUMMARY:                             host / GC5aeb5265a4fe
   ---------------
   Total number of jobs:        2     Successful jobs:       0    0%
   Jobs being processed:        0        Failing jobs:       0    0%
   Detailed Status Information:
   Jobs       INIT:       2  100%     Jobs  SUBMITTED:       0    0%
   Jobs   DISABLED:       0    0%     Jobs      READY:       0    0%
   Jobs    WAITING:       0    0%     Jobs     QUEUED:       0    0%
   Jobs    ABORTED:       0    0%     Jobs    RUNNING:       0    0%
   Jobs     CANCEL:       0    0%     Jobs    UNKNOWN:       0    0%
   Jobs  CANCELLED:       0    0%     Jobs       DONE:       0    0%
   Jobs     FAILED:       0    0%     Jobs    SUCCESS:       0    0%
   -----------------------------------------------------------------
   
   2019-06-26 14:14:46 - Time left for access token "AFSAccessToken": 23h 54min 31sec

The task has been initialzed and a working directory :file:`work.host` has been created to capture the state of the current task.
Two jobs have been created (both in the INIT state), because we explicitly specified this.
Everything looks fine and we can actually submit the jobs: We invoke ``grid-control`` again, but without the ``-s`` flag to enable job submission. The current task state will be picked up from the working directory corresponding to the configuration file.

.. code-block:: shell-session

   $ grid-control -c host.conf
   
   *****************************************************************
   *               _     _                      _             _    *
   *     __ _ _ __(_) __| |      ___ ___  _ __ | |_ _ __ ___ | |   *
   *    / _` | '__| |/ _` |____ / __/ _ \| '_ \| __| '__/ _ \| |   *
   *   | (_| | |  | | (_| |____| (_| (_) | | | | |_| | | (_) | |   *
   *    \__, |_|  |_|\__,_|     \___\___/|_| |_|\__|_|  \___/|_|   *
   *    |___/                                                      *
   *                 The Swiss Army knife of job submission tools  *
   *                                             arXiv:1707.03198  *
   *                                                               *
   * https://github.com/grid-control/grid-control                  *
   *****************************************************************
   Revision: 1.9.89 (aa054081)
   Current task ID: GC5aeb5265a4fe
   Current workdir: /home/jolange/playground/gc/work.host
   Task started on: 2019-06-26
   Using batch system: HOST
   
   -----------------------------------------------------------------
   REPORT SUMMARY:                             host / GC5aeb5265a4fe
   ---------------
   Total number of jobs:        2     Successful jobs:       0    0%
   Jobs being processed:        0        Failing jobs:       0    0%
   Detailed Status Information:
   Jobs       INIT:       2  100%     Jobs  SUBMITTED:       0    0%
   Jobs   DISABLED:       0    0%     Jobs      READY:       0    0%
   Jobs    WAITING:       0    0%     Jobs     QUEUED:       0    0%
   Jobs    ABORTED:       0    0%     Jobs    RUNNING:       0    0%
   Jobs     CANCEL:       0    0%     Jobs    UNKNOWN:       0    0%
   Jobs  CANCELLED:       0    0%     Jobs       DONE:       0    0%
   Jobs     FAILED:       0    0%     Jobs    SUCCESS:       0    0%
   -----------------------------------------------------------------
   
   Running in continuous mode. Press ^C to exit.
   2019-06-26 14:15:14 - Time left for access token "AFSAccessToken": 23h 54min 03sec
   2019-06-26 14:15:14 - Job 0 state changed from INIT to SUBMITTED
   2019-06-26 14:15:14 - Job 1 state changed from INIT to SUBMITTED
   2019-06-26 14:15:20 - Job 0 state changed from SUBMITTED to DONE
   2019-06-26 14:15:20 - Job 1 state changed from SUBMITTED to DONE
   2019-06-26 14:15:25 - Job 0 state changed from DONE to SUCCESS (runtime 0h 00min 00sec)
   2019-06-26 14:15:25 - Job 1 state changed from DONE to SUCCESS (runtime 0h 00min 00sec)
   2019-06-26 14:15:30 - Task successfully completed. Quitting grid-control!
   2019-06-26 14:15:30 - Workdir was /home/jolange/playground/gc/work.host

While grid-control is running, it checks the status of running jobs, retrieves and analyzes finished jobs and then submits jobs that are ready. We used the continuous mode (``-c``) to let grid-control repeat this until the task is finished. We can interrupt this by pressing :kbd:`Ctrl-C` and restart it by just calling ``grid-control [-s] <conf>`` again.


Further examples
----------------

Further example configurations can be found here:
https://github.com/grid-control/grid-control/tree/master/docs/examples
