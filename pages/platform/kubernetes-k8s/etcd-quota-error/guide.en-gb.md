---
title: ETCD Quotas error, troubleshooting
excerpt: ''
slug: etcd-quota-error
section: Diagnostics
---

**Last updated April 22<sup>nd</sup>, 2021.**

## Objective

At some point during the life of your Managed Kubernetes cluster, you may encounter the following error which prevents you from altering resources:

```log
"Error from server: rpc error: code = Unknown desc = ETCD storage quota exceeded"
```

**This guide will show you how to debug and resolve this situation.**

## Requirements

- An OVHcloud Managed Kubernetes cluster
- The [`kubectl`](https://kubernetes.io/docs/reference/kubectl/overview/){.external} command-line tool installed

## Instructions

### Background

Each Kubernetes cluster has a dedicated quota on ETCD storage usage, calculated through the following formula:
*Quota = 10MB + (25MB per node)* (capped to 200MB)

For example, a cluster with 3 `b2-7` servers has a quota of 85MB.

The quota can thus be increased by adding nodes, but will never be decreased (even if all nodes are removed) to prevent data loss.

The error mentioned above states that the cluster's ETCD storage usage has exceeded the quota.

**To resolve the situation, resources created in excess need to be deleted.**

### Most common case: misconfigured cert-manager

Most users install cert-manager through Helm, and then move on a bit hastily.

The most common cases of ETCD quota issues come from a bad configuration of [cert-manager](https://cert-manager.io/docs/), making it continuously create `certificaterequest` resources.

This behaviour will fill the ETCD with resources until the quota is reached.

To verify if you are in this situation, you can get the number of `certificaterequest` resources:

```bash
kubectl get certificaterequests -A | wc -l
```

If you have a huge number of certificate requests (+1000 for example), you have found the root cause.

To resolve the situation, we propose the following method:

- Stopping cert-manager

```bash
kubectl scale deployment --replicas 0 cert-manager
```

- Flushing all certificaterequests resources

```bash
kubectl delete certificaterequests --all
```

- Updating cert-manager

There is no generic way to do this, but if you use Helm we recommend you to use it for the update.
[Cert Manager official documentation](https://cert-manager.io/docs/installation/kubernetes/)

- Fixing the issue

We recommend you to take the following steps to troubleshoot your cert-manager, and to ensure that everything is correctly configured:
[Acme troubleshoot](https://cert-manager.io/docs/faq/acme/)

- Starting cert-manager

### Other cases

If cert-manager is not the root cause, you should turn to the other running operators which create kubernetes resources.

We have found that the following resources can sometimes be generated continuously by existing operators:
<br>backups.velero.io
<br>ingress.networking.k8s.io
<br>ingress.extensions

If that still does not cover your case, you can use a tool like [ketall](https://github.com/corneliusweig/ketall) to easily list and count resources in your cluster.

Then you should delete the resources in excess and fix the process responsible for their creation.

## Go further

To learn more about using your Kubernetes cluster the practical way, we invite you to look at our [OVHcloud Managed Kubernetes doc site](../).

Join our [community of users](https://community.ovh.com/en/).
