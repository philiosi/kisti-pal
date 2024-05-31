=============================
Data Transfer
=============================

SFTP (Secure File Transfer Protocol, SecureFTP)
-------------------------------

You can access PAL UI server by FTP software like Filezilla, winSCP, etc.
    - Protocol: SFTP or SCP
    - Host: pal-ui-el7.sdfarm.kr / pal-ui02-el7.sdfarm.kr / pal-ui.sdfarm.kr
    - Port: 4280
    - Account: KISTI account and password
        * Note: please refer to the `OTP Guide - Lgin with OTP <https://gsdc-farm.gitbook.io/gsdc-otp/generate-otp>`_. 
    
    .. image:: ../images/sftp.png
        :scale: 70 %
        :align: center   

Globus Online
-------------------------------

KISTI GSDC has the Globus online endpoints, which are set as pal-ui-el7@gsdc-pal.

Steps to transfer data:

    1. Sign up for Globus online and log in (supports CILogon, Google, ORCID iD, globus id).
    
    2. Search for the GSDC-PAL collection using the keyword “gsdc-pal”.

    Note: now you can use only the **"pal-ui-el7@GSDC"** endpoints.

    .. image:: ../images/globus_1.png
        :scale: 70 %
        :align: center   

    3. Log in with your KISTI account (being able to access your home and group directory).

    Note: Please choose **continue**. And then select your identity or identity provider to continue.
    .. image:: ../images/globus_2.png
        :scale: 70 %
        :align: center

    Note: you can login with your KISTI account and password.
        - If you generated an OTP token, then you should input **<password+OTP>** at the password field.
        (ex) password: *1q2w3e4r* and OTP: 123 456, then your password is *"1q2w3e4r123456"*

    .. image:: ../images/globus_3.png
        :scale: 70 %
        :align: center

    4. Add your endpoint by downloading and installing “Globus Connect Personal”.
    5. Transfer your data from the GSDC-PAL collection to your personal collection.

Please note that the transmission performance of Globus online depends entirely on network conditions. KISTI supports high-performance network infrastructure to provide research resources to approximately 200 R&D institutes.