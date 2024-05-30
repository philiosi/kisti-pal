==================================================
Access PAL UI server
==================================================

---------------------------------------------------
Prerequisites
---------------------------------------------------
- Terminal program such as “putty” (available for Windows), “Terminal” (available for Linux and Mac)
- Xming (Windows), XQuartz (Mac) or others relevant for X-windows applications

---------------------------------------------------
KISTI Account Creation
---------------------------------------------------

You can be guided through beamline manager of your experiment.

---------------------------------------------------
KISTI PAL UI Nodes
---------------------------------------------------
+---------------------+----------------------------------------------------------+
| **Hostname**        | pal-ui-el7.sdfarm.kr                                     |
|                     | pal-ui02-el7.sdfarm.kr                                   |
|                     | pal-ui.sdfarm.kr                                         |
+---------------------+----------------------------------------------------------+

---------------------------------------------------
User Interface (UI) Access with SSH
---------------------------------------------------

To access the UI nodes, use the following SSH commands:

.. code-block:: sh

    ssh -Y -p 4280 <kisti_account>@pal-ui-el7.sdfarm.kr

OR

.. code-block:: sh

    ssh -Y -p 4280 <kisti_account>@pal-ui02-el7.sdfarm.kr

OR

.. code-block:: sh

    ssh -Y -p 4280 <kisti_account>@pal-ui.sdfarm.kr

- Only nodes with a registered public IP are accessible. \
  Note: If you have a private IP address as shown below, please ask to your IT support team to get the public IP address to way out from your building.

    * 10.0.0.0 ~ 10.255.255.255(10.0.0.0/8)
    * 172.16.0.0 ~ 172.31.255.255(172.16.0.0/12)
    * 192.168.0.0 ~ 192.168.255.255(192.168.0.0/16)

- When a KISTI account is created, an initial password is assigned.
- After the first login, you must generate an OTP within 3 days following the `OTP Guide <https://gsdc-farm.gitbook.io/gsdc-otp/generate-otp>`_.

---------------------------------------------------
Setting Environment Variables
---------------------------------------------------

Following command is to setup a default environment using HDF5-1.10.5, CrystFEL 0.10.1, Cheetah_FXL.

.. code-block:: sh

    source /pal/lib/setup.sh

To use CrystFEL 1.10.1 with HDF5 1.10.5, run

.. code-block:: sh

    source /pal/lib/setup_crystfel-0.10.1_hdf5-1.10.5.sh

Note that logout from the current session is required to setup a different environment, e.g. CrystFEL 0.9.0 and/or HDF5 1.10.5.

Using cheetah (cheetah_FXL)

.. code-block:: sh

    source setup_cheetah_FXL.sh

Please contact PAL support team by email for any inquiry. Please refer to the helpdesk service section below.

---------------------------------------------------
Helpdesk Service
---------------------------------------------------

KISTI GSDC provides a Helpdesk service regarding the KISTI computing farm, excluding experimentation and data management.

Please email gsdc-support@kisti.re.kr with the keyword "[PAL]" in the subject as below:
ex) [PAL] your subject title

If you miss the keyword "[PAL]" in your email, you will get an auto-reply notice email written in Korean and the mail will not create a ticket of helpdesk service. Just email again with the keyword "[PAL]".
