---
title: "CKA: Running the Cluster"
author: vishalanarase
date: 2026-07-26 12:00:00 +0530
categories: [The Road to Golden Kubestronaut]
tags: [kubestronaut, cka]
render_with_liquid: false
keywords: 'cka, certified kubernetes administrator, kubestronaut, kubernetes, cncf'
---

This is the second exam on [The Road to Golden Kubestronaut](/posts/kubestronaut-00-introduction/). [KCNA](/posts/kubestronaut-01-kcna) gave you the words. **CKA** asks you to use them.

The Certified Kubernetes Administrator exam is hands-on. For two hours you are at a command line, fixing and building a real cluster. This is where confidence starts to feel earned, because you can no longer recognize the right sentence. You have to make the cluster match the task.

## Where this exam sits

CKA is the administrator's exam. You install and manage clusters, place workloads, wire networking, attach storage, and find what is broken. CKAD, later on this road, is the application side of the same cluster. CKS cannot be booked until CKA is active, so this certificate is also the key to the security exam.

Passing CKA renews KCNA under the Linux Foundation CARE program. Later, earning or recertifying CKS on or after 18 June 2026 extends your CKA to the same expiration date as that CKS. The work you do here keeps the earlier step alive.

## Exam snapshot

Figures below are from the [CNCF CKA page](https://www.cncf.io/training/certification/cka/), the [Linux Foundation exam page](https://training.linuxfoundation.org/certification/certified-kubernetes-administrator-cka/), and the [CKA, CKAD, and CKS FAQ](https://docs.linuxfoundation.org/tc-docs/certification/faq-cka-ckad-cks). Check them again the week you book. The Kubernetes version moves forward within about four to eight weeks of a new release.

| | |
| --- | --- |
| Exam | Certified Kubernetes Administrator (CKA) |
| Format | Online, proctored, performance-based. You solve tasks on a command line. |
| Tasks | 15 to 20 |
| Duration | 2 hours |
| Pass mark | 66% |
| Kubernetes version | v1.35 |
| Validity | 2 years |
| Prerequisite | None. KCNA is the right preparation. It is not required. |
| Price | $445 for the exam, including one retake |
| Time to schedule | 12 months from purchase |
| Included | Two Killer.sh simulator attempts. Each attempt stays open for 36 hours and has 17 graded questions. |

Your score arrives by email, usually within 24 hours.

## Syllabus

The [open curriculum](https://github.com/cncf/curriculum/blob/master/CKA_Curriculum_v1.35.pdf) and the exam pages use the same five domains. Troubleshooting is the largest. Practice it as its own subject, not as whatever time is left on the last evening.

### Troubleshooting, 30%

Almost a third of the score. A node that will not join, a control-plane component that is down, a Pod that will not start, a Service that has no endpoints.

- Clusters and nodes. `kubectl get nodes`, the conditions on a node, and `kubelet` when a node stays NotReady.
- Cluster components. Static Pods for the control plane, manifests under the kubelet's static pod path, and logs when the API server or scheduler is unhappy.
- Resource usage. You can inspect what a node and a container are consuming.
- Container output. `kubectl logs` is how you read why a process exited.
- Services and networking. A Service with an empty endpoints list is a label problem until you prove otherwise.

The [Kubernetes docs on troubleshooting clusters](https://kubernetes.io/docs/tasks/debug/debug-cluster/) are allowed during the exam. Learn the path to those pages before the clock starts.

### Cluster Architecture, Installation and Configuration, 25%

You build the cluster, you do not only use one someone else built.

- RBAC. A Role or ClusterRole, a RoleBinding or ClusterRoleBinding, and a subject. If you can grant a ServiceAccount permission to list Pods in one namespace, this competency is in reach.
- kubeadm. Initialize a cluster, join a node, and know which config lives where.
- A highly available control plane. More than one control-plane node, and what has to be shared for that to work.
- Helm and Kustomize. Install a cluster component from a chart or a kustomization. You are applying them, not writing a chart from scratch.
- Extension interfaces. CRI runs containers, CNI connects Pods, CSI attaches storage. Name the job of each.
- CRDs and operators. A CustomResourceDefinition extends the API. An operator acts on those objects.

### Services and Networking, 20%

Follow one request from outside the cluster to a container.

- Pod to Pod connectivity, and what a NetworkPolicy changes when you deny that path.
- Service types. ClusterIP, NodePort, and LoadBalancer, and what Endpoints are.
- Ingress and the Gateway API. An Ingress is the older object. Gateway API is in the current curriculum, and the [Gateway API docs](https://gateway-api.sigs.k8s.io/) are allowed in the exam browser for CKA.
- CoreDNS. Pods resolve Services by name because CoreDNS is running. When they cannot, you look there.

### Workloads and Scheduling, 15%

This is the part KCNA already named. CKA asks you to change it.

- Deployments. A rolling update and a rollback, on purpose.
- ConfigMaps and Secrets mounted or injected so the application can read them.
- Autoscaling a workload.
- Self-healing. Probes, a restart policy, and a Deployment that replaces a dead Pod.
- Scheduling. Requests and limits, node affinity, and the reason a Pod stays Pending.

### Storage, 10%

The smallest domain, and easy to leave until it costs you an easy task.

- A PersistentVolume and a PersistentVolumeClaim, and which one the Pod uses.
- Access modes and reclaim policy.
- A StorageClass and dynamic provisioning, so a claim creates the volume for you.

## How to prepare

Book the exam only after you can do the work without a tutorial beside you. A useful order:

1. Read the five domains on the [curriculum PDF](https://github.com/cncf/curriculum/blob/master/CKA_Curriculum_v1.35.pdf) once, so you know what "done" looks like.
2. Build a cluster with kubeadm, or with [kind](https://kind.sigs.k8s.io/), and break it. Drain a node, scale a Deployment, change a Service selector, delete a PVC, and put each one back.
3. Practice from the docs, not from memory of a blog. During the exam you may use the browser inside the virtual machine for [kubernetes.io/docs](https://kubernetes.io/docs/), [kubernetes.io/blog](https://kubernetes.io/blog/), [helm.sh/docs](https://helm.sh/docs/), and, for CKA, the Gateway API docs. Search on kubernetes.io/docs is allowed. Opening a result that leaves those sites is not. Bookmark the task pages you actually use: RBAC, NetworkPolicy, Ingress, and persistent volumes.
4. Spend both Killer.sh attempts. The first one shows you the clock. The second one is where you fix the habits that cost you tasks. Each session is 17 questions and stays up for 36 hours, so you can repeat it inside that window.

Speed matters. Fifteen to twenty tasks in two hours is a few minutes each. Do the ones you can finish, flag the rest, and come back. A perfect solution to half the paper scores less than a working solution to most of it.

On the exam hosts, `kubectl` is already aliased to `k`, with Bash completion. Practice with that alias. Copy and paste inside the exam terminal is `Ctrl+Shift+C` and `Ctrl+Shift+V`. Each task tells you which host to `ssh` to. Finish the task, `exit` back to the base system, and do not reboot that base host.

## Resources

Use them in this order.

1. **The official curriculum.** [CNCF CKA page](https://www.cncf.io/training/certification/cka/) for the domains and weights. [Linux Foundation CKA page](https://training.linuxfoundation.org/certification/certified-kubernetes-administrator-cka/) for the competencies, the Kubernetes version, and the Killer.sh attempts included with registration. [CKA curriculum v1.35](https://github.com/cncf/curriculum/blob/master/CKA_Curriculum_v1.35.pdf) in the open [cncf/curriculum](https://github.com/cncf/curriculum) repository.
2. **The docs you are allowed to read during the exam.** [Kubernetes documentation](https://kubernetes.io/docs/), the [Kubernetes blog](https://kubernetes.io/blog/), [Helm's docs](https://helm.sh/docs/), and the [Gateway API docs](https://gateway-api.sigs.k8s.io/). The allowed list is on the [Linux Foundation resources page](https://docs.linuxfoundation.org/tc-docs/certification/certification-resources-allowed).
3. **The simulator that comes with the exam.** Two Killer.sh attempts, described on the Linux Foundation CKA page. Use both before you sit.
4. **A course to follow.** The [CKA Certification Course on KodeKloud](https://learn.kodekloud.com/learn/courses/cka-certification-course-certified-kubernetes-administrator) walks the exam in order, so you can practice the five domains without assembling the syllabus yourself. Kubernetes Fundamentals (LFS258) can also be bought in a bundle with the exam from the Linux Foundation page. You do not have to buy a course if the docs and the two Killer.sh attempts are already enough.

## Before you book

The [CKA and CKAD instructions](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad) are worth reading once, all the way through.

- One monitor, a private room, and a government photo ID whose name matches your exam account.
- A computer where you can install the PSI secure browser. A virtual machine and a locked-down work laptop are both poor choices. The compatibility check can pass on a VM and the exam still will not.
- The desk stays clear. No notes and no second device. The docs in the exam browser are the reference. A personal cheatsheet is not allowed.
- Plug the laptop in. You have two hours, and a dead battery does not pause the exam.
- A pass is 66% or higher. The result comes by email after the sitting.

Then go sit it. KCNA was the front door. CKA is the first room you furnish yourself.

The next post is **[CKAD](/posts/kubestronaut-03-ckad/)**, the Certified Kubernetes Application Developer exam. CKA taught you to run the cluster. CKAD asks you to build what runs on it.
