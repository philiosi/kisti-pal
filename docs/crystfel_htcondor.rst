==================================================
HTCondor & CrystFEL Tutorial
==================================================

Indexamajig_condorjob example
 * KISTI Storage - /pal/htcondor/htcondor_sample_ori.tar
 * Github - https://github.com/philiosi/indexamajig_htcondor


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
  [USERID@pal-ui-el7 your_path] chmod ug+x 1_exec_file_list_script.sh

  # Script(2_submit_condor_indexing.sh) update from github
  [USERID@pal-ui-el7 your_path] rm -rf {your_path}/htcondor_sample_ori/ue_191027_SFX/proc/cheetah/hdf5/indexamajig_htcondor/2_submit_condor_indexing.sh
  [USERID@pal-ui-el7 your_path] wget https://raw.githubusercontent.com/philiosi/indexamajig_htcondor/main/2_submit_condor_indexing.sh
  [USERID@pal-ui-el7 your_path] chmod ug+x 2_submit_condor_indexing.sh

  # Script(3_exec_indexing.sh) update from github
  [USERID@pal-ui-el7 your_path] rm -rf {your_path}/htcondor_sample_ori/ue_191027_SFX/proc/cheetah/hdf5/indexamajig_htcondor/3_exec_indexing.sh
  [USERID@pal-ui-el7 your_path] wget https://raw.githubusercontent.com/philiosi/indexamajig_htcondor/main/3_exec_indexing.sh
  [USERID@pal-ui-el7 your_path] chmod ug+x 3_exec_indexing.sh
  
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
    ├── [0000082-pal40]
    ├── [0000083-pal40]
    ├── [0000084-pal40]
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
  drwxr-x---. 2 pal pal_users  4096 Sep  6 11:22 0000082-pal40
  drwxr-x---. 2 pal pal_users  4096 Sep  6 11:22 0000083-pal40
  drwxr-x---. 2 pal pal_users  4096 Sep  6 11:22 0000084-pal40
  drwxr-x---. 2 pal pal_users  4096 Sep  6 11:23 0000085-pal40
  drwxr-x---. 2 pal pal_users  4096 Sep  6 11:23 0000086-pal40
  drwxr-x---. 2 pal pal_users  4096 Sep  6 11:23 0000087-pal40
  drwxr-x---. 2 pal pal_users  4096 Sep  6 11:24 0000088-pal40
  drwxr-x---. 2 pal pal_users  4096 Sep  6 11:24 0000089-pal40
  drwxr-x---. 2 pal pal_users  4096 Sep  6 11:24 0000090-pal40
  drwxr-x---. 2 pal pal_users  4096 Sep  6 11:25 0000091-pal40
  drwxr-x---. 2 pal pal_users  4096 Sep  6 11:25 0000101-pal40
  drwxr-x---. 2 pal pal_users  4096 Sep  6 11:26 0000102-pal40
  drwxr-x---. 2 pal pal_users  4096 Sep  6 11:26 0000103-pal40
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
  drwxrwx---. 2 pal pal_users 4096 Jun  3 13:19 0000082-pal40
  drwxrwx---. 2 pal pal_users 4096 Jun  3 13:19 0000083-pal40
  drwxrwx---. 2 pal pal_users 4096 Jun  3 13:19 0000084-pal40
  drwxrwx---. 2 pal pal_users 4096 Jun  3 13:19 0000085-pal40
  drwxrwx---. 2 pal pal_users 4096 Jun  3 13:19 0000086-pal40
  drwxrwx---. 2 pal pal_users 4096 Jun  3 13:19 0000087-pal40
  drwxrwx---. 2 pal pal_users 4096 Jun  3 13:19 0000088-pal40
  drwxrwx---. 2 pal pal_users 4096 Jun  3 13:19 0000089-pal40
  drwxrwx---. 2 pal pal_users 4096 Jun  3 13:19 0000090-pal40
  drwxrwx---. 2 pal pal_users 4096 Jun  3 13:19 0000091-pal40
  drwxrwx---. 2 pal pal_users 4096 Jun  3 13:19 0000101-pal40
  drwxrwx---. 2 pal pal_users 4096 Jun  3 13:19 0000102-pal40
  drwxrwx---. 2 pal pal_users 4096 Jun  3 13:19 0000103-pal40
  
**CASE 3 : Use your own file**

Copy your own data sets to the location below:

   - copyFile location : /pal/{your_path}/{your_directory}/hdf5
  
*Note* : Please refer to the directory structure in the section "1.2. Preparing analysis data".

Create your own `lst` file(s) wherever you want. 

.. code-block:: bash
  :caption: Example of multiple cxi files in a single lst file

  # relative path
  ../0000091-pal40/ue_191027_SFX-r0091-c00.cxi    
  ../0000091-pal40/ue_191027_SFX-r0091-c01.cxi
  # absolute path
  /{your_path}/htcondor_sample/ue_191027_SFX/proc/cheetah/hdf5/0000091-pal40/ue_191027_SFX-r0091-c00.cxi
  
Create your own `lst` file(s) wherever you want.

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
  ../0000082-pal40/ue_191027_SFX-r0082-c00.cxi r0082c00
  ../0000082-pal40/ue_191027_SFX-r0082-c01.cxi r0082c01
  ../0000083-pal40/ue_191027_SFX-r0083-c00.cxi r0083c00 
  ../0000084-pal40/ue_191027_SFX-r0084-c00.cxi r0084c00
  

**Result**
  
