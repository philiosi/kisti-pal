==================================================
HTCondor & CrystFEL Tutorial
==================================================

Indexamajig_condorjob example
 * KISTI Storage - /pal/htcondor/htcondor_sample_ori.tar
 * Github - https://github.com/philiosi/indexamajig_htcondor

Partialator_condorjob example
 * Github - https://github.com/philiosi/partialator_htcondor

---------------------------------------------------
1. Data analysis preparation
---------------------------------------------------

1.1. Preparing Analysis Script

Copy the `indexamajig_condorjob` script or the GitHub code to either the account home directory (`/pal/home/{account}/{your_dir}`) or the group folder (`/pal/data/{group_dir}/{your_dir}`).

**case 1 : Copy indexamajig_htcondor directory**

.. code-block:: bash

  # check sample files location
  [USERID@pal-ui-el7 indexamajig_htcondor]$ pwd
  /pal/htcondor/

  # Copy the tar file to the specified paths
  [USERID@pal-ui-el7 ~]$ cp -rf /pal/htcondor/htcondor_sample_ori.tar /pal/{home, data}/{your_path}

  # Change to /pal/{home, data}/{your_path} directory 
  [USERID@pal-ui-el7 ~]$ cd /pal/{home, data}/{your_path}
  
  # Extract the tar file
  [USERID@pal-ui-el7 your_path] tar xvf indexamajig_htcondor_ori.tar
  
   # Script(1_exec_file_list_script.sh) update from github
  [USERID@pal-ui-el7 your_path] rm -rf {your_path}/htcondor_sample_ori/ue_191027_SFX/proc/cheetah/hdf5/indexamajig_htcondor/1_exec_file_list_script.sh
  [USERID@pal-ui-el7 your_path] wget https://raw.githubusercontent.com/philiosi/indexamajig_htcondor/main/1_exec_file_list_script.sh
  [USERID@pal-ui-el7 your_path] chmod ug+x 1_exec_file_list_script.sh   # Granting Execution Permission

  # Script(2_submit_condor_indexing.sh) update from github
  [USERID@pal-ui-el7 your_path] rm -rf {your_path}/htcondor_sample_ori/ue_191027_SFX/proc/cheetah/hdf5/indexamajig_htcondor/2_submit_condor_indexing.sh
  [USERID@pal-ui-el7 your_path] wget https://raw.githubusercontent.com/philiosi/indexamajig_htcondor/main/2_submit_condor_indexing.sh
  [USERID@pal-ui-el7 your_path] chmod ug+x 2_submit_condor_indexing.sh  # Granting Execution Permission

  # Script(3_exec_indexing.sh) update from github
  [USERID@pal-ui-el7 your_path] rm -rf {your_path}/htcondor_sample_ori/ue_191027_SFX/proc/cheetah/hdf5/indexamajig_htcondor/3_exec_indexing.sh
  [USERID@pal-ui-el7 your_path] wget https://raw.githubusercontent.com/philiosi/indexamajig_htcondor/main/3_exec_indexing.sh
  [USERID@pal-ui-el7 your_path] chmod ug+x 3_exec_indexing.sh           # Granting Execution Permission
  
*Note* : Please do not miss download updated script from github.
  - 1_exec_file_list_script.sh
  - 2_submit_condor_indexing.sh
  - 3_exec_indexing.sh

**case 2 : Github clone**

.. code-block:: bash
  
  [USERID@pal-ui-el7 ~]$ cd /pal/{home, data}/{your_path}
  # Change to /pal/{home, data}/{your_path} directory. 
  # If you need, then create {your_path} directory using "mkdir {your_path}" commnad.

  [USERID@pal-ui-el7 your_path]$ git clone https://github.com/philiosi/indexamajig_htcondor.git
  Cloning into 'indexamajig_htcondor'...
  remote: Enumerating objects: 80, done.
  remote: Counting objects: 100% (80/80), done.
  remote: Compressing objects: 100% (60/60), done.
  remote: Total 80 (delta 39), reused 47 (delta 16), pack-reused 0
  Unpacking objects: 100% (80/80), done.

1.2. Preparing analysis data

**Directory structure:**

