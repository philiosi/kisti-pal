Section 1 Introduction
=====================================================================

Section 1.1 CrystFEL
----------------------------------------------------------------------------
.. epigraph::
    CrystFEL is a suite of programs for processing diffraction data acquired "serially" in a "snapshot" manner, such as when using the technique of Serial Femtosecond Crystallography (SFX) with a free-electron laser source. 
    
    CrystFEL comprises programs for indexing and integrating diffraction patterns, scaling and merging intensities, simulating patterns, calculating figures of merit for the data and visualising the results. Supporting scripts are provided to help at all stages, including importing data into CCP4 for further processing.
 
    -- CrystFEL overview - https://www.desy.de/~twhite/crystfel/

Section 1.2 HTcondor
--------------------------------------------------

.. epigraph::
    HTCondor is a software system that creates a High-Throughput Computing (HTC) environment. It effectively uses the computing power of machines connected over a network, be they a single cluster, a set of clusters on a campus, cloud resources either standalone or temporarily joined to a local cluster, or international grids. Power comes from the ability to effectively harness shared resources with distributed ownership.

    A user submits jobs to HTCondor. HTCondor finds available machines and begins running the jobs there. HTCondor has the capability to detect that a machine running a job is no longer available (perhaps the machine crashed, or maybe it prefers to run another job). HTCondor will automatically restart the job on another machine without intervention from the user.

    HTCondor is useful when a job must be run many (thousands of) times, perhaps with hundreds of different data sets. With one command, all of the jobs are submitted to HTCondor. Depending upon the number of machines in the HTCondor pool, hundreds of otherwise idle machines can be running the jobs at any given moment.

    -- HTCondor overview - https://htcondor.org/

Section 1.3 Indexamajig Execution based on the HTCondor 
------------------------------------------------------------------------

You can perform 10 indexamajig job with 72cores on the each worker nodes of HTCondor like below:

.. image:: ../images/usingHtcondor.png
    :scale: 70 %
    :align: center

* KISTI-PAL computing farm have two submit nodes for condor job.
    * pal-ui-el7.sdfarm.kr
    * pal-ui02-el7.sdfarm.kr

* You can refer the section 2 CrystFEL & HTCondor to create file list (lsf files) and submit bulk indexamajig jobs.
