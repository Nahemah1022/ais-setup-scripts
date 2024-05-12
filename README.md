# AIStore Cloud Deployment with Terraform

This directory contains Terraform definitions and scripts that enable deploying AIStore clusters on Kubernetes in the cloud using GCP. After setting up Kubernetes with Terraform, `kubectl` and `helm` are used to deploy AIStore on Kubernetes.
The main script (`deploy.sh`) will guide you through the steps to set up the AIStore cluster.

Pre-requisites:

* A GCP account, with GKE API enabled.
* Terraform, kubectl, and helm client tools installed on your workstation (see appendix for installation details).

## Google

In the `gcp/main.tf` file, several variables can be adjusted to your preferences:
* `zone` - the zone in which the cluster will be deployed.
* `machine_type` - the machine type used for GKE nodes.
* `machine_preemptible` - determines if the machine is preemptible.

## Deploy

To deploy a new Kubernetes + AIStore cluster, run the `./deploy.sh all` script and follow the instructions.
When the script successfully finishes, the AIStore cluster should be accessible and ready to use.

```console
$ ./deploy.sh all
```

Alternatively, deploy AIStore to an existing Kubernetes cluster as follows:

```console
$ ./deploy.sh ais
```

```console
$ ./deploy.sh k8s
```

### Supported Arguments

`./deploy.sh DEPLOY_TYPE [--flag=value ...]`

There are 3 `DEPLOY_TYPE`s:
* `all` - start nodes on GCP, start K8s cluster, and deploy AIStore on K8s nodes.
* `ais` - only deploy AIStore on K8s nodes, assumes K8s cluster is already deployed.
* `k8s` - only deploy K8s cluster, without deploying AIStore.
* `dashboard` - start K8s dashboard connected to started K8s cluster.

| Flag | Description | Default value |
| ---- | ----------- | ------------- |
| `--cloud` | Cloud provider to be used (`gcp`). | - |
| `--node-cnt` | Number of instances/nodes to be started. | - |
| `--disk-cnt` | Number of disks per instance/node. | - |
| `--cluster-name` | Name of the Kubernetes cluster. | - |
| `--wait` | Maximum timeout to wait for all the Pods to be ready. | `false` |
| `--aisnode-image` | The image name of `aisnode` container. | `aistore/aisnode:3.4` |
| `--admin-image` | The image name of `admin` container. | `aistore/admin:3.4` |
| `--dataplane` | Network dataplane to be used (`kube-proxy`). | `kube-proxy` |
| `--expose-external` | Will expose AIStore cluster externally by assigning an external IP. | - |
| `--help` | Show help message. | - |

## Destroy

To remove and clean up the cluster, use the `destroy.sh` script, which will guide you through the necessary steps.

### Supported arguments

`./destroy.sh DESTROY_TYPE [--flag=value ...]`

There are 2 `DESTROY_TYPE`s:
* `all` - stop AIStore Pods and destroy started K8s nodes.
* `ais` - only stop AIStore Pods so that the cluster can be redeployed.

| Flag | Description |
| ---- | ----------- |
| `--preserve-disks` | Do not remove persistent volumes. Data on targets will be available on the next deployment. Not supported with `all`. |
| `--help` | Show help message. |