.. code-block:: bash

    [hdf5]
    ├── [0000079-pal40]                     # data sample
    │   ├── cheetah.ini
    │   ├── cheetah.out
    │   ├── cleaned.txt
    │   ├── frames.txt
    │   ├── h5files.txt
    │   ├── log.txt
    │   ├── original.ini
    │   ├── peaks.txt
    │   ├── r0079-detector0-class0-sum.h5
    │   ├── r0079-detector0-class1-sum.h5
    │   ├── r0079-detector0-class2-sum.h5
    │   ├── r0079-detector0-class3-sum.h5
    │   ├── status.txt
    │   ├── ue_191027_SFX-r0079-c00.cxi
    │   └── ue_191027_SFX-r0079-c00.h5
    ├── [0000080-pal40]
    ├── [0000081-pal40]
    └── [indexamajig_htcondor]              # code base directory
        ├── 1_exec_file_list_script.sh      # [script] create lst list
        ├── 2_submit_condor_indexing.sh     # [script] submit indexamajig condor job
        ├── 3_exec_indexing.sh              # [script] to be executed by the condor job
        ├── file_list                       # [Directory] Files ('lst' files) to be processed by indexamajig
        ├── geom_file1.geom                 # [file] Example geom file 1
        ├── geom_file2.geom                 # [file] Example geom file 2
        ├── geom_files                      # [Directory] geom files
        ├── lib                             # [Directory] lib
        ├── mosflm.lp                       # [file] example mosflm file
        ├── pdb_file1.pdb                   # [file] example pdb file
        ├── r009400.lst                     # [file] example lst file
        ├── README.md
        └── SASE_1.stream                   # [file] example stream file


---------------------------------------------------
2. CXI File Lists Creation
---------------------------------------------------

2.1 Preparing files for analysis
===================================================

**[!important]**
To use the script for generating lst file list (1_exec_file_list_script.sh), each file directory must end with a specific keyword.

  - (Ex) directories ending with 'pal40': 0000079-pal40, 0000080-pal40, ...

**CASE 1 : indexamajig_htcondor directory**

Use sample files in the "htcondor_sample_ori"
  - please check location of example files below:

.. code-block:: bash
  :caption: /pal/{your_path}/htcondor_sample_ori/ue_191027_SFX/proc/cheetah/hdf5/

  [USERID@pal-ui-el7 hdf5]$ ll /pal/{your_path}/htcondor_sample_ori/ue_191027_SFX/proc/cheetah/hdf5/
  total 104
  drwxr-x---. 2 pal pal_users  4096 Sep  6 11:20 0000079-pal40
  drwxr-x---. 2 pal pal_users  4096 Sep  6 11:20 0000080-pal40
  drwxr-x---. 2 pal pal_users  4096 Sep  6 11:21 0000081-pal40
  drwxrwx---. 6 pal pal_users  4096 Sep 22 15:28 indexamajig_htcondor

**CASE 2 : Github clone**
Copy sample files in the "/pal/htcondor/hdf5_sample"

.. code-block:: bash
  :caption: (Ex) Copy data sets 

  [USERID@pal-ui-el7 condor]$ pw
  /pal/htcondor/hdf5
  [USERID@pal-ui-el7 condor]$ cp -rf /pal/htcondor/hdf5/pal/{your_path}/{your_directory}/
  [USERID@pal-ui-el7 hdf5]# ll
  total 64
  drwxrwx---. 2 pal pal_users 4096 Jun  3 13:19 0000079-pal40
  drwxrwx---. 2 pal pal_users 4096 Jun  3 13:19 0000080-pal40
  drwxrwx---. 2 pal pal_users 4096 Jun  3 13:19 0000081-pal40
  
**CASE 3 : Use your own file**

Step 1. Copy your own data sets to the location below:

   - copyFile location : /pal/{your_path}/{your_directory}/hdf5
  
*Note* : Please refer to the directory structure in the section "1.2. Preparing analysis data".

Step 2. Create your own `lst` file(s) wherever you want.

.. code-block:: bash
  :caption: Example of cxi file in a single lst file

  # relative path
  ../0000091-pal40/ue_191027_SFX-r0091-c00.cxi    
  # absolute path
  /{your_path}/htcondor_sample/ue_191027_SFX/proc/cheetah/hdf5/0000091-pal40/ue_191027_SFX-r0091-c00.cxi

.. warning::
  When executing `./2_submit_condor_indexing.sh`, make sure to clearly specify the path (absolute or relative) of the `lst` file with the `-f` option.

2.2 Generating CXI file list
===================================================

**Excute '1_exec_file_list_script.sh' script**
  
Step 1 : Please change the 'target' value to whatever you want (Default : ../{your_path}/{your_directory}/hdf5/indexamajig_htcondor/file_list)
  
