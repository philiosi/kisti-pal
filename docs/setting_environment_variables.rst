---------------------------------------------------
Setting Environment Variables
---------------------------------------------------

Following command is to setup a default environment using HDF5-1.10.5, CrystFEL 0.9.1, Cheetah_FXL.

.. code-block:: sh

    source /pal/lib/setup.sh

To use CrystFEL 0.13.0 with HDF5 1.12.2 (threadsafe), run

.. code-block:: sh

    source /pal/lib/setup_crystfel-0.13.0_hdf5-1.12.2-threadsafe-v18.sh

To use CrystFEL 0.12.0 with HDF5 1.10.5, run

.. code-block:: sh

    source /pal/lib/setup_crystfel-0.12.1_hdf5-1.10.5.sh

To use CrystFEL 0.11.1 with HDF5 1.10.5, run

.. code-block:: sh

    source /pal/lib/setup_crystfel-0.11.1_hdf5-1.10.5.sh

Note that logout from the current session is required to setup a different environment, e.g. CrystFEL 0.10.1 and/or HDF5 1.10.5.

.. note::

   HTCondor jobs use the environment of the shell you submit from.
   Source the CrystFEL version you want **before** running
   ``2_submit_condor_indexing.sh``. Each job log records the ``indexamajig``
   path and version it actually used.

Using cheetah (cheetah_FXL)

.. code-block:: sh

    source setup_cheetah_FXL.sh

Please contact PAL support team by email for any inquiry. Please refer to the helpdesk service section below.