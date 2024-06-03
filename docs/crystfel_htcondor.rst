==================================================
CrystFEL & HTCondor Tutorial
==================================================

Indexamajig_condorjob example
 * KISTI Storage - /pal/data/htcondor_sample_ori.tar
 * Github - https://github.com/philiosi/kisti-pal


---------------------------------------------------
1. Data analysis preparation
---------------------------------------------------

1.1. Preparing Analysis Script

Copy the `indexamajig_condorjob` script or the GitHub code to either the account home directory (`/pal/home/{account}/condor`)[^1] or the group folder (`/pal/data/{group_dir}/condor`)[^2].

[^1 ^2]: In this example, create a "condor" folder in the account home directory or the group folder.

**case 1 : Copy indexamajig_htcondor directory**

.. code-block:: bash

  [USERID@pal-ui-el7 indexamajig_htcondor]$ pwd
  /pal/data/htcondor_sample/ue_191027_SFX/proc/cheetah/hdf5/indexamajig_htcondor_ori
  # check sample files location

  [USERID@pal-ui-el7 ~]$ cp -rf /pal/data/htcondor_sample/ue_191027_SFX/proc/cheetah/hdf5/indexamajig_htcondor_ori.tar /pal/{home, data}/{your_path}
  # Copy the tar file to the specified paths
   
  [USERID@pal-ui-el7 ~]$ cd /pal/{home, data}/{your_path}
  # Change to /pal/{home, data}/{your_path} directory

  [USERID@pal-ui-el7 your_path] tar xvf indexamajig_htcondor_ori.tar
  # Extract the tar file

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

- Directory structure:

.. code-block:: bash

    {condor}
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
        ├── 1_exec_file_list_script.sh      # script to create lst list
        ├── 2_submit_condor_indexing.sh     # submit indexamajig condor job
        ├── 3_exec_indexing.sh              # Script to be executed by the condor job
        ├── file_list                       # Files ('lst' files) to be processed by indexamajig
        ├── geom_file1.geom                 # Example geom file 1
        ├── geom_file2.geom                 # Example geom file 2
        ├── geom_files                      # geom files directory
        ├── lib                             # lib
        ├── mosflm.lp                       # example mosflm file
        ├── pdb_file1.pdb                   # example pdb file
        ├── r009400.lst                     # example lst file
        ├── README.md
        └── SASE_1.stream                   # example stream file


---------------------------------------------------
2. CXI File Lists Creation
---------------------------------------------------

2.1 Preparing files for analysis
===================================================

**case 1) Use example files**

Location of example files

.. code-block:: bash
  :caption: /pal/data/htcondor_sample/ue_191027_SFX/proc/cheetah/hdf5/

  [USERID@pal-ui-el7 condor]$ ll /pal/data/htcondor_sample/ue_191027_SFX/proc/cheetah/hdf5/
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

To use the script for generating lst file list (1_exec_file_list_script.sh), each file directory must end with a specific keyword.
- (Ex) directories ending with 'pal40': 0000079-pal40, 0000080-pal40, ...

.. code-block:: bash
  :caption: (Ex) Copy six data sets from 0000079-pal40 to 0000084-pal40

  [USERID@pal-ui-el7 condor]$ cp -rf /pal/data/htcondor_sample/ue_191027_SFX/proc/cheetah/hdf5/{0000079..0000084}-pal40 /pal/{home, data}/{your_directory}
  
**case 2) Use your own file**

  File location : /pal/{home, data}/{your_directory}
  (Refer to the "2.1.2. Preparing files for analysis")


2.2 Generating CXI file list
===================================================

**Excute '1_exec_file_list_script.sh' script**
  
- Please change the 'target' value to whatever you want (Default : ./{your_directory}/file_list)
  
.. code-block:: bash
  :caption: 1_exec_file_list_script.sh

  # target directory will be created.
  # Please change directory name what you want
  target="file_list"

- Excute

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
  
- Result
  
.. code-block:: bash
  :caption: created lst file list
    
  [USERID@pal-ui-el7 indexamajig_htcondor]$ ll ./file_list/
  total 209
  -rwxr-x---. 1 shna shna 45 Sep 25 13:30 r0079c00.lst
  -rwxr-x---. 1 shna shna 45 Sep 25 13:30 r0080c00.lst
  -rwxr-x---. 1 shna shna 45 Sep 25 13:30 r0081c00.lst
  -rwxr-x---. 1 shna shna 45 Sep 25 13:30 r0081c01.lst
  -rwxr-x---. 1 shna shna 45 Sep 25 13:30 r0082c00.lst
  -rwxr-x---. 1 shna shna 45 Sep 25 13:30 r0082c01.lst
  -rwxr-x---. 1 shna shna 45 Sep 25 13:30 r0083c00.lst
  -rwxr-x---. 1 shna shna 45 Sep 25 13:30 r0084c00.lst
  [USERID@pal-ui-el7 indexamajig_htcondor]$ cat ./file_list/r0079c00.lst
  ../0000079-pal40/ue_191027_SFX-r0079-c00.cxi
 
---------------------------------------------------
3 Submit indexamajig condor jobs
---------------------------------------------------

3.1 HTcondor job submit overview
===================================================

Submitting jobs to HTCondor based on indexamajig inputs
- Sequentially submit jobs for each input geom file(s) and lst file(s)

.. code-block:: bash
  :caption: submit_condor_indexing job submit example

  ./2_submit_condor_indexing.sh -g pal1_new12.geom -i xgandalf -j 72 -f file_list -o SASE_1.stream -p 1vds_sase_temp3.pdb -e "--int-radius=3,4,5 --threshold=600 --min-srn=4 --min-gradient=100000" 

- "-g" : specific geometry file or directory(multiful geom files)
- "-i" : indexing method - mosflm, xds, asdf, dirax, xgandalf
- "-j" : CPU number[2]_
- "-f" : specific lst file(.lst) or directory(multiful lst files)
- "-o" : stream file
- "-p" : pdb file

.. [2] max 72 cores

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
  # 'stream_foler' and 'log' directories are required. Please change directories what you want.
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
  :caption: multiful geom and multiful lst
  
  ./2_submit_condor_indexing.sh -g geom_files -i xgandalf -j 72 -f file_list -o SASE_1.stream -p pdb_file1.pdb -e "--int-radius=3,4,5 --threshold=600 --min-srn=4 --min-gradient=100000"

.. code-block:: bash 
  :caption: multiful geom and single lst
  
  ./2_submit_condor_indexing.sh -g geom_files -i xgandalf -j 72 -f file_list/r009100.lst -o SASE_1.stream -p pdb_file1.pdb -e "--int-radius=3,4,5 --threshold=600 --min-srn=4 --min-gradient=100000"

.. code-block:: bash 
  :caption: sigle geom and multiful lst
  
  ./2_submit_condor_indexing.sh -g geom_files/geom_file1.geom -i xgandalf -j 72 -f file_list -o SASE_1.stream -p pdb_file1.pdb -e "--int-radius=3,4,5 --threshold=600 --min-srn=4 --min-gradient=100000"

.. code-block:: bash 
  :caption: sigle geom and single lst
  
  ./2_submit_condor_indexing.sh -g geom_files/geom_file1.geom -i xgandalf -j 72 -f file_list/r009100.lst -o SASE_1.stream -p pdb_file1.pdb -e "--int-radius=3,4,5 --threshold=600 --min-srn=4 --min-gradient=100000"

