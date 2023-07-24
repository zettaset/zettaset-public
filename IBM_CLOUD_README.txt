Steps for Deploying the Xcrypt HelmChart

Login to the Cluster using the oc command line
e.g:

[docker@localhost CEPH_RH]$ oc login --token=sha256~_ROVLvOU0XbmN6WT8wsk83_FhD1rLeKpFPetIpr7fiI --server=https://c116-e.us-south.containers.cloud.ibm.com:30739

Create the Secrets

2.1  Create the Image Pull Secret

Contact Zettaset for username/password

e.g:

oc create secret docker-registry zts-dockerhub-token --docker-server=https://index.docker.io --docker-username=zetauser --docker-password=Zetapassword --docker-email=user@zettaset.com -n zts-xcrypt

2.2 Create the Ceph Conf Secret

Sample zts.ceph.conf contents are:

[docker@localhost CEPH_RH]$ cat zts.ceph.conf

[global]

mon_host = 172.21.247.49:6789,172.21.3.253:6789,172.21.102.101:6789

[client.admin]

keyring = /etc/ceph/keyring

e.g

oc create secret generic zts-ceph-conf --from-file=ceph.conf=zts.ceph.conf -n zts-xcrypt

2.3 Create the Ceph Keyring Secret

Sample ceph.client.admin.keyring are:

[docker@localhost CEPH_RH]$ cat ceph.client.admin.keyring

[client.admin]

key = AQAIvLFkcmSQLBAAqweYcUiFsTHJZ5j4Rf3GRQ==

e.g

oc create secret generic zts-ceph-keyring --from-file=keyring=ceph.client.admin.keyring -n zts-xcrypt

Verify that the secrets have been created in the zts-xcrypt namespace

e.g:

[docker@localhost CEPH_RH]$ oc get secret -n zts-xcrypt | grep zts-ceph

zts-ceph-conf                                         Opaque                                1      16h

zts-ceph-keyring                                      Opaque                                1      16h

[docker@localhost CEPH_RH]$ oc get secret -n zts-xcrypt | grep zts-docker

zts-dockerhub-token                                   kubernetes.io/dockerconfigjson        1      2d11h

Validation

Run the Validate from the IBM Cloud to automatically install the Xcrypt Helm Chart.
