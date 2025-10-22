=====================================================================
Partialator on HTCondor
=====================================================================

.. contents::
   :local:
   :depth: 2

Overview
=====================================================================

The `partialator_htcondor <https://github.com/philiosi/partialator_htcondor>`_ repository
packages PAL-specific helper scripts for scaling and merging CrystFEL streams with
``partialator`` on the HTCondor pool.  It complements the :doc:`crystfel_htcondor`
material by focusing on the post-indexing steps:

* collecting stream files produced by :program:`indexamajig`
* staging configuration files and reference data
* scheduling many ``partialator`` jobs with HTCondor
* harvesting merged ``.hkl``/``.mtz`` outputs for downstream refinement

The following sections summarise the workflow and highlight PAL-specific
considerations so that the scripts can be adapted for new experiments.

Repository layout
=====================================================================

After cloning the repository into your PAL workspace (``/pal/home`` or
``/pal/data``), inspect the top-level directories:

``condor/``
    Template ``.sub`` files and helper snippets that describe how each
    partialator task is dispatched to HTCondor worker nodes.

``scripts/``
    Shell wrappers that prepare arguments for :program:`partialator`, create
    job lists, and post-process merged results.

``templates/``
    Example configuration files (geometry, reference structure, partialator
    parameter blocks) that serve as starting points for your own datasets.

``README.md``
    Step-by-step notes from the original project.  Always review the README for
    the latest options or PAL-specific updates before running production jobs.

Required software environment
=====================================================================

#. Load the CrystFEL module that matches the version used to produce the
   stream files.  The helper scripts expect the ``partialator`` CLI to be on
   ``$PATH``.

   .. code-block:: bash

      module avail crystfel
      module load crystfel/0.10.2

#. If your workflow relies on Python utilities from the repository (for
   example, to rewrite geometry files or curate stream lists), activate the PAL
   Anaconda environment suggested in :doc:`anaconda`.

#. Ensure that HTCondor credentials (OTP and Kerberos) are valid before
   attempting any submission.  Reuse the procedures documented in
   :doc:`htcondor_job_processing`.

Preparing input stream lists
=====================================================================

``partialator`` consumes one or more CrystFEL stream files.  The helper
``scripts/build_stream_list.sh`` script scans a target directory (defaults to
``./streams``) and creates a text file with one stream per line.  Adjust the
search pattern when your experiment stores streams in nested folders.

.. code-block:: bash

   cd /pal/data/<group>/<experiment>/partialator_htcondor
   ./scripts/build_stream_list.sh \
       --streams ../indexamajig_htcondor/streams \
       --output inputs/stream_files.txt

The generated list becomes the input queue for HTCondor.  Inspect it carefully
and remove failed or incomplete streams to avoid wasting worker time.

Customising partialator options
=====================================================================

Edit ``templates/partialator.cfg`` (or duplicate it for each resolution shell)
with settings appropriate for your experiment:

* ``geometry =`` absolute path to the detector geometry used during indexing
* ``symmetry =`` crystal point group
* ``target-cell =`` unit cell parameters (``a b c alpha beta gamma``)
* ``partialator-mode =`` e.g. ``scales`` or ``partialator``
* ``push-res =`` high-resolution cutoff for scaling
* ``iterations =`` number of macro cycles

Whenever the input streams are generated with per-run wavelength
refinements, also enable ``fix-wavelength = false`` to allow the scaler to
reuse those values.

HTCondor submission workflow
=====================================================================

The repository’s ``condor/partialator.sub`` submit description and
``scripts/submit_partialator.sh`` orchestrate the farm execution.  The
submission script accepts a stream list and optional overrides for the
partialator configuration and temporary directories.

.. code-block:: bash

   ./scripts/submit_partialator.sh \
       --streams inputs/stream_files.txt \
       --config configs/xfel_partialator.cfg \
       --tag 2024q2_test

Key directives inside ``condor/partialator.sub``:

``executable``
    Points to ``scripts/run_partialator_worker.sh`` which loads modules and
    launches :program:`partialator` with the requested arguments.

``transfer_input_files``
    Ships the selected stream file, geometry, and configuration snippets to the
    worker scratch space.  Verify that referenced files are smaller than
    HTCondor’s transfer limits or stage them to a shared filesystem instead.

``request_cpus`` and ``request_memory``
    Default to 16 cores and 8 GB respectively, matching PAL’s standard HTCondor
    slots.  Increase these requests only when your ``partialator`` options need
    more resources.

Monitoring and collecting results
=====================================================================

* Use ``condor_q`` to check the job queue and ``condor_status`` to monitor
  available worker nodes.  Refer to :doc:`htcondor_reference` for additional
  monitoring tips.
* Logs under ``logs/`` capture HTCondor submission events, while the worker log
  shows the exact ``partialator`` command line executed on each node.
* Resulting ``*.hkl`` or ``*.mtz`` files are written to ``output/<tag>/``.
  Merge the per-stream results with ``scripts/collect_results.sh`` which also
  collates figures of merit into ``summary.csv`` for downstream plotting.

Troubleshooting checklist
=====================================================================

* ``partialator`` exits immediately
    Ensure the geometry file path embedded in the configuration is accessible
    from worker nodes.  If necessary, stage geometries via ``transfer_input_files``.

* Jobs remain idle
    Confirm that the stream list points to files on a shared filesystem or that
    transfer directives copy the data into the job’s sandbox.  Idle jobs also
    appear when resource requests exceed available slot sizes.

* Output ``*.hkl`` files show zero reflections
    Revisit the ``push-res`` limit and ``min-measurements`` thresholds in the
    configuration.  Excessively aggressive cutoffs remove all data before
    scaling begins.

For deeper debugging, re-run ``run_partialator_worker.sh`` manually on the UI
node with the same arguments used by HTCondor.  This allows rapid iteration on
configuration changes before resubmitting to the farm.
