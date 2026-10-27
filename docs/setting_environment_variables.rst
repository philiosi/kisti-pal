---------------------------------------------------
Setting Environment Variables
---------------------------------------------------

Following command is to setup a default environment using HDF5 1.8.10-patch1, CrystFEL 0.9.1, Cheetah_FXL.

.. code-block:: sh

    source /pal/lib/setup.sh

To use CrystFEL 0.13.0 with HDF5 1.12.2 (threadsafe), run

.. code-block:: sh

    source /pal/lib/setup_crystfel-0.13.0_hdf5-1.12.2-threadsafe-v18.sh

To use CrystFEL 0.12.0 with HDF5 1.10.5, run

.. code-block:: sh

    source /pal/lib/setup_crystfel-0.12.0_hdf5-1.10.5.sh

To use CrystFEL 0.11.1 with HDF5 1.10.5, run

.. code-block:: sh

    source /pal/lib/setup_crystfel-0.11.1_hdf5-1.10.5.sh

Note that logout from the current session is required to setup a different environment, e.g. CrystFEL 0.10.1 and/or HDF5 1.10.5.

These scripts prepend to ``PATH`` and ``LD_LIBRARY_PATH`` and do not undo each other,
so source only one of them per session. After sourcing, check what you actually got
before starting a long run:

.. code-block:: sh

    which indexamajig
    indexamajig --version
    echo $HDF5_ROOT

.. note::

   ``$HDF5_ROOT`` is the HDF5 that CrystFEL is linked against, but the
   command-line HDF5 tools picked up from ``PATH`` come from the Anaconda
   installation and are a different version (``h5dump`` reports 1.10.1). Use
   ``$HDF5_ROOT/bin/h5dump`` when you need the tools that match CrystFEL.

.. note::

   HTCondor jobs use the environment of the shell you submit from.
   Source the CrystFEL version you want **before** running
   ``2_submit_condor_indexing.sh``. Each job log records the ``indexamajig``
   path and version it actually used.

Using cheetah (cheetah_FXL)

.. code-block:: sh

    source setup_cheetah_FXL.sh

.. note::

   ``setup_cheetah_FXL.sh`` sets ``CRYSTFEL`` to ``/pal/lib/pal-soft/crystfel``,
   which is CrystFEL 0.8.0. Sourcing a CrystFEL setup script afterwards does give
   you the newer CrystFEL, but it also changes ``CDIR`` from ``cheetah_FXL`` to
   ``cheetah_pal`` and ``HDF5_ROOT`` to the one that CrystFEL build needs. If you
   need Cheetah FXL and a recent CrystFEL, use separate sessions.

   Each CrystFEL build is linked against one specific HDF5 (0.8.0 against
   1.8.10-patch1, 0.12.0 against 1.10.5, 0.13.0 against 1.12.2), so the setup
   script and the CrystFEL version have to stay paired.

Please contact PAL support team by email for any inquiry. Please refer to the helpdesk service section below.