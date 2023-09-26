Section 2 CrystFEL & HTCondor
==================================================
Indexamajig_condorjob에 대한 예제는 아래 경로 및 Github repository에서 확인 가능
 * KISTI Storage - /pal/data/htcondor_sample/ue_xxxxxx_SFX/proc/cheetah/hdf5/indexamajig_condorjob/
 * Github - https://github.com/philiosi/kisti-pal

Section 2.1 Data analysis preparation
---------------------------------------------------

2.1.1. 분석용 Script 준비
indexamajig_condorjob 또는 Github 코드를 계정 홈(/pal/home/{account}) 또는 그룹 폴더(/pal/data/{group_dir})로 복사

* case 1) indexamajig_condorjob 복사
.. code-block:: bash

    [USERID@pal-ui-el7 indexamajig_htcondor]$ pwd
    /pal/data/htcondor_sample/ue_191027_SFX/proc/cheetah/hdf5/indexamajig_htcondor
    [USERID@pal-ui-el7 ~]$ cp -rf /pal/data/htcondor_sample/ue_191027_SFX/proc/cheetah/hdf5/indexamajig_htcondor /pal/{home, data}/{where_you_want}

* case 2) Github clone
.. code-block:: bash
    
    [USERID@pal-ui-el7 condor]$ git clone https://github.com/philiosi/indexamajig_htcondor.git
    Cloning into 'indexamajig_htcondor'...
    remote: Enumerating objects: 80, done.
    remote: Counting objects: 100% (80/80), done.
    remote: Compressing objects: 100% (60/60), done.
    remote: Total 80 (delta 39), reused 47 (delta 16), pack-reused 0
    Unpacking objects: 100% (80/80), done.

2.1.2. 분석 데이터 준비

- 폴더 구성

.. code-block:: bash

    {your_directory}
    ├── 0000079-pal40
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
    ├── 0000080-pal40
    ├── 0000081-pal40
    ├── 0000082-pal40
    ├── 0000083-pal40
    ├── 0000084-pal40
    └── indexamajig_htcondor
        ├── 1_exec_file_list_script.sh      # lst 파일 리스트 생성 스크립트
        ├── 2_submit_condor_indexing.sh     # indexamajig condor job 제출
        ├── 3_exec_indexing.sh              # condor job에서 실행할 스크립트
        ├── file_list                       # indexamajig을 실행할 대상 파일
        ├── geom_file1.geom                 # geom 예제 파일
        ├── geom_file2.geom                 # geom 예제 파일
        ├── geom_files                      # geom 예제 파일 모음 디렉토리
        ├── lib                             # 분석에 사용할 lib
        ├── mosflm.lp                       # mosflm 예제 파일
        ├── pdb_file1.pdb                   # pdb 예제 파일
        ├── r009400.lst                     # lst 예제 파일
        ├── README.md
        └── SASE_1.stream                   # stream 예제 파일

Section 2.2 CXI File Lists Creation
---------------------------------------------------

2.2.1 indexamajig condor job을 위한 파일 준비
* case 1) 예제 파일 사용
파일 위치 : /pal/data/htcondor_sample/ue_191027_SFX/proc/cheetah/hdf5/

.. code-block:: bash
  
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

Condor job 테스트를 위한 파일 복사 : 0000079-pal40부터 0000084-pal40까지 6개 데이터 디렉토리 복사

.. code-block:: bash
  
  [USERID@pal-ui-el7 condor]$ cp -rf /pal/data/htcondor_sample/ue_191027_SFX/proc/cheetah/hdf5/{0000079..0000084}-pal40 /pal/{home, data}/{your_directory}
  
* case 2) 직접 파일 준비
파일 준비 위치 : /pal/{home, data}/{your_directory}/
("2.1.2. 분석 데이터 준비" 참조)

2.2.2 CXI 파일 리스트 생성 

* 1_exec_file_list_script.sh 스크립트 실행
  
  - 준비 : "2.2.1 indexamajig condor job을 위한 파일 준비"
    * 각 파일 디렉토리는 특정 keyward로 끝나야 함
      (예) 'pal40'으로 끝나는 디렉토리 : 0000079-pal40, 0000080-pal40, ... 
  
  - 파일 리스트 생성을 위한 output 디렉토리 설정 (Default : ./{your_directory}/file_list)
  
  .. code-block:: bash
    :caption: 1_exec_file_list_script

    # target directory will be created
    target="file_list"
  
  - 실행
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
  
  - 결과
  
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

Section 2.3 Submit indexamajig condor jobs
---------------------------------------------------
