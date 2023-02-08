# ACM Managed Cluster Generator
This project is to used to create a the manifests used by hive in [Red Hat Advanced Cluster Management for kubernetes \(RHACM\)](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.6/) generate clusters.

This is still a Work In Progress (WIP)
## Role
```
roles/
└── managed-clusters-role
    ├── defaults
    │   └── main
    │       ├── aws_secret.yml
    │       ├── aws.yml
    │       ├── main.yml
    │       ├── secret.yml
    │       ├── vsphere_secret.yml
    │       └── vsphere.yml
    ├── files
    ├── handlers
    │   └── main.yml
    ├── meta
    │   └── main.yml
    ├── README.md
    ├── tasks
    │   └── main.yml
    ├── templates
    │   ├── aws-clusterdeployment.yaml.j2
    │   ├── aws-creds.yaml.j2
    │   ├── aws-infra-machinepool.yaml.j2
    │   ├── aws-install-config.yaml.j2
    │   ├── aws-worker-machinepool.yaml.j2
    │   ├── cluster-install-config-secret.yaml.j2
    │   ├── full-deploy.j2
    │   ├── klusterletaddonconfig.yaml.j2
    │   ├── managedcluster.yaml.j2
    │   ├── namespace.yaml.j2
    │   ├── pull-secret.yaml.j2
    │   ├── ssh-key.yaml.j2
    │   ├── vs-ca-certs.yaml.j2
    │   ├── vs-certs.yaml.j2
    │   ├── vs-clusterdeployment.yaml.j2
    │   ├── vs-creds.yaml.j2
    │   ├── vs-infra-machinepool.yaml.j2
    │   ├── vs-install-config.yaml.j2
    │   └── vs-worker-machinepool.yaml.j2
    ├── tests
    │   ├── inventory
    │   └── test.yml
    └── vars
        └── main
            └── main.yml
```

### Important defaults files
N.B. The defaults files exist under `roles/managed-cluster-role/defaults/main` directory.

All variable files that have `secret` in their name should be [ansible-vault](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html) encrypted if these are being saved on a publically readable SCM.  This is NOT done in this repo so as to make them easy to see their contents for explanation.  Please be sure to encrypt them.

#### main.yml
This contains common variables that are applicable to all clusters.

`publiczonename` : The domain name for the cluster.

`imageset` : The version of OpenShift to start from.  This should match the ImageSet object available to your cluster.

`clustercidr` : Network cidr to use for the cluster.

`servicecidr` : Network cidr for the service network of the cluster.

`ocp_networkType` : Network type to use for the install.  N.B.  OCP 4.12 changes the default to `OVNKubernetes` however in this release of this tool, `OpenShiftSDN` is set as the default.  Feel free to change it to suit your needs.

`infra` : Boolean determining whether to create infractructure machine pools by default.

`addtionalTags` : A YAML dictionary of additional labels to add to a managed cluster.

#### secret.yml
This contains common variables that are applicable to all clusters but this info should NOT be publically visible.  See the `N.B.` above.

`trustBundle` : The additional CA bundle to use if you have your own private Certificate Authority.

`ssh_public_key` : Public ssh key to be used at install.  This variable doesn't need to be encrypted as it is the PUBLIC key but it is kept in here with the `ssh_private_key` to ease of location.

`ssh_private_key` : Private ssh key to be used at install.

#### aws.yml
This contains variables common to the AWS Cloud provider

`aws_worker_instance` : EC2 instance to use for worker instances

`aws_infra_instance` : EC2 instace to use for infra nodes.  If not specified will default to `aws_worker_instance`

`aws_master_instance` : EC2 instance to use for control plane nodes.

`aws_vpccidr` : list of network cidrs to use for the EC2 instances

`aws_region` : AWS Region to install into.

`aws_hostedZone` : AWS Hosted Zone name to install into.

`aws_userTags` : A YAML dictionary of additional tags to add to cluster objects

`aws_proxy` : Proxy setting for the cluster

`aws_noproxy` : list of domains to access without a proxy

`aws_subnets` : list of subnets to use in the VPC

`aws_publish` : Whether the cluster will be Private (`Internal`) or Internet Accessible (`External`)

`aws_credentialsMode` : How the Cloud Credentials operator issues new credentials.

(More to come later)
