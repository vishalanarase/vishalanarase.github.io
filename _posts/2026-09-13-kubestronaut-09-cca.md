---
title: "CCA: How Pods Talk"
author: vishalanarase
date: 2026-09-13 12:00:00 +0530
categories: [The Road to Golden Kubestronaut]
tags: [kubestronaut, cca]
render_with_liquid: false
keywords: 'cca, cilium certified associate, kubestronaut, cilium, ebpf, cncf'
image: assets/images/kubestronaut/title/cca.jpg
---

This is the networking exam on [The Road to Golden Kubestronaut](/posts/kubestronaut-00-introduction). [OTCA](/posts/kubestronaut-08-otca) was telemetry. **CCA** is how pods actually talk, and how you secure that path.

The Cilium Certified Associate exam is multiple choice. The catalog also phrases the name as Certified Cilium Associate. The abbreviation is CCA either way.

## Where this exam sits

CKA taught you Services and NetworkPolicy as Kubernetes objects. Cilium is the datapath that can enforce them, with eBPF, and can show you the flows. Istio, next, is a service mesh on top of this network. Learn the network before the mesh.

## Exam snapshot

Domains are an even split in the [open curriculum](https://github.com/cncf/curriculum/tree/master/cca). Multiple-choice rules are in the [FAQ](https://docs.linuxfoundation.org/tc-docs/certification/faq-mc). Confirm details on the [Linux Foundation CCA page](https://training.linuxfoundation.org/certification/certified-cilium-associate-cca/).

| | |
| --- | --- |
| Exam | Cilium Certified Associate (CCA) |
| Format | Online, proctored, multiple choice |
| Duration | 90 minutes |
| Pass mark | 75% |
| Validity | 2 years |
| Prerequisite | None |
| Included | No hands-on simulator. |

## Syllabus

Five domains, 20% each.

### Cilium Fundamentals, 20%

What Cilium is for: networking, security, and observability for Kubernetes, built on eBPF. eBPF runs in the kernel, so the datapath does not depend on iptables rules you cannot see. Know that sentence well enough to explain it to someone who has only used kube-proxy.

### Cilium Architecture and Components, 20%

The agent on each node, the operator in the cluster, and the client you use to inspect them. Hubble is the observability side. Know which component answers which question.

### Kubernetes Networking with Cilium, 20%

Pod to pod, Services, and kube-proxy replacement. Identity comes from labels, not only from IP addresses. That is why a policy can follow a workload when the IP changes.

### Observability with Cilium, 20%

Hubble shows flows. You can ask which pods talked, on which port, and whether policy allowed it. This connects back to PCA and OTCA: a flow log is another signal, not a replacement for metrics.

### Securing Workloads with Cilium, 20%

Network policy, including rules that Kubernetes NetworkPolicy cannot express on its own. Identity-based policy. Encryption of traffic between nodes when you turn it on. CKS already asked you to use Cilium for pod-to-pod encryption. Here you learn the product those tasks came from.

## How to prepare

1. Read the [Cilium documentation overview](https://docs.cilium.io/).
2. Install Cilium on a kind cluster and apply one policy that allows a frontend to reach a backend and denies everything else.
3. Open Hubble and find that allowed flow, then find a denied one.
4. Book the exam when you can say what eBPF is doing in that path, without reciting a kernel lecture.

## Resources

1. **The official curriculum.** [cncf/curriculum cca](https://github.com/cncf/curriculum/tree/master/cca) and the [Linux Foundation CCA page](https://training.linuxfoundation.org/certification/certified-cilium-associate-cca/).
2. **The project docs.** [Cilium docs](https://docs.cilium.io/) and [Hubble](https://docs.cilium.io/en/stable/observability/hubble/).
3. **Registration.** The exam attempt and one retake. Confirm the current price on the exam page.

## Before you book

One monitor, a private room, a matching photo ID, a clear desk. A pass is 75% or higher. The result comes by email.

The next post is **[ICA](/posts/kubestronaut-10-ica)**, the Istio Certified Associate exam. It is the hands-on mesh exam. You already know how to be calm in a lab.
