---
title: "KCSA: Security Before the Lab"
author: vishalanarase
date: 2026-08-09 12:00:00 +0530
categories: [The Road to Golden Kubestronaut]
tags: [kubestronaut, kcsa]
render_with_liquid: false
keywords: 'kcsa, kubernetes and cloud native security associate, kubestronaut, kubernetes, cncf'
---

This is the fourth exam on [The Road to Golden Kubestronaut](/posts/kubestronaut-00-introduction/). [CKAD](/posts/kubestronaut-03-ckad/) is behind you. **KCSA** is the quiet one before the security lab.

The Kubernetes and Cloud Native Security Associate exam is multiple choice. You learn the ideas in calm conditions: what to protect, which control does it, and how a threat moves through a cluster. [CKS](/posts/kubestronaut-05-cks/), next, will ask you to apply those ideas with a keyboard.

The Linux Foundation titles this exam Kubernetes and Cloud Native Security Associate. The [CNCF catalog page](https://www.cncf.io/training/certification/kcsa/) shortens that to Kubernetes and Cloud Security Associate. It is the same exam.

## Where this exam sits

KCSA is the associate security exam. There is no prerequisite. It sits here, after CKA and CKAD, because the questions assume you already know what a Pod, a ServiceAccount, and the API server are. If those are still fuzzy, go back to [KCNA](/posts/kubestronaut-01-kcna/) and [CKA](/posts/kubestronaut-02-cka/) before you book this.

Passing CKS later renews KCSA under the Linux Foundation CARE program. The multiple-choice exam you take now stays current when you pass the lab exam, as long as you do that before KCSA expires.

## Exam snapshot

Figures below are from the [CNCF KCSA page](https://www.cncf.io/training/certification/kcsa/), the [Linux Foundation exam page](https://training.linuxfoundation.org/certification/kubernetes-and-cloud-native-security-associate-kcsa/), and the [multiple-choice exam FAQ](https://docs.linuxfoundation.org/tc-docs/certification/faq-mc). Check them again the week you book.

The domain list in the [cncf/curriculum](https://github.com/cncf/curriculum/tree/master/kcsa) README does not match the exam pages. Use the exam pages. They are the ones below.

| | |
| --- | --- |
| Exam | Kubernetes and Cloud Native Security Associate (KCSA) |
| Format | Online, proctored, multiple choice |
| Duration | 90 minutes |
| Pass mark | 75% |
| Validity | 2 years |
| Prerequisite | None |
| Price | $250 for the exam, including one retake |
| Time to schedule | 12 months from purchase |
| Included | An exam preparation handbook. There is no hands-on simulator. |

Your score arrives by email, usually within 24 hours.

During this exam you may not open external sites. The Kubernetes docs that are allowed on CKA and CKAD are not allowed here. Learn the ideas before you sit down.

## Syllabus

Six domains. The two Kubernetes domains are almost half the exam together.

### Kubernetes Cluster Component Security, 22%

Every part of the cluster is an attack surface. Know what each one is trusted to do.

- API server, controller manager, scheduler, kubelet, kube-proxy, and etcd.
- The container runtime and the Pod.
- Container networking and storage.
- The client that talks to the cluster. `kubectl` is part of the security story.

### Kubernetes Security Fundamentals, 22%

The controls you will configure by hand on CKS. Here you need to know what they are for.

- Pod Security Standards and Pod Security Admission.
- Authentication and authorization.
- Secrets.
- Isolation and segmentation.
- Audit logging.
- NetworkPolicy.

### Kubernetes Threat Model, 16%

How an attacker moves, so the controls above have a reason.

- Trust boundaries and data flow inside the cluster.
- Persistence, denial of service, and malicious code in a container.
- An attacker on the network, access to sensitive data, and privilege escalation.

### Platform Security, 16%

The platform around Kubernetes, which CKS will touch again as supply chain and runtime.

- Supply chain and the image repository.
- Observability, service mesh, and PKI.
- Connectivity and admission control.

### Overview of Cloud Native Security, 14%

The map, before the cluster details.

- The 4Cs: cloud, cluster, container, and code.
- Cloud provider and infrastructure security.
- Controls, frameworks, and isolation techniques.
- Artifact repositories, image security, and the security of the application code itself.

### Compliance and Security Frameworks, 10%

The smallest domain, and the one people skip.

- Compliance frameworks and threat modelling frameworks.
- Supply chain compliance.
- Automation and tooling that keep a check repeatable.

## How to prepare

Read the competencies on the [Linux Foundation KCSA page](https://training.linuxfoundation.org/certification/kubernetes-and-cloud-native-security-associate-kcsa/) and explain each one out loud.

1. Start with the 4Cs, so every later control has a layer.
2. Draw the control plane and say what goes wrong if etcd, the API server, or the kubelet is exposed.
3. Connect each threat in the threat-model domain to a control in Kubernetes Security Fundamentals. A NetworkPolicy answers an attacker on the network. Pod Security Standards answer a container that should not be privileged.
4. Book the exam when you can do that without the page in front of you.

The questions are conceptual. People who have already taken CKA still find new ground here, because the exam asks why a control exists, not only how to apply it.

## Resources

Use them in this order.

1. **The official curriculum.** [CNCF KCSA page](https://www.cncf.io/training/certification/kcsa/) and the [Linux Foundation KCSA page](https://training.linuxfoundation.org/certification/kubernetes-and-cloud-native-security-associate-kcsa/). Trust those domain lists over the README in [cncf/curriculum](https://github.com/cncf/curriculum/tree/master/kcsa) until that README matches them.
2. **The project docs, for study, not for the exam room.** [Kubernetes security](https://kubernetes.io/docs/concepts/security/), [Pod Security Standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/), and the [cloud native glossary](https://glossary.cncf.io/).
3. **What registration includes.** The exam attempt, one retake, and an exam preparation handbook. KCSA has no performance-based simulator.
4. **A course to follow.** The [Kubernetes and Cloud Native Security Associate (KCSA) course on KodeKloud](https://learn.kodekloud.com/learn/courses/kubernetes-and-cloud-native-security-associate-kcsa) walks the six domains in order, so you can study the ideas before the CKS lab.

## Before you book

A few rules from the multiple-choice FAQ:

- You need a valid government photo ID. The name on it has to match the name on your exam account.
- Use one monitor, in a private room, on a computer where you can install the PSI secure browser.
- The desk stays clear. No notes, and no docs site on a second machine.
- You get the result by email after the sitting. A pass is 75% or higher.

Then go sit it. The lab comes next, and it will feel familiar if these words are already yours.

The next post is **CKS**, the Certified Kubernetes Security Specialist exam. You can schedule it after you have passed CKA. KCSA is the reason the lab will make sense.
