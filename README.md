# Linkerd mTLS workshops

This repo contains material for two workshops that the Buoyant team runs as
part of its [Service Mesh Academy][sma] initiative. Both workshops are focused
on achieving communication security in a Kubernetes cluster using
[Linkerd](https://linkerd.io), the ultra-light and ultra-fast graduated CNCF
service mesh.

## A Deep Dive into mTLS with Linkerd

Mutual TLS (mTLS) is a hot topic in the Kubernetes world, especially for anyone
tasked with getting “encryption in transit” for their applications. In this
workshop, we give you a solid understanding of what mTLS is, how it works, and
how it compares to alternatives. We also walk you through how to set up,
monitor, and understand mTLS between your services on a Kubernetes cluster with
Linkerd, the CNCF service mesh.

You can find the `STEPS` and manifests for this workshop in the [mtls-workshop] directory

## Enterprise PKI in the cloud-native world with Linkerd and cert-manager

Migrating an existing enterprise PKI to Kubernetes can be daunting — there are
so many moving parts to achieving trust across boundaries! From bootstrapping
certificates to terminating TLS at the ingress level, all the way down to
securing communication between workloads, supporting identity management
quickly becomes non-trivial. In this hands-on workshop, members of the
cert-manager and Linkerd teams will show you how to combine the two projects to
manage identity while providing mTLS between your workloads, greatly reducing
the burden on platform teams. You'll learn how to integrate with a CA from an
external PKI, and use it to bootstrap zero-trust across all cluster boundaries.

There are two similar demos that make use of different external issuers. The
presenter's recommendation is that you first go through the materials in the
[pki-workshop-venafi-tpp] directory to get an idea for how cert-manager and
Linkerd can be used together to bootstrap identity and mTLS in a Kubernetes
environment. A good follow-up that the reader can use as a hands-on exercise
can be found in the steps substantiated in the [pki-workshop-vault] directory.

[sma]: https://buoyant.io/service-mesh-academy/
[mtls-workshop]: ./mtls-workshop/STEPS.md
[pki-workshop-vault]: ./pki-workshop-vault/STEPS.md
[pki-workshop-venafi-tpp]: ./pki-workshop-venafi-tpp/README.md
