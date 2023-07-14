====================
Running on a Cluster
====================

Nornir uses parallel python to delegate work to other computers when possible.  This can dramatically speed up the build process.

Requirements
------------

1. Each machine should be running the same version of Nornir and external tools. In particular match versions of Image Magick since bugs/changes have effected color handling in the past.

2. The build should be targeting a network drive accessible to all machines on the cluster

3. The firewall must be configured to allow Parallel Python traffic.  

Joining the cluster
-------------------

To make a machine eligible to receive nornir cluster job one simply runs the `startserver.cmd` file located in the python scripts directory after install.

Troubleshooting
---------------

At this time error handling is not robust for jobs delegated to the cluster.  If a cluster job fails an exception is sent back to the caller which is visible to the user.  The best approach is to remove machines from the cluster by closing the console running the startserver command.  Run the job without the cluster.  If it works then add one machine at a time until the faulting machine is isolated.

A common cause of errors is for a cluster machine to drop a drive mapping.  All jobs sent to that machine complete instantly.  So the majority of jobs complete on the machine but are not actually completed. 

Another issue we've seen is for a machine to use a different version of Image Magick which handle colors differently.  This results in different shading across tiles in a mosaic depending on which machine in the cluster took the job. 