.. code-block:: bash
  :caption: 1_exec_file_list_script.sh

  # target directory will be created.
  # Please change directory name what you want
  target="file_list"

Step 2 : Run

  - "-d" : applies to directories within the hdf5 directory that contain the keyword(default:pal).

.. code-block:: bash
  :caption: Usage: ./1_exec_file_list_script.sh -d pal40 (default:pal)
  
  [USERID@pal-ui-el7 indexamajig_htcondor]$ ./1_exec_file_list_script.sh                                                                                                           
  Usage: ./1_exec_file_list_script.sh -d pal40 (default:pal)
  [USERID@pal-ui-el7 indexamajig_htcondor]$ ./1_exec_file_list_script.sh -d pal40 
  ../0000079-pal40/ue_191027_SFX-r0079-c00.cxi r0079c00 
  ../0000080-pal40/ue_191027_SFX-r0080-c00.cxi r0080c00 
  ../0000081-pal40/ue_191027_SFX-r0081-c00.cxi r0081c00 
  ../0000081-pal40/ue_191027_SFX-r0081-c01.cxi r0081c01   

**Result**
  
.. code-block:: bash
  :caption: created lst file list
    
  [USERID@pal-ui-el7 indexamajig_htcondor]$ ll ./file_list/
  total 209
  -rwxr-x---. 1 USERID USERID 45 Sep 25 13:30 r0079c00.lst
  -rwxr-x---. 1 USERID USERID 45 Sep 25 13:30 r0080c00.lst
  -rwxr-x---. 1 USERID USERID 45 Sep 25 13:30 r0081c00.lst
  -rwxr-x---. 1 USERID USERID 45 Sep 25 13:30 r0081c01.lst
  [USERID@pal-ui-el7 indexamajig_htcondor]$ cat ./file_list/r0079c00.lst
  ../0000079-pal40/ue_191027_SFX-r0079-c00.cxi
 
- `1_exec_file_list_script.sh` generates each `lst` file containing the relative path to one `cxi` file.
- You can generate `lst` files manually. Both absolute and relative paths for `cxi` files are allowed.

.. code-block:: bash
  :caption: Example of a cxi file in a single lst file

  # relative path
  ../0000091-pal40/ue_191027_SFX-r0091-c00.cxi

  # absolute path
  /{your_path}/htcondor_sample/ue_191027_SFX/proc/cheetah/hdf5/0000091-pal40/ue_191027_SFX-r0091-c00.cxi
  
---------------------------------------------------
3 Submit indexamajig condor jobs
---------------------------------------------------

3.1 HTcondor job submit overview
===================================================

Submitting jobs to HTCondor based on indexamajig inputs
  
  - Sequentially submit jobs for each input geom file(s) and lst file(s)

.. code-block:: bash
  :caption: submit_condor_indexing job submit example

  [USERID@pal-ui-el7 indexamajig_htcondor]$ ./2_submit_condor_indexing.sh -g pal1_new12.geom -i xgandalf -j 72 -f file_list -o SASE_1.stream -p 1vds_sase_temp3.pdb -e "--int-radius=3,4,5 --threshold=600 --min-srn=4 --min-gradient=100000" 

- "-g" : specific geometry file or directory(multiful geom files)
- "-i" : indexing method - mosflm, xds, asdf, dirax, xgandalf
- "-j" : Numbers of CPU[1]_
- "-f" : specific lst file(.lst) or directory(multiful lst files)
- "-o" : stream file
- "-p" : pdb file
- "-e" : another parameters such as -p, -no-check-peaks, --multi, --int-radius, --threshold, --min-srn, --min-fradient, etc.

.. [1] max 72 cores

3.2 Output Setting
===================================================

**Please change the target of 'stream_dir'과 'log' if you want. Each directory will be created**

.. code-block:: bash
  :caption: 2_submit_condor_indexing.sh, line 16 to 42

  # debug print option 
  # ex) if [ $DEBUG -eq 1 ]; then echo "[debug] -f option is directory : mf"; fi
  DEBUG=1
  
  # Input
  # The directory location is determined based on the input parameter.
  geom_dir="" # Do not assign a value. -g option parameter
  lst_dir="" # Do not assign a value. -f option parameter
  
  # Output
  # 'stream_dir' and 'log' directories are required. Please change directories what you want.
  # Default directory are 'file_stream' and 'log'
  stream_dir="file_stream"
  log="log"
  
  # create folder for output and log
  PROCDIR="$( cd "$( dirname "$0" )" && pwd -P )"
  
  # fourc input type
  # - 1010 : 10 multi lst, multi geom
  # - 1001 : 9  multi lst, single geom
  # - 0110 : 6  single lst, multi geom
  # - 0101 : 5  single lst, single geom
  in_type=0
  
  # asign memory
  MEM=360

