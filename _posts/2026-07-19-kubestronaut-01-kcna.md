---
title: "KCNA: The Front Door"
author: vishalanarase
date: 2026-07-19 12:00:00 +0530
categories: [The Road to Golden Kubestronaut]
tags: [kubestronaut, kcna]
render_with_liquid: false
keywords: 'kcna, kubernetes and cloud native associate, kubestronaut, kubernetes, cncf'
---

This is the first exam on [The Road to Golden Kubestronaut](/posts/kubestronaut-00-introduction/).

**KCNA** is the Kubernetes and Cloud Native Associate exam. It is multiple choice, and it is the front door. You learn the words, the shape of a cluster, and a map of the cloud native landscape, so the later exams feel like a language you already speak.

You can sit this with no other certification. When you pass, you have started. The next post on this road is **CKA**, where you will type the commands this exam only asks you to recognize.

## Where this exam sits

Sixteen exams is a long road. KCNA is the step that makes the rest readable.

The [CNCF curriculum](https://github.com/cncf/curriculum/tree/master/kcna) describes it as a beginner-friendly on-ramp for people who work in the cloud native ecosystem, developers and non-developers alike. A certified KCNA can explain Kubernetes fundamentals, deploy an application with basic `kubectl`, name the parts of a cluster, and place projects such as Prometheus, Envoy, and GitOps tools on the map. That is the whole job of this exam. CKA, CKAD, and CKS assume you already have it.

## Exam snapshot

Figures below are from the [CNCF KCNA page](https://www.cncf.io/training/certification/kcna/), the [Linux Foundation exam page](https://training.linuxfoundation.org/certification/kubernetes-cloud-native-associate/), and the [multiple-choice exam FAQ](https://docs.linuxfoundation.org/tc-docs/certification/faq-mc). Check those pages again the week you book. Prices and domains do move.

| | |
| --- | --- |
| Exam | Kubernetes and Cloud Native Associate (KCNA) |
| Format | Online, proctored, multiple choice |
| Duration | 90 minutes |
| Pass mark | 75% |
| Validity | 2 years |
| Prerequisite | None |
| Price | $250 for the exam, including one retake |
| Time to schedule | 12 months from purchase |
| Included | An exam preparation handbook. There is no hands-on simulator. |

Your score report arrives by email, usually within 24 hours.

KCNA is part of the Linux Foundation CARE program. Passing **CKA** or **CKAD** renews KCNA automatically. The associate exam you take now stays alive when you continue down this road, as long as you pass one of those two before KCNA expires. Renewing by retaking KCNA is the other option, and that retake has to happen before the expiration date.

## Syllabus

The open curriculum and the exam pages agree on four domains. Spend your hours in something close to these proportions.

### Kubernetes Fundamentals, 44%

This is almost half the exam. Learn it until you can explain it without notes.

- **Core concepts.** A Pod is the smallest thing Kubernetes runs. A Deployment keeps a desired number of Pods. A Service gives those Pods a stable way to be reached. A ConfigMap and a Secret hold configuration. A Namespace is a boundary inside the cluster.
- **Administration.** The control plane decides what should run. The nodes do the running. `kube-apiserver`, `etcd`, the scheduler, and `kubelet` each have one job. You should be able to say which piece answers when you run `kubectl`.
- **Scheduling.** The scheduler places a Pod on a node that can take it. Requests, limits, taints, and tolerations are the vocabulary. You are not asked to tune a production scheduler. You are asked to know why a Pod landed where it did.
- **Containerization.** An image is the package. A container is a running instance of that image. Kubernetes runs containers inside Pods. If those two sentences are solid, this competency is in reach.

The Kubernetes documentation on [components](https://kubernetes.io/docs/concepts/overview/components/), [workloads](https://kubernetes.io/docs/concepts/workloads/), and [services](https://kubernetes.io/docs/concepts/services-networking/) is the text to read for this domain.

### Container Orchestration, 28%

This domain is why a cluster exists: it keeps the application running after you have described what you want.

- **Networking.** Every Pod gets an IP. Services select Pods by label. Ingress is how traffic from outside reaches a Service. You want the path of a request, from the user to the container, in your own words.
- **Security.** Authentication, authorization, and what a ServiceAccount is for. NetworkPolicy is the idea that Pods do not have to talk to every other Pod. This is awareness, not the CKS lab.
- **Troubleshooting.** Pending, CrashLoopBackOff, and ImagePullBackOff are states you should recognize and explain. The exam asks what those symptoms mean.
- **Storage.** A Volume outlives a single container in the Pod. A PersistentVolume and a PersistentVolumeClaim separate "the disk exists" from "this application wants the disk."

### Cloud Native Application Delivery, 16%

This is how software gets from a repository onto the cluster.

- **Application delivery.** Continuous integration builds and tests. Continuous delivery rolls the result out. GitOps is delivery where Git holds the desired state and a controller reconciles the cluster to match it. You will meet GitOps again in CGOA and CAPA. Here, know what problem it solves.
- **Debugging.** You can reason about a failing rollout: a bad image, a failed probe, a config that does not match the code. `kubectl` commands such as `get`, `describe`, and `logs` are the ones to recognize.

### Cloud Native Architecture, 12%

The smallest domain, and the one that introduces the rest of this series.

- **Observability.** Metrics, logs, and traces answer different questions. Prometheus is the metrics tool you will sit a whole exam on later. Here, know what it is for.
- **Ecosystem and principles.** Containers, microservices, immutable infrastructure, and declarative configuration. Scalability, reliability, and portability are the reasons Kubernetes exists, not slogans to memorize in isolation.
- **Community.** The [CNCF landscape](https://landscape.cncf.io/) groups projects: orchestration, networking, service mesh, observability, provisioning, and the rest. You do not need to memorize the chart. You need to know which neighborhood a tool lives in, and that the [cloud native glossary](https://glossary.cncf.io/) is where the shared definitions live.

## How to prepare

Give Kubernetes Fundamentals most of the calendar. If that domain is shaky, the other three have nothing to attach to.

A practical way through:

1. Read the four domains on the [curriculum page](https://github.com/cncf/curriculum/tree/master/kcna) once, so you know the edges of the exam.
2. Run a cluster with [kind](https://kind.sigs.k8s.io/) or [minikube](https://minikube.sigs.k8s.io/docs/start/). Create a Deployment, expose it with a Service, and read `kubectl describe` until the words in the syllabus are things you have seen.
3. Walk one page of the CNCF landscape and say out loud what Prometheus, Envoy, and a GitOps tool are for.
4. Book the exam when you can explain a Pod, a Service, and a control plane without looking them up.

The questions are conceptual, and people who already hold CKA still describe them as broader than a lab exam. Treat "associate" as the level of the credential, not as a promise that the paper will be obvious.

## Resources

Use them in this order.

1. **The official curriculum.** [CNCF KCNA page](https://www.cncf.io/training/certification/kcna/) for the domains and weights. [Linux Foundation KCNA page](https://training.linuxfoundation.org/certification/kubernetes-cloud-native-associate/) for the competencies, the price, and the handbook that comes with registration. [cncf/curriculum](https://github.com/cncf/curriculum/tree/master/kcna) for the same domains in the open repository.
2. **The project docs.** [Kubernetes concepts](https://kubernetes.io/docs/concepts/), the [CNCF glossary](https://glossary.cncf.io/), and the [CNCF landscape](https://landscape.cncf.io/).
3. **What registration includes.** The exam attempt, one retake, and an exam preparation handbook. KCNA has no performance-based simulator.
4. **A course to follow.** The [Kubernetes and Cloud Native Associate (KCNA) course on KodeKloud](https://learn.kodekloud.com/learn/courses/kubernetes-and-cloud-native-associate-kcna) walks the exam in order, so you can study the four domains without assembling the syllabus yourself. The CNCF curriculum also points to [Introduction to Kubernetes on edX](https://www.edx.org/course/introduction-to-kubernetes), [Introduction to Linux on edX](https://www.edx.org/course/introduction-to-linux), [Kubernetes and Cloud Native Essentials (LFS250)](https://training.linuxfoundation.org/training/kubernetes-and-cloud-native-essentials-lfs250/), and the [freeCodeCamp KCNA course](https://www.youtube.com/watch?v=AplluksKvzI). LFS250 can be bought with the exam. The edX courses and the freeCodeCamp course are enough to start if you would rather begin with free material.

## Before you book

A few rules from the multiple-choice FAQ, worth knowing before you pick a slot:

- You need a valid government photo ID. The name on it has to match the name on your exam account.
- Use one monitor, in a private room, on a computer where you can install the PSI secure browser. A locked-down work laptop is a common way to lose the morning.
- The desk stays clear. No notes, no second device.
- You get the result by email after the sitting. A pass is 75% or higher.

Then go sit it. One exam is the whole assignment.

The next post is **[CKA](/posts/kubestronaut-02-cka)**, the Certified Kubernetes Administrator exam. KCNA taught you the words. CKA asks you to operate the cluster.
