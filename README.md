# K8s Basics

This repo has some example config files for creating some basic resources in
kubernetes.


## namespace.yaml
This file creates a new namespace. Namespaces are sort of like folders on a
hard disk. They can be used to group resources and set security boundaries.

Change the `name` value on line 4.

`$ kubectl apply -f namespace.yaml`

## postgres.yaml
This file creates a single pod deployment running postgres. It assumes
that you have already created the StorageClass, PersistentVolume, and PersistentVolumeClaim.

Update the namespace and claimName before running.

`$ kubectl apply -f postgres.yaml`

When the pod comes up it will have been assigned a dynamic IP address.

## postgres-service.yaml
This file creates a DNS hostname on the cluster and a static IP address that other pods in your
namespace can use to connect.

Update the namespace before running.

`$ kubectl apply -f postgres-service.yaml`

## utils.yaml
This file creates a single pod deployment that can be used to connect to the postgres container.
The dockerfile is available [here](https://github.com/groksrc/utils) and the container is hosted [here](https://hub.docker.com/r/groksrc/utils)

Update the namespace and claimName before running. Optionally you can remove the volume and volumeMount. These are there
so that you can look at the filesystem of the PersistentVolumeClaim.

## pv.yaml
This file creates a PersistentVolume. Think of a PersistentVolume like a hard
drive. It attaches a hard drive to your cluster with the Storage Class you specify.

This is a cluster wide configuration and does not need a namespace.

Update the volumeHandle to match the EFS volume you created in the AWS EFS Console.

## pvc.yaml
This file creates a PersistentVolumeClaim, or claim on some storage of a PersistentVolume.
Think of it like a hard drive partition. You must partition your hard drive before you
can put anything on it.

The pvc does get mapped to a namespace and makes the storage available to containers in
that namespace.

If the PVC is deleted, the underlying storage can be either deleted or unbound depending on your
settings, so be careful!

Change the name and namespace before running.

`$ kubectl apply -f pvc.yaml`

## storageclass.yaml

Kubernetes clusters can be connected to lots of different storage backends. In
the case of AWS two primary examples are EFS [Elastic File System](https://aws.amazon.com/efs/) and
EBS [Elastic Block Store](https://aws.amazon.com/ebs/).

This file makes the kubernetes cluster aware of EFS via the `provisioner` and gives
the storage class the name of `efs-sc`.

This is a cluster wide configuration and does not need a namespace.