3.3 Indexmajig Job(HTCondor) Submition
==================================================

- **geom_files** : directory for multiful geom files
- **file_list** : directory for multiful lst files 

.. code-block:: bash
  :caption: multiful geoms and multiful lsts
  
  [USERID@pal-ui-el7 indexamajig_htcondor]$ ./2_submit_condor_indexing.sh -g geom_files -i xgandalf -j 72 -f file_list -o SASE_1.stream -p pdb_file1.pdb -e "--int-radius=3,4,5 --threshold=600 --min-srn=4 --min-gradient=100000"

.. code-block:: bash 
  :caption: multiful geoms and single lst
  
  [USERID@pal-ui-el7 indexamajig_htcondor]$ ./2_submit_condor_indexing.sh -g geom_files -i xgandalf -j 72 -f file_list/r009100.lst -o SASE_1.stream -p pdb_file1.pdb -e "--int-radius=3,4,5 --threshold=600 --min-srn=4 --min-gradient=100000"

.. code-block:: bash 
  :caption: sigle geom and multiful lsts
  
  [USERID@pal-ui-el7 indexamajig_htcondor]$ ./2_submit_condor_indexing.sh -g geom_files/geom_file1.geom -i xgandalf -j 72 -f file_list -o SASE_1.stream -p pdb_file1.pdb -e "--int-radius=3,4,5 --threshold=600 --min-srn=4 --min-gradient=100000"

.. code-block:: bash 
  :caption: sigle geom and single lst
  
  [USERID@pal-ui-el7 indexamajig_htcondor]$ ./2_submit_condor_indexing.sh -g geom_files/geom_file1.geom -i xgandalf -j 72 -f file_list/r009100.lst -o SASE_1.stream -p pdb_file1.pdb -e "--int-radius=3,4,5 --threshold=600 --min-srn=4 --min-gradient=100000"

.. warning::
  Make sure to check the paths (absolute/relative) of the files for each option(`-g`, `-f`, `-o`, `-p`) are correct.

---------------------------------------------------
4 HTCondor job managing 
---------------------------------------------------

Condor_manual : `HTCondor Version 9.8.1 Manual — HTCondor Manual 9.8.1 documentation <https://htcondor.readthedocs.io/en/latest/index.html>`_.

	- `Submitting a Job — HTCondor Manual 9.8.1 documentation <https://htcondor.readthedocs.io/en/latest/users-manual/submitting-a-job.html>`_.
	- `Managing a Job — HTCondor Manual 9.8.1 documentation <https://htcondor.readthedocs.io/en/latest/users-manual/managing-a-job.html>`_.

4.1. Checking the Condor Queue after Running script job
====================================================================================================

  Verify the Condor queue status (condor_q) after executing *job*.
  
  Initially, jobs will be in the IDLE state before resource allocation, then transition to the RUN state according to HTCondor scheduling policies.
  
  Check job status and errors: `Analyzing Jobs in HTCondor <https://kisti-pal.readthedocs.io/en/latest/htcondor_reference.html#analyzing-idle-jobs-in-htcondor>`_
    - `condor_q -analyze {JOB_IDS}`: Shows the scheduling status or error information for the jobs.
    - `condor_q -better-analyze {JOB_IDS}`: more detailed analysis compared to -analyze
    - `condor_q -l {JOB_IDS}`: Provides detailed information about the jobs.

  *Note* : If there are existing jobs submitted by other users, resource allocation might be delayed according to `scheduling policies <https://kisti-pal.readthedocs.io/en/latest/htcondor_reference.html#analyzing-idle-jobs-in-htcondor>`_. Please Refer to the *HTCondor References* chapter for information on job queue and priority.

4.2. HTCondor Resource Status
====================================================================================================

  You can check the status of Condor resources:
    - Verify the allocation (Claimed) status of jobs on each Worker Node.

Example:

