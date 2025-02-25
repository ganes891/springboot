
upgrade: CSI driver scenarios:

https://portal.nutanix.com/page/documents/details?targetId=CSI-Volume-Driver-v3_2:csi-csi-driver-deployment-and-upgrade-scenarios-r.html

=============

Prod Pre work:

1. Nutanix CSI driver opertor upgrade:
 
For Version 2.6.6 to 3.1 upgrade:

Copy two files from Dev mirror server to prod Mirror server to create two new secrets. we will apply this during activity.

Dev mirror directory --->/opt/registry/ocpupgrade-4.13-4.14/ntnx-secrets

-rw-r-----. 1 root root 203 Feb 22 16:03 ntnx-pe-secret.yaml      ---> update prod prism element user and password
-rw-r-----. 1 root root 212 Feb 22 16:04 ntnx-pc-secret.yaml      ---> update prod prism central user and password 

=============

Prod plan 
Day one -- if 3 hours left will go for 4.14.
