
# OpenShift Virtualization Design Principles

This guide does not address specific problems.
This guide does separate areas of responsibility and guardrails.

It tries to connect services (in a microservice world) in a meaningfull way.

## Purpose

OpenShift Virtualization is designed to

- run all enterprise vitrualization workloads
- run them semaless alongside containers and other OpenShift workloads

## Architecture

### Complement the core
OpenShift by itself is an enterprise distribution of Kubernetes bundled with a range of additional projects from the CNCF (and other) ecosystems.

OpenShift Virtualization by itself is an enterprise distribution of KubeVirt bundled with a range of additional projects from the CNCF ecosystem.

Kubernetes and KubeVirt are at the core.
OpenShift and OpenShift Virtualization complement this core with additional projects in order to solve common problems and solve the specific use-cases.

## Razor

Virtualization -problems- use-cases are commonly sharing requirements with container workloads.
Thus virtualization requirements should be solved in the core - Kubernetes and/or OpenShift - first if they are also relevant for container workloads.

Under pressure we might compromise on this principle.

## Compute

### Nodes

VMs running on nodes should be self contained, and not directly impacted by cluster level issues.

Node-level resource pressure must be solved on a node level (hint: Soft and Hard evictions).

### Pods

VMs are always run in pods.
A pod can be considered to be the VMs runtime environment, this is, because a pod provides most of the hypervisor VM runtime.

### Capacity

#### Requests

All VM pods
- must have requests set
- can have requests < VM memory - this is called memory under requests, or memory over commit

#### Limits

Limits on VMs are 
- not required
- can lead to performance issues if set too tight
- can be required to meet quota requirements

## Storage

### Availability

Storage availability is required for VMs to run. Storage unavailability has to be assumed to lead to VM interruptions and crashes.

### Performance

Storage latency > 100ms is expected to cause problems.
p99 stiorage latency is expected to be sub 10ms for regular enterprise workloads.