.. code-block:: console
  
  [USERID@pal-ui-el7 your]$ condor_status
  Name                         OpSys      Arch   State     Activity LoadAv Mem     ActvtyTime
  slot1@pal-wn1001.sdfarm.kr   LINUX      X86_64 Unclaimed Idle      0.000  18030  0+00:33:44
  slot1_1@pal-wn1001.sdfarm.kr LINUX      X86_64 Claimed   Busy     75.940 368640  0+00:28:54
  slot1@pal-wn1002.sdfarm.kr   LINUX      X86_64 Unclaimed Idle      0.000  18030  0+14:26:17
  slot1_1@pal-wn1002.sdfarm.kr LINUX      X86_64 Claimed   Busy     71.570 368640  0+00:29:42
  slot1@pal-wn1003.sdfarm.kr   LINUX      X86_64 Unclaimed Idle      0.000  18030  0+14:27:53
  slot1_1@pal-wn1003.sdfarm.kr LINUX      X86_64 Claimed   Busy     71.530 368640  0+00:29:41
  slot1@pal-wn1004.sdfarm.kr   LINUX      X86_64 Unclaimed Idle      0.000  18030  0+14:25:42
  slot1_1@pal-wn1004.sdfarm.kr LINUX      X86_64 Claimed   Busy     71.550 368640  0+00:29:42
  slot1@pal-wn1005.sdfarm.kr   LINUX      X86_64 Unclaimed Idle      0.000  18030  0+14:25:41
  slot1_1@pal-wn1005.sdfarm.kr LINUX      X86_64 Claimed   Busy     71.630 368640  0+00:29:42
  slot1@pal-wn1006.sdfarm.kr   LINUX      X86_64 Unclaimed Idle      0.000  18030  0+20:32:27
  slot1_1@pal-wn1006.sdfarm.kr LINUX      X86_64 Claimed   Busy     71.580 368640  0+00:29:36
  slot1@pal-wn1007.sdfarm.kr   LINUX      X86_64 Unclaimed Idle      0.000  18030  0+14:25:22
  slot1_1@pal-wn1007.sdfarm.kr LINUX      X86_64 Claimed   Busy     71.520 368640  0+00:29:35
  slot1@pal-wn1008.sdfarm.kr   LINUX      X86_64 Unclaimed Idle      0.000  18030  0+14:24:48
  slot1_1@pal-wn1008.sdfarm.kr LINUX      X86_64 Claimed   Busy     71.580 368640  0+00:29:02
  slot1@pal-wn1009.sdfarm.kr   LINUX      X86_64 Unclaimed Idle      0.000  18030  0+14:24:31
  slot1_1@pal-wn1009.sdfarm.kr LINUX      X86_64 Claimed   Busy     72.000 368640  0+00:29:39
  Machines Owner Claimed Unclaimed Matched Preempting  Drain
  X86_64/LINUX       18     0       9         9       0          0      0
  Total              18     0       9         9       0          0      0

4.3. Execution Results
====================================================================================================

The indexing process logs are generated in the ../indexamajig_htcondor/log/ directory:
  - \*.error: Indexing log, elapsed time
  - \*.log: condor_submit information
  - \*.out: Output log

Example:

.. code-block:: console

  [USERID@pal-ui-el7 log]$ cd log
  [USERID@pal-ui-el7 log]$ ll
  total 8242
  -rw-r--r--. 1 USERID USERID  795612 Aug 29 12:00 geom_file1_xgandalf_r0079c00_SASE_1_condor.error
  -rw-r--r--. 1 USERID USERID    1838 Aug 29 12:00 geom_file1_xgandalf_r0079c00_SASE_1_condor.log
  -rw-r--r--. 1 USERID USERID       0 Aug 29 11:30 geom_file1_xgandalf_r0079c00_SASE_1_condor.out
  -rw-r--r--. 1 USERID USERID 1038891 Aug 29 12:06 geom_file1_xgandalf_r0080c00_SASE_1_condor.error
  -rw-r--r--. 1 USERID USERID    1837 Aug 29 12:06 geom_file1_xgandalf_r0080c00_SASE_1_condor.log
  -rw-r--r--. 1 USERID USERID       0 Aug 29 11:30 geom_file1_xgandalf_r0080c00_SASE_1_condor.out
  -rw-r--r--. 1 USERID USERID 1127187 Aug 29 12:08 geom_file1_xgandalf_r0081c00_SASE_1_condor.error
  -rw-r--r--. 1 USERID USERID    1162 Aug 29 12:06 geom_file1_xgandalf_r0081c00_SASE_1_condor.log
  -rw-r--r--. 1 USERID USERID       0 Aug 29 11:30 geom_file1_xgandalf_r0081c00_SASE_1_condor.out
  -rw-r--r--. 1 USERID USERID    1706 Aug 29 11:31 geom_file1_xgandalf_r0081c01_SASE_1_condor.error
  -rw-r--r--. 1 USERID USERID    1220 Aug 29 11:31 geom_file1_xgandalf_r0081c01_SASE_1_condor.log
  -rw-r--r--. 1 USERID USERID       0 Aug 29 11:30 geom_file1_xgandalf_r0081c01_SASE_1_condor.out