.. code-block:: bash
  :caption: created lst file list
    
  [USERID@pal-ui-el7 indexamajig_htcondor]$ ll ./file_list/
  total 209
  -rwxr-x---. 1 USERID USERID 45 Sep 25 13:30 r0080c00.lst
  -rwxr-x---. 1 USERID USERID 45 Sep 25 13:30 r0081c00.lst
  -rwxr-x---. 1 USERID USERID 45 Sep 25 13:30 r0081c01.lst
  -rwxr-x---. 1 USERID USERID 45 Sep 25 13:30 r0082c00.lst
  -rwxr-x---. 1 USERID USERID 45 Sep 25 13:30 r0082c01.lst
  -rwxr-x---. 1 USERID USERID 45 Sep 25 13:30 r0083c00.lst
  -rwxr-x---. 1 USERID USERID 45 Sep 25 13:30 r0084c00.lst
  [USERID@pal-ui-el7 indexamajig_htcondor]$ cat ./file_list/r0079c00.lst
  ../0000079-pal40/ue_191027_SFX-r0079-c00.cxi
 
- `1_exec_file_list_script.sh` generates each `lst` file containing the relative path to one `cxi` file.
- When creating `lst` files manually, multiple `cxi` files can be listed within a single `lst` file for analysis. Both absolute and relative paths for `cxi` files are allowed.

.. code-block:: bash
  :caption: Example of multiple cxi files in a single lst file

  # relative path
  ../0000091-pal40/ue_191027_SFX-r0091-c00.cxi    
  ../0000091-pal40/ue_191027_SFX-r0091-c01.cxi
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
- "-e" : another parameters such as -p, --int-radius, --threshold, --min-srn, --min-fradient

.. [1] max 72 cores

3.2 Output Setting
===================================================

**Please change the target of 'stream_dir'과 'log' if you want. Each directory will be created**

.. code-block:: bash
  :caption: 2_submit_condor_indexing.sh, line 16 to 42

  # debug print option 
  # ex) if [ $DEBUG -eq 1 ]; then echo "[debug] -f option is directory : mf"; fi
  EBUG=1
  
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

3.3 Job Submition
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
  Make sure to check if the paths (absolute/relative) of the files to be entered for each option(`-g`, `-f`, `-o`, `-p`) are correct based on the path of the `./2_submit_condor_indexing.sh` script to be executed.

---------------------------------------------------
4 HTCondor job managing 
---------------------------------------------------

Condor_manual : `HTCondor Version 9.8.1 Manual — HTCondor Manual 9.8.1 documentation <https://htcondor.readthedocs.io/en/latest/index.html>`_.

	- `Submitting a Job — HTCondor Manual 9.8.1 documentation <https://htcondor.readthedocs.io/en/latest/users-manual/submitting-a-job.html>`_.
	- `Managing a Job — HTCondor Manual 9.8.1 documentation <https://htcondor.readthedocs.io/en/latest/users-manual/managing-a-job.html>`_.

4.1. Checking the Condor Queue after Running 2_exec_condor_indexing.sh
====================================================================================================

  Verify the Condor queue status (condor_q) after executing *2_exec_condor_indexing.sh*.
  
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
  
  [USERID@pal-ui-el7 indexamajig_htcondor]$ condor_status
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
  slot1@pal-wn1006.sdfarm.kr   LINUX      X86_64 Unclaimed Idle      0.000  18030167+20:32:27
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
  -rw-r--r--. 1 USERID pal_users 212370 May 25 09:20 xgandalf_r0079c00_SASE_1_condor.error
  -rw-r--r--. 1 USERID pal_users    861 May 25 08:57 xgandalf_r0079c00_SASE_1_condor.log
  -rw-r--r--. 1 USERID pal_users      0 May 25 08:42 xgandalf_r0079c00_SASE_1_condor.out
  -rw-r--r--. 1 USERID pal_users 225473 May 25 09:20 xgandalf_r0080c00_SASE_1_condor.error
  -rw-r--r--. 1 USERID pal_users    861 May 25 09:08 xgandalf_r0080c00_SASE_1_condor.log
  -rw-r--r--. 1 USERID pal_users      0 May 25 08:42 xgandalf_r0080c00_SASE_1_condor.out
  -rw-r--r--. 1 USERID pal_users 160855 May 25 09:20 xgandalf_r0081c00_SASE_1_condor.error
  -rw-r--r--. 1 USERID pal_users   1157 May 25 09:08 xgandalf_r0081c00_SASE_1_condor.log
  -rw-r--r--. 1 USERID pal_users      0 May 25 08:42 xgandalf_r0081c00_SASE_1_condor.out
  -rw-r--r--. 1 USERID pal_users   1957 May 25 08:43 xgandalf_r0081c01_SASE_1_condor.error
  -rw-r--r--. 1 USERID pal_users   1079 May 25 08:43 xgandalf_r0081c01_SASE_1_condor.log
  -rw-r--r--. 1 USERID pal_users      0 May 25 08:42 xgandalf_r0081c01_SASE_1_condor.out
  -rw-r--r--. 1 USERID pal_users 190031 May 25 09:20 xgandalf_r0082c00_SASE_1_condor.error
  -rw-r--r--. 1 USERID pal_users   1158 May 25 09:17 xgandalf_r0082c00_SASE_1_condor.log
  -rw-r--r--. 1 USERID pal_users      0 May 25 08:42 xgandalf_r0082c00_SASE_1_condor.out
  -rw-r--r--. 1 USERID pal_users   1820 May 25 08:43 xgandalf_r0082c01_S

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



