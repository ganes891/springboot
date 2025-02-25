[root@sgctcrhsatcap01 FTP]# yum info ansible-core
Updating Subscription Management repositories.
Last metadata expiration check: 2:55:52 ago on Mon 24 Feb 2025 10:55:04 PM EST.
Installed Packages
Name         : ansible-core
Epoch        : 1
Version      : 2.14.17
Release      : 1.el9
Architecture : x86_64
Size         : 10 M
Source       : ansible-core-2.14.17-1.el9.src.rpm
Repository   : @System
From repo    : rhel-9-for-x86_64-appstream-rpms
Summary      : SSH-based configuration management, deployment, and task execution system
URL          : http://ansible.com
License      : GPLv3+
Description  : Ansible is a radically simple model-driven configuration management,
             : multi-node deployment, and remote task execution system. Ansible works
             : over SSH and does not require any software or daemons to be installed
             : on remote nodes. Extension modules can be written in any language and
             : are transferred to managed machines automatically.

[root@sgctcrhsatcap01 FTP]#


[root@sgctcrhsatcap01 ansible-core]# yum localinstall ansible-core-2.14.17-1.el9.x86_64.rpm
Updating Subscription Management repositories.
Last metadata expiration check: 3:01:55 ago on Mon 24 Feb 2025 10:55:04 PM EST.
Dependencies resolved.
=============================================================================================================================
 Package                        Architecture     Version                    Repository                                  Size
=============================================================================================================================
Installing:
 ansible-core                   x86_64           1:2.14.17-1.el9            @commandline                               2.6 M
Installing dependencies:
 git-core                       x86_64           2.43.5-2.el9_5             rhel-9-for-x86_64-appstream-rpms           4.4 M
 python3-cffi                   x86_64           1.14.5-5.el9               rhel-9-for-x86_64-baseos-rpms              257 k
 python3-cryptography           x86_64           36.0.1-4.el9               rhel-9-for-x86_64-baseos-rpms              1.2 M
 python3-packaging              noarch           20.9-5.el9                 rhel-9-for-x86_64-appstream-rpms            81 k
 python3-ply                    noarch           3.11-14.el9                rhel-9-for-x86_64-baseos-rpms              111 k
 python3-pycparser              noarch           2.20-6.el9                 rhel-9-for-x86_64-baseos-rpms              139 k
 python3-pyparsing              noarch           2.4.7-9.el9                rhel-9-for-x86_64-baseos-rpms              154 k
 python3-resolvelib             noarch           0.5.4-5.el9                rhel-9-for-x86_64-appstream-rpms            38 k
 sshpass                        x86_64           1.09-4.el9                 rhel-9-for-x86_64-appstream-rpms            30 k

Transaction Summary
=============================================================================================================================
Install  10 Packages

Total size: 9.1 M
Total download size: 6.5 M
Installed size: 38 M
Is this ok [y/N]:

