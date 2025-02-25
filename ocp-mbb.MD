
upgrade: CSI driver scenarios:

https://portal.nutanix.com/page/documents/details?targetId=CSI-Volume-Driver-v3_2:csi-csi-driver-deployment-and-upgrade-scenarios-r.html


###
Precheck: Nutanix CSI driver:
 
for Version 2.6.6 to 3.1 upgrade:

Copy two files from Dev mirror server to prod Mirror server and before we upgrade CSI driver we need to create below two secrets under "openshift-nutanix-csi-driver" namespace.

directory --->/opt/registry/ocpupgrade-4.13-4.14/ntnx-secrets

-rw-r-----. 1 root root 203 Feb 22 16:03 ntnx-pe-secret.yaml      ---> update prism element user password and apply
-rw-r-----. 1 root root 212 Feb 22 16:04 ntnx-pc-secret.yaml      ---> update prism central user password and apply
