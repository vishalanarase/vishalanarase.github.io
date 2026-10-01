---
title: "CKAD: Building What Runs"
author: vishalanarase
date: 2026-08-02 12:00:00 +0530
categories: [The Road to Golden Kubestronaut]
tags: [kubestronaut, ckad]
render_with_liquid: false
keywords: 'ckad, certified kubernetes application developer, kubestronaut, kubernetes, cncf'
---

This is the third exam on [The Road to Golden Kubestronaut](/posts/kubestronaut-00-introduction/). [CKA](/posts/kubestronaut-02-cka) taught you to run the cluster. **CKAD** asks you to build what runs on it.

The Certified Kubernetes Application Developer exam is hands-on, like CKA. For two hours you design, configure, expose, and watch an application. The cluster is already there. Your job is the software on top of it.

## Where this exam sits

CKAD is the developer exam. You define application resources, choose the right workload, wire configuration and secrets, and expose the application to traffic. You already know the cluster pieces from CKA. Here those pieces show up as the environment your application has to live in.

There is no prerequisite. Passing CKAD also renews KCNA under the Linux Foundation CARE program, the same way passing CKA does. Either exam keeps that first certificate current.

## Exam snapshot

Figures below are from the [CNCF CKAD page](https://www.cncf.io/training/certification/ckad/), the [Linux Foundation exam page](https://training.linuxfoundation.org/certification/certified-kubernetes-application-developer-ckad/), and the [CKA, CKAD, and CKS FAQ](https://docs.linuxfoundation.org/tc-docs/certification/faq-cka-ckad-cks). Check them again the week you book. CKAD is already on a newer Kubernetes minor than CKA.

| | |
| --- | --- |
| Exam | Certified Kubernetes Application Developer (CKAD) |
| Format | Online, proctored, performance-based. You solve tasks on a command line. |
| Tasks | 15 to 20 |
| Duration | 2 hours |
| Pass mark | 66% |
| Kubernetes version | v1.37 |
| Validity | 2 years |
| Prerequisite | None |
| Price | $445 for the exam, including one retake |
| Time to schedule | 12 months from purchase |
| Included | Two Killer.sh simulator attempts. Each attempt stays open for 36 hours and has 17 graded questions. |

Your score arrives by email, usually within 24 hours.

## Syllabus

The [open curriculum](https://github.com/cncf/curriculum/tree/master/ckad) and the exam pages use the same five domains. Configuration and security is the largest. Spend the most time there.

### Application Environment, Configuration and Security, 25%

This is how an application is allowed to run, and what it is allowed to see.

- ConfigMaps and Secrets. Create them and consume them from a Pod.
- Resource requests, limits, and quotas. A container that asks for more than the namespace allows will not schedule.
- ServiceAccounts. The identity the Pod uses when it talks to the API.
- Authentication, authorization, and admission control, at the level an application developer meets them.
- SecurityContext and Linux capabilities. What the process inside the container is allowed to do.
- CRDs and operators. You can discover a resource that extends Kubernetes and use it.

### Application Design and Build, 20%

You choose the shape of the application before you ship it.

- Container images. Define, build, and modify an OCI image.
- The right workload. A Deployment, a DaemonSet, a Job, or a CronJob, and why one of them fits.
- Multi-container Pods. An init container that prepares the way, and a sidecar that stays beside the application.
- Volumes. Persistent when the data must outlive the Pod, ephemeral when it only has to outlive one container.

### Application Deployment, 20%

You change a running application without losing it.

- Deployments and rolling updates.
- Blue/green and canary, using Kubernetes primitives.
- Helm, to install an existing package.
- Kustomize, to patch a manifest without forking it.

### Services and Networking, 20%

You make the application reachable, and you decide who may talk to it.

- Services, and what to check when traffic does not arrive.
- Ingress rules that expose the application.
- NetworkPolicy, at a basic level. Which Pods are allowed to connect.

Gateway API is a CKA topic. It is not in the CKAD competency list. Confirm the live page before you study it for this exam.

### Application Observability and Maintenance, 15%

You can tell whether the application is healthy, and you can fix it when it is not.

- Probes and health checks.
- Container logs.
- Built-in CLI tools for watching the application.
- API deprecations. A manifest that used to work can fail because the API moved.
- Debugging. The same `kubectl` habits as CKA, aimed at the application instead of the node.

## How to prepare

The exam assumes you are comfortable with container images and with cloud native application ideas. If those are still new, spend a day on them before the Kubernetes tasks.

1. Read the five domains on the [Linux Foundation CKAD page](https://training.linuxfoundation.org/certification/certified-kubernetes-application-developer-ckad/).
2. Practice on a cluster at Kubernetes v1.37, or as close as you can get. Build an image, run it as a Deployment, add a sidecar, mount a Secret, expose it with a Service and an Ingress, and add a NetworkPolicy.
3. Practice from the docs you may open during the exam: [kubernetes.io/docs](https://kubernetes.io/docs/), [kubernetes.io/blog](https://kubernetes.io/blog/), and [helm.sh/docs](https://helm.sh/docs/). Search on the docs site is allowed. Opening a result that leaves those sites is not. Gateway API docs are allowed on CKA, not on CKAD.
4. Use both Killer.sh attempts. Each one is 17 questions and stays up for 36 hours.

The clock is the same as CKA. Fifteen to twenty tasks in two hours means you finish the ones you know and come back to the rest. `kubectl` is aliased to `k` on the exam hosts. Copy and paste in that terminal is `Ctrl+Shift+C` and `Ctrl+Shift+V`.

## Resources

Use them in this order.

1. **The official curriculum.** [CNCF CKAD page](https://www.cncf.io/training/certification/ckad/) for the domains and weights. [Linux Foundation CKAD page](https://training.linuxfoundation.org/certification/certified-kubernetes-application-developer-ckad/) for the competencies and the Kubernetes version. The open curriculum lives in [cncf/curriculum](https://github.com/cncf/curriculum/tree/master/ckad).
2. **The docs you are allowed to read during the exam.** [Kubernetes documentation](https://kubernetes.io/docs/), the [Kubernetes blog](https://kubernetes.io/blog/), and [Helm's docs](https://helm.sh/docs/). The allowed list is on the [Linux Foundation resources page](https://docs.linuxfoundation.org/tc-docs/certification/certification-resources-allowed).
3. **The simulator that comes with the exam.** Two Killer.sh attempts, described on the Linux Foundation CKAD page. Use both before you sit.
4. **A course to follow.** The [Certified Kubernetes Application Developer (CKAD) course on KodeKloud](https://learn.kodekloud.com/learn/courses/certified-kubernetes-application-developer-ckad) walks the exam in order, so you can practice the five domains without assembling the syllabus yourself. Kubernetes for Developers (LFS259) is the Linux Foundation course aligned to this exam. You do not have to buy a course if the docs and the two Killer.sh attempts are already enough.

## Before you book

The [CKA and CKAD instructions](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad) cover both exams.

- One monitor, a private room, and a government photo ID whose name matches your exam account.
- A computer where you can install the PSI secure browser. Do not use a virtual machine or a locked-down work laptop.
- The desk stays clear. The docs in the exam browser are the reference.
- Plug the laptop in.
- A pass is 66% or higher. The result comes by email after the sitting.

Then go sit it. CKA was the cluster. CKAD is the application you came here to run.

The next post is **[KCSA](/posts/kubestronaut-04-kcsa/)**, the Kubernetes and Cloud Native Security Associate exam. It is multiple choice. You learn the security ideas before CKS puts you in a lab.