.. note::
  The naming convention for the log and stream files is as follows:
  
  output = log/{geom_file_name}_{indexing method}_{runnum}_{streamname}_condor.out
  error = log/{geom_file_name}_{indexing method}_{runnum}_{streamname}_condor.error
  log = log/{geom_file_name}_{indexing method}_{runnum}_{streamname}_condor.log

  stream = file_stream/{geom_file_name}_{indexing method}_{runnum}_{streamname}.stream

4.4. Job History
====================================================================================================

View log of HTCondor jobs completed to date(`condor_history <https://htcondor.readthedocs.io/en/latest/man-pages/condor_history.html>`_)

Example:

.. code-block:: console
  
  [USERID@pal-ui-el7 ~]$ condor_history | more
  ID        OWNER      SUBMITTED   RUN_TIME     ST    COMPLETED  CMD
  56235.0   userid     6/3 22:28   0+00:10:11   C     6/4  15:04 ../path/3_exec_indexing.sh ... ommited ... 
  56237.0   userid     6/3 22:28   0+00:09:11   C     6/4  15:04 ../path/3_exec_indexing.sh ... ommited ... 
  56234.0   userid     6/3 22:28   0+00:10:12   C     6/4  15:04 ../path/3_exec_indexing.sh ... ommited ... 
  56233.0   userid     6/3 22:28   0+00:10:11   C     6/4  15:04 ../path/3_exec_indexing.sh ... ommited ... 
  56232.0   userid     6/3 22:28   0+00:10:11   C     6/4  15:04 ../path/3_exec_indexing.sh ... ommited ... 
  56231.0   userid     6/3 22:28   0+00:10:11   C     6/4  15:04 ../path/3_exec_indexing.sh ... ommited ... 
  ... ... ommited ... ...

- ID : The cluster/process id of the job.
- OWNER : The owner of the job.
- SUBMITTED : The month, day, hour, and minute the job was submitted to the queue.
- RUN_TIME : Remote wall clock time accumulated by the job to date in days, hours, minutes, and seconds, given as the job ClassAd attribute RemoteWallClockTime.
- ST : Completion status of the job (C = completed and X = removed).
- COMPLETED : The time the job was completed.
- CMD : The name of the executable.

---------------------------------------------------
5 Submit Partialator condor jobs
---------------------------------------------------

This project(`Partialator HTCondor 저장소 - GitHub <https://github.com/philiosi/partialator_htcondor>`_) provides a set of scripts to run CrystFEL's `partialator` on multiple `.stream` files using HTCondor, where each stream file is processed as an individual HTCondor job.

5.1 Script Description
=======================

1. **CrystFEL_partialator.sh**
   
   - This script is executed by each HTCondor job.
   - It takes a single ``.stream`` file path as an argument, along with symmetry, number of cores, and base output/log directories.
   - It sources the CrystFEL environment (specifically ``/pal/lib/setup_crystfel-0.9.1_hdf5-1.10.5.sh``) and adds ``/pal/htcondor/lib`` to ``LD_LIBRARY_PATH``.
   - Runs ``partialator`` on the given stream file.
   - Outputs ``.hkl`` file to ``<output_dir_base>/<stream_basename>.hkl``.
   - Writes ``partialator``'s stdout and stderr to ``<log_dir_base>/<stream_basename>.out`` and ``<log_dir_base>/<stream_basename>.err`` respectively.

2. **submit_partialator_htcondor.sh**
   
   - The main submission script to be run by the user.
   - Scans an input directory for ``.stream`` files.
   - For each ``.stream`` file found, it submits an HTCondor job that will execute ``CrystFEL_partialator.sh``.
   - Creates base output and log directories if they don't exist.
   - HTCondor's own log files for each job (e.g., ``job.condor.out``, ``job.condor.err``, ``job.condor.log``) are stored in a subdirectory ``<log_dir_base>/condor_job_logs/``.

----

5.2 Directory Structure
=======================

