# kubectl-ssh

Modified the kubectl-ssh plugin from jordanwilson230 updated for containerd: https://github.com/jordanwilson230/kubectl-plugins

This is for clusters that use the containerd backend. 

CRI support was added starting with [Kubernetes v1.7](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.7.md#container-runtime-interface). Docker runtime support will no longer be supported starting from [Kubernetes v1.24](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.24.md#urgent-upgrade-notes) with the removal of `dockershim`.

This script does the following:

Creates a pod with the following:
- image: https://hub.docker.com/repository/docker/bensonyanger/nerdctl (an alpine linux 3.15.0 image with the nerdctl binary copied)
- volumes mounted: (`/var/run/containerd`, `/tmp`, `/var/lib/containerd`)
- permissions: privileged

After this pod is created, you will kubectl exec into this new pod. From here, you should have a shell into the container you requested.

After exiting from the pod for any reason, the pod will be deleted.

## NOTE

This pod will be run with privileges and mounts volumes that the containerd runtime is actively using.

**THIS CAN BE VERY DANGEROUS.** 

## Prerequisites

- [kubectl](https://kubernetes.io/docs/tasks/tools/)

## Install

### Linux

#### cURL
```
curl -L https://raw.githubusercontent.com/BensonYanger/kubectl-ssh/main/kubectl-ssh -o kubectl-ssh
chmod +x kubectl-ssh
sudo mv kubectl-ssh /usr/local/bin
```

#### Wget
```
wget https://raw.githubusercontent.com/BensonYanger/kubectl-ssh/main/kubectl-ssh
chmod +x kubectl-ssh
sudo mv kubectl-ssh /usr/local/bin
```

- To use this as a kubectl plugin, kubectl requires you move it to any directory on your `PATH`. Otherwise, you will need to invoke the script manually.

## Script
- This is basically a kubectl exec but it gives you the option to choose what user you want to be 

Usage:
  ```./kubectl-ssh [OPTIONAL: -n <namespace>] [OPTIONAL: -u <user>] [OPTIONAL: -c <Container Name>] [REQUIRED: <PodName> ] -- [command]```
  
If installed as a plugin:
  ```kubectl ssh [OPTIONAL: -n <namespace>] [OPTIONAL: -u <user>] [OPTIONAL: -c <Container Name>] [REQUIRED: <PodName> ] -- [command]```

---

Example:
  ```./kubectl-ssh -n challenges -u root challenge-5dbfc6b6b8-n8ckt -- /bin/bash```
  
If installed as a plugin:
  ```kubectl ssh -n challenges -u root challenge-5dbfc6b6b8-n8ckt -- /bin/bash```

---
**Flags**

Option | Required | Description
------------- | ------------- | -------------
-h | N | Show usage
-d | N | Enable debug mode. Print a trace of each commands
-n | N | The namespace scope for this CLI request
-u | N | User to exec as. Defaults to root
-c | N | Specify container within pod
-- | N | Pass an optional command. Defaults to /bin/sh
