# kubectl-ssh

Modified the kubectl-ssh plugin from jordanwilson230 updated for containerd: https://github.com/jordanwilson230/kubectl-plugins

This is for clusters that use the containerd backend. This does the following:

Creates a pod with the following:
- image: https://hub.docker.com/repository/docker/bensonyanger/nerdctl (an alpine linux 3.15.0 image with the nerdctl binary copied)
- volumes mounted: (`/var/run/containerd`, `/tmp`, `/var/lib/containerd`)
- permissions: privileged

---
**NOTE**

This pod will be run with privileges and mounts volumes that the containerd runtime is actively using.

**THIS CAN BE VERY DANGEROUS.** 

---

### kubectl ssh
- This is basically a kubectl exec but it gives you the option to choose what user you want to be 

Usage:
  ```./kubectl-ssh [OPTIONAL: -n <namespace>] [OPTIONAL: -u <user>] [OPTIONAL: -c <Container Name>] [REQUIRED: <PodName> ] -- [command]```

Example:
  ```./kubectl-ssh -n challenges -u root challenge-5dbfc6b6b8-n8ckt -- /bin/bash```

Option | Required | Description
------------- | ------------- | -------------
-h | N | Show usage
-d | N | Enable debug mode. Print a trace of each commands
-n | N | The namespace scope for this CLI request
-u | N | User to exec as. Defaults to root
-c | N | Specify container within pod
-- | N | Pass an optional command. Defaults to /bin/sh