.. code-block:: text

   crystfel_partialator_htcondor/
   ├── CrystFEL_partialator.sh
   ├── submit_partialator_htcondor.sh
   ├── file_stream/
   │   ├── example1.stream
   │   └── example2.stream
   ├── output_hkl/
   ├── logs_partialator_multi/
   │   ├── condor_job_logs/
   │   │   ├── example1.condor.out
   │   │   └── example1.condor.err
   │   └── example1.out
   └── README.md


5.3 Prerequisites
=================

* HTCondor environment configured and accessible.
* CrystFEL installed and the environment setup script (``/pal/lib/setup_crystfel-0.9.1_hdf5-1.10.5.sh``) available on HTCondor worker nodes.
* Shared filesystem between the submission node and worker nodes, so that stream files (accessed via absolute paths) and the executable script are accessible.


5.4 Usage
=========

1. Place ``CrystFEL_partialator.sh`` and ``submit_partialator_htcondor.sh`` in the same directory.
2. Make them executable:

   .. code-block:: bash

      chmod +x CrystFEL_partialator.sh
      chmod +x submit_partialator_htcondor.sh

3. Run the submission script:

   .. code-block:: bash

      ./submit_partialator_htcondor.sh <input_stream_dir> <symmetry> <num_cores> [output_dir_base] [log_dir_base]

   * ``<input_stream_dir>``: Directory containing your input ``.stream`` files.
   * ``<symmetry>``: Symmetry argument for ``partialator`` (e.g., ``p1``, ``c2mm``).
   * ``<num_cores>``: Number of CPU cores to request for each ``partialator`` job.
   * ``[output_dir_base]`` *(Optional)*: Base directory where ``.hkl`` files will be stored. Defaults to ``output_hkl``.
   * ``[log_dir_base]`` *(Optional)*: Base directory for all logs. Defaults to ``logs_partialator_multi``.

**Example:**

.. code-block:: bash

   # Create a directory for your input stream files
   mkdir -p ./my_experiment/stream_files
   # (Copy your .stream files into ./my_experiment/stream_files)

   # Submit jobs
   ./submit_partialator_htcondor.sh <input_stream_dir> <symmetry> <num_cores> [output_dir_base] [log_dir_base]
   ./submit_partialator_htcondor.sh ./my_experiment/stream_files p1 72 ./my_experiment/hkl_output ./my_experiment/all_logs


5.5 Understanding Variables in the Scripts
==========================================

Variables in ``CrystFEL_partialator_executor.sh``
-------------------------------------------------

This script uses variables to manage file paths, script arguments, and environment settings.

**Script Arguments:**

When ``CrystFEL_partialator_executor.sh`` is run (typically by an HTCondor job), it receives five arguments. These are assigned to the following variables at the beginning of the script:

* ``STREAM_FILE_PATH``: The absolute path to the input ``.stream`` file to be processed. (From ``$1``)
* ``SYMMETRY``: The symmetry argument required by the ``partialator`` command (e.g., ``p1``, ``c2mm``). (From ``$2``)
* ``NUM_CORES``: The number of CPU cores allocated for the ``partialator`` job. (From ``$3``)
* ``OUTPUT_DIR_BASE``: The base directory where the output ``.hkl`` file will be saved. (From ``$4``)
* ``LOG_DIR_BASE``: The base directory where ``partialator``'s own log files (stdout and stderr for this specific stream) will be stored. (From ``$5``)

**Key Internal Variables:**

* ``SETUP_SCRIPT``: Defines the path to the CrystFEL environment setup script (e.g., ``/pal/lib/setup_crystfel-0.9.1_hdf5-1.10.5.sh``). This script is sourced to initialize the necessary environment for ``partialator``.
* ``BASENAME``: Derived from ``STREAM_FILE_PATH`` (e.g., if ``STREAM_FILE_PATH`` is ``/data/run1/my.stream``, ``BASENAME`` becomes ``my``). This is used to create unique names for output and log files.
* ``OUTPUT_HKL_FILE``: The full path for the output ``.hkl`` file, constructed as ``${OUTPUT_DIR_BASE}/${BASENAME}.hkl``.
* ``LOG_STDOUT_PARTIALATOR``: The full path for the file that captures ``partialator``'s standard output, constructed as ``${LOG_DIR_BASE}/${BASENAME}.out``.
* ``LOG_STDERR_PARTIALATOR``: The full path for the file that captures ``partialator``'s standard error, constructed as ``${LOG_DIR_BASE}/${BASENAME}.err``.
* ``EXIT_STATUS``: Stores the exit code of the ``partialator`` command to determine if it ran successfully.

