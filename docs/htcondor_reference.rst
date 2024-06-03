=====================================================================
HTCondor References
=====================================================================

Condor_manual: HTCondor Version 9.8.1 Manual — HTCondor Manual 9.8.1 documentation

- Submitting a Job — HTCondor Manual 9.8.1 documentation
- Managing a Job — HTCondor Manual 9.8.1 documentation

Condor Commands
----------------------------------------------------------------------------

You can check the commands by typing `condor_` and pressing the tab key.

.. code-block:: console

    [USERID@pal-ui02-el7 ~]$ condor_
    condor_advertise             condor_had                   condor_release               condor_submit_dag
    condor_aklog                 condor_history               condor_replication           condor_suspend
    condor_annex                 condor_hold                  condor_reschedule            condor_tail
    condor_c-gahp                condor_job_router_info       condor_restart               condor_test_match
    condor_c-gahp_worker_thread  condor_master                condor_rm                    condor_testwritelog
    condor_check_userlogs        condor_negotiator            condor_router_history        condor_transferd
    condor_cod                   condor_now                   condor_router_q              condor_transfer_data
    condor_collector             condor_nsenter               condor_router_rm             condor_transform_ads
    condor_config_val            condor_off                   condor_run                   condor_update_machine_ad
    condor_continue              condor_on                    condor_schedd                condor_updates_stats
    condor_convert_history       condor_ping                  condor_set_shutdown          condor_userlog
    condor_credd                 condor_pool_job_report       condor_shadow                condor_userlog_job_counter
    condor_dagman                condor_power                 condor_sos                   condor_userprio
    condor_docker_enter          condor_preen                 condor_ssh_to_job            condor_vacate
    condor_drain                 condor_prio                  condor_startd                condor_vacate_job
    condor_fetchlog              condor_procd                 condor_starter               condor_version
    condor_findhost              condor_q                     condor_stats                 condor_vm-gahp-vmware
    condor_gather_info           condor_qedit                 condor_status                condor_vm_vmware
    condor_gridmanager           condor_qsub                  condor_store_cred            condor_wait
    condor_gridshell             condor_reconfig              condor_submit                condor_who


Checking Condor Job Queue
----------------------------------------------------------------------------

- `condor_q`: Shows jobs submitted by the user (yourself).
  - Refer to `condor_q -help` or the official documentation for options.
- `condor_q -alluser`: Shows jobs submitted by other users.
  - Only jobs on the single UI node (pal-ui-el7.sdfarm.kr, pal-ui02-el7.sdfarm.kr) can be checked.
- `condor_q -alluser -global`: Shows jobs submitted by other users across all UI nodes (pal-ui-el7.sdfarm.kr, pal-ui02-el7.sdfarm.kr).

Analyzing IDLE Jobs in HTCondor
----------------------------------------------------------------------------

To analyze jobs that are in the IDLE state in HTCondor, you can use the `condor_q` command with the `-analyze` or `-better-analyze` options. These options provide detailed information about why a job is in the IDLE state and suggest potential reasons for scheduling issues.

1. Use `condor_q -analyze`:
   - This command provides a basic analysis of the job's status and reasons why it might not be running.

   .. code-block:: console

      condor_q -analyze {JOB_ID}

   Example:
   .. code-block:: console

      condor_q -analyze 123.0

2. Use `condor_q -better-analyze`:
   - This command offers a more detailed analysis compared to `-analyze`, giving deeper insights into the job's scheduling constraints and resource availability.

   .. code-block:: console

      condor_q -better-analyze {JOB_ID}

   Example:
   .. code-block:: console

      condor_q -better-analyze 123.0

By using these commands, you can identify the reasons why a job remains in the IDLE state and take appropriate actions to resolve any issues.

Removing Submitted Condor Jobs
----------------------------------------------------------------------------

- `condor_rm ${JOB_IDS}`
  - JOB_IDS can be found from the `condor_q` result.
  - Example: `condor_rm 9803.0`

- If there are many jobs, you can use braces to specify a range of job IDs.
  - Example: `{9800..9827}`: The job IDs from the start number to the end number.

.. code-block:: console

    [USERID@pal-ui-el7 file_stream]$ condor_rm {25865..25880}
    All jobs matching constraint (ClusterId == 25865 || ClusterId == 25866 || ClusterId == 25867 || ClusterId == 25868 || ClusterId == 25869 || ClusterId == 25870 || ClusterId == 25871 || ClusterId == 25872 || ClusterId == 25873 || ClusterId == 25874 || ClusterId == 25875 || ClusterId == 25876 || ClusterId == 25877 || ClusterId == 25878 || ClusterId == 25879 || ClusterId == 25880) have been marked for removal


Condor Job Prioritization
----------------------------------------------------------------------------

- Jobs are scheduled according to Condor's scheduling policy.
  - For example, if a user submits a large number of jobs and another user submits new jobs, the priority might shift, causing delays in resource allocation for the waiting jobs.

.. code-block:: console

    [USERID@pal-ui02-el7 ~]$ condor_userprio -all
    Last Priority Update:  6/14 13:43
                        Effective     Real   Priority   Res   Total Usage       Usage             Last       Time Since
    User Name            Priority   Priority  Factor   In Use (wghted-hrs)    Start Time       Usage Time    Last Usage
    ------------------ ------------ -------- --------- ------ ------------ ---------------- ---------------- ----------
    OTHERUSERID@sdfarm.kr   34436.42    34.44   1000.00      0      3974.01  5/25/2022 12:16  6/14/2022 12:59    0+00:43
    USERID@sdfarm.kr       343972.75   343.97   1000.00    720     61202.98  5/23/2022 14:29  6/14/2022 13:43      <now>
    ------------------ ------------ -------- --------- ------ ------------ ---------------- ---------------- ----------
    Number of users: 2                                    720     65176.99                   6/13/2022 13:43    0+23:59

- Effective Priority
  - Numerical value indicating the level of resource allocation.
  - Lower values represent higher priority.