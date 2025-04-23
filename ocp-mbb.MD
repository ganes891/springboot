
upgrade: CSI driver scenarios:

https://portal.nutanix.com/page/documents/details?targetId=CSI-Volume-Driver-v3_2:csi-csi-driver-deployment-and-upgrade-scenarios-r.html

=============

Nutanix CSI driver opertor upgrade:
 
For Version 2.6.6 to 3.1 upgrade:

Prod Pre work:
1. Create a user "ocp-admin" with admin privilege in Prism central to create the PC secret in OCP.
2. use the existing username "admin" and password to create the below pe secret (can be ignore if there is an existing secret exist with the same name)

Copy two files from Dev mirror server to prod Mirror server to create two new secrets. we will apply this during activity.

Dev mirror directory --->/opt/registry/ocpupgrade-4.13-4.14/ntnx-secrets

-rw-r-----. 1 root root 212 Feb 22 16:04 ntnx-pc-secret.yaml      ---> update prod prism central user and password 
-rw-r-----. 1 root root 203 Feb 22 16:03 ntnx-pe-secret.yaml      ---> update prod prism element user and password

#oc apply -f .

Upgrade steps:

3. upgrade the CSI Operator from 2.6.6 to 3.1 under Operators --> installed operator --> install plan.
4. Monitor if the all daemonsets are recycled automatically and upgrade end with successful message.
5. Create a new PVC under a test project and mount it for a pod to ensure the volume mounts.
6. create a file under the filesystem and test if its read-write mode.

method : Uninstall CSI with delete option checked and see if all pods gone and reinstalled- fix


=============

Prod plan 
Day one -- if 3 hours left will go for 4.14.


==========
the greatest successes come from having the freedom to fail
CER document update requests:
Please update the exact upgrade SHA ID for below respective


========

DR ocp upgrade has been completed (4.12--> 4.13 --> 4.14)

Apr22 activities:

- Ocp DR platform 4.12 --> 4.13 upgrade completed
- post activity health checks done
- All service post upgrade checks completed

Apr23 Activities:

- Ocp DR platform  4.13 to 4.14 upgrade - completed

- Redhat operators upgrades - completed:
    - Compliance operator upgrade as level as prod.
    - Loki operator upgrade same level as prod.
    - ACS operator upgrade same level as prod.
    - Quay opertaor upgrade same level as prod.

- Certified operators upgrades  - completed
    - Nunatix CSI operator upgrade same level as prod

- Elastic search opertor and pvc removal - completed.

- All operator service validation (image push/pull/login/error logs/PVC check) - completed
- cluster health check overall healcheck - completed