**Environment Variables:**

* ``LD_LIBRARY_PATH``: This standard Linux environment variable is appended with ``/pal/htcondor/lib`` if it's not already present. This ensures that necessary shared libraries for the HTCondor environment are found.

Understanding these variables can be helpful if you need to debug the script's execution or trace how file paths are constructed.


Variables in ``submit_partialator_htcondor.sh``
-----------------------------------------------

This script manages the submission of multiple jobs to HTCondor. It uses variables for configuration, path management, and looping through stream files.

**Script Arguments:**

The script accepts three mandatory and two optional arguments:

* ``INPUT_STREAM_DIR``: Directory containing the input ``.stream`` files. (From ``$1``)
* ``SYMMETRY``: Symmetry argument for ``partialator``. (From ``$2``)
* ``NUM_CORES``: Number of CPU cores for each job. (From ``$3``)
* ``OUTPUT_DIR_BASE``: *(Optional)* Base directory for output ``.hkl`` files. Defaults to ``output_hkl``.
* ``LOG_DIR_BASE``: *(Optional)* Base directory for all logs. Defaults to ``logs_partialator_condor``.

**Default Configuration Variables:**

* ``DEFAULT_OUTPUT_DIR_BASE``: Stores the default name for the base output directory (``output_hkl``).
* ``DEFAULT_LOG_DIR_BASE``: Stores the default name for the base log directory (``logs_partialator_condor``).
* ``DEFAULT_REQUEST_MEMORY``, ``DEFAULT_REQUEST_DISK``: Define default memory and disk requests for HTCondor jobs.
* ``TEMP_STREAM_LIST_FILENAME``: Name for a temporary file that lists all found ``.stream`` files.

**Path and File Management Variables:**

* ``PROCDIR``: Stores the absolute path of the directory where ``submit_partialator_htcondor.sh`` itself is located. Determined using ``realpath "$(dirname "$0")"``.
* ``ABS_OUTPUT_DIR_BASE``, ``ABS_LOG_DIR_BASE``: Absolute paths for output and log directories, resolved using ``realpath -m``.
* ``CONDOR_SYSTEM_LOG_SUBDIR``: Subdirectory (``condor_job_logs``) within ``ABS_LOG_DIR_BASE`` where HTCondor’s own log files are stored.
* ``EXECUTABLE_SCRIPT_NAME``: The name of the executor script (``CrystFEL_partialator_executor.sh``).
* ``ABS_EXECUTABLE_PATH``: The absolute path to the executor script, constructed as ``${PROCDIR}/${EXECUTABLE_SCRIPT_NAME}``.
* ``ABS_INPUT_STREAM_DIR``: The absolute path of the input stream directory.
* ``TEMP_STREAM_LIST_PATH``: The full path to the temporary file listing all stream files.

**Loop and Submission Variables (within the while loop):**

* ``SINGLE_STREAM_FILE_PATH``: Path to one ``.stream`` file read from ``TEMP_STREAM_LIST_PATH``.
* ``STREAM_BASENAME``: The base name of the current ``.stream`` file.
* ``SANITIZED_STREAM_BASENAME``: Sanitized version of the basename, replacing problematic characters with underscores.
* ``CONDOR_OUT_LOG``, ``CONDOR_ERR_LOG``, ``CONDOR_LOG_FILE``: Full paths for the HTCondor output, error, and log files for the job.
* ``submission_output_for_log``, ``submission_exit_code``: Capture output and exit status of the ``condor_submit`` command.

**Debugging Variable:**

* ``DEBUG``: Set to ``1`` to enable verbose output for diagnostics (default is ``0``).

**Counter Variables:**

* ``JOB_COUNT``, ``SUCCESS_COUNT``, ``FAILURE_COUNT``: Used to track the number of jobs processed, successful submissions, and failed ones.

These variables are crucial for the script’s ability to find stream files, construct paths, submit jobs to HTCondor, and manage logging.


A Note on Variable Scope
------------------------

In these shell scripts (``CrystFEL_partialator_executor.sh`` and ``submit_partialator_htcondor.sh``), variables are generally *global* within the script where they are defined.  
Once a variable is set, it can be accessed and modified from anywhere in that same script.  
These scripts do not use shell functions with the ``local`` keyword, which would otherwise limit scope to a specific function.





