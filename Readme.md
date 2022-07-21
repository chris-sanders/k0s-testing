# Overview

This repository contains configurations I've been using to test k0s. The k0sctl tool is interesting as a way to quickly provision clusters of machines and I liked that it's claimed to be just vanilla Kubernetes.

## Observations

While working on this I've found:

Things to watch out for
* kotsctl is only aware of http and https helm charts. [Ref](https://github.com/k0sproject/k0s/issues/1934)
* The helm reconciler is very fragile [Ref](https://github.com/k0sproject/k0s/issues/1492) and a bad config option will leave it stuck requiring you to reinstall from scratch
* Doing a reset is fairly reliable but the nodes networking is left in a poor state, fixed with a node reboot. I haven't tested if Calcio instead of kube-router fixes this
* The manifest reconciler doesn't handle namespaces, they must be in the manifest. This means `helm template` can not be used to generate manifests (if say you had an oci base repo you needed to use)
* all logs are in once place, I don't love this some may. If i need to see what went wrong with a specific component I have to grep the log file and tailing it is useless
* You don't appear to be able to restart a single component, it's restart them all, all the time. A 'live reload' of some sort would be very nice

Thing that work very well:
* kotsctl is really convenient, works with existing keyring, and generally was a pleasure to connect to and setup machines
* Being able to include any helm chart you want and have it show up with a clean cluster is very handy for known reproduction environments
* While I've only tried it once or twice, upgrades seem to work well and I like that the versioning includes the specific k8s version
