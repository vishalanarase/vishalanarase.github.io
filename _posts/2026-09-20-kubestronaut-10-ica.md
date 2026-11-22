---
title: "ICA: The Service Mesh Lab"
author: vishalanarase
date: 2026-09-20 12:00:00 +0530
categories: [The Road to Golden Kubestronaut]
tags: [kubestronaut, ica]
render_with_liquid: false
keywords: 'ica, istio certified associate, kubestronaut, istio, service mesh, cncf'
image: assets/images/kubestronaut/title/ica.jpg
---

This is the mesh exam on [The Road to Golden Kubestronaut](/posts/kubestronaut-00-introduction). [CCA](/posts/kubestronaut-09-cca) was the network. **ICA** is Istio on top of it: traffic, security, and what to do when the mesh is wrong.

The Istio Certified Associate exam is proctored and lasts two hours. The [Linux Foundation page](https://training.linuxfoundation.org/certification/istio-certified-associate-ica/) describes it as performance-based and multiple choice. You will configure Istio, not only define it.

## Where this exam sits

A service mesh moves retry, timeout, mTLS, and authorization out of application code. CKS already pointed at Istio for pod-to-pod encryption. ICA is where you install it and make traffic do what you asked. GitOps, next, is how you stop applying those changes by hand.

The [curriculum README](https://github.com/cncf/curriculum/tree/master/ica) still lists an older domain split, with observability called out on its own. The exam page has moved on. Use the exam page.

## Exam snapshot

| | |
| --- | --- |
| Exam | Istio Certified Associate (ICA) |
| Format | Online, proctored, performance-based and multiple choice |
| Duration | 2 hours |
| Validity | 2 years |
| Prerequisite | None |
| Price | $250 for the exam, including one retake |
| Time to schedule | 12 months from purchase |
| Included | One retake. Istio Service Mesh Essentials (LFS245) can be bundled with the exam. |

Confirm the pass mark on the ICA FAQ the week you book. Do not borrow the 75% multiple-choice number or the 66% CKA number without checking. This exam is a mix.

## Syllabus

Weights from the Linux Foundation exam page.

### Traffic Management, 35%

The largest domain. This is the reason people install a mesh.

- Ingress and egress.
- Routing inside the mesh.
- Destination rules for traffic policy.
- Shifting traffic between versions.
- Connecting in-mesh workloads to services outside the mesh.
- Resilience: circuit breaking, failover, outlier detection, timeouts, retries.
- Fault injection, so you can prove the resilience works.

### Securing Workloads, 25%

- Authorization policy. Who may call whom.
- Authentication. mTLS between workloads, and JWT at the edge.
- TLS for traffic entering the mesh.

### Installation, Upgrade, and Configuration, 20%

- Install with istioctl or Helm.
- Sidecar mode and ambient mode. Know what each one puts in the path of a request.
- Customize the installation.
- Upgrade, canary or in place.

### Troubleshooting, 20%

- Bad configuration.
- The control plane. istiod has to be healthy before any route works.
- The data plane. A sidecar or a ztunnel that is not receiving the config you think you pushed.

## How to prepare

1. Install Istio on a cluster and put two versions of one service behind it.
2. Shift 10% of traffic to the new version, then all of it.
3. Turn on mTLS and write an authorization policy that allows one client and denies another.
4. Break the mesh on purpose. A bad port, a missing sidecar, a destination rule aimed at the wrong subset. Fix each one from the [Istio docs](https://istio.io/latest/docs/), because those docs are allowed during the exam, along with the [Istio blog](https://istio.io/blog/) and the Kubernetes docs. The list is on the [resources page](https://docs.linuxfoundation.org/tc-docs/certification/certification-resources-allowed).

## Resources

1. **The official exam page.** [Linux Foundation ICA](https://training.linuxfoundation.org/certification/istio-certified-associate-ica/) for the domains above. The [CNCF certification catalog](https://www.cncf.io/training/certification/) links here as well.
2. **The docs you can use in the exam.** [Istio documentation](https://istio.io/latest/docs/) and the [Istio blog](https://istio.io/blog/).
3. **The course sold with the exam.** Istio Service Mesh Essentials (LFS245), bundled on the exam page.

## Before you book

The room rules match the other proctored exams. One monitor, a private room, a matching photo ID, a clear desk, and a computer that can install the secure browser. Plug the laptop in. You have two hours.

The next post is **[CGOA](/posts/kubestronaut-11-cgoa)**, the GitOps exam. You have been applying changes by hand. Next you let Git be the source of truth.
