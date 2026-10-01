---
title: "CKS: Hardening the Cluster"
author: vishalanarase
date: 2026-08-16 12:00:00 +0530
categories: [The Road to Golden Kubestronaut]
tags: [kubestronaut, cks]
render_with_liquid: false
keywords: 'cks, certified kubernetes security specialist, kubestronaut, kubernetes, cncf'
---

This is the fifth exam on [The Road to Golden Kubestronaut](/posts/kubestronaut-00-introduction/). [KCSA](/posts/kubestronaut-04-kcsa/) gave you the security ideas. **CKS** asks you to apply them.

The Certified Kubernetes Security Specialist exam is hands-on. For two hours you harden a cluster, a host, a workload, and the path that built the image. Pass this, with the four exams before it still current, and you are a Kubestronaut.

## Where this exam sits

CKS is the security lab. You must have passed [CKA](/posts/kubestronaut-02-cka/) before you can sit it. You can purchase CKS earlier. You schedule it after CKA is done. The [CNCF CKS page](https://www.cncf.io/training/certification/cks/) says CKA does not have to still be active on exam day. Confirm that on the FAQ the week you book, and keep CKA unexpired anyway if you want the Kubestronaut title. Every certification has to be current for that title, even when one exam lets you sit the next with an older pass.

Earning or recertifying CKS on or after 18 June 2026 extends your CKA to the same expiration date, under the CARE program. Passing CKS also renews KCSA.

## Exam snapshot

Figures below are from the [CNCF CKS page](https://www.cncf.io/training/certification/cks/), the [Linux Foundation exam page](https://training.linuxfoundation.org/certification/certified-kubernetes-security-specialist/), and the [CKA, CKAD, and CKS FAQ](https://docs.linuxfoundation.org/tc-docs/certification/faq-cka-ckad-cks). Check them again the week you book.

The Linux Foundation page and the CNCF page currently disagree on two weights. Linux Foundation lists Cluster Setup at 15% and System Hardening at 10%. CNCF still lists Cluster Setup at 10% and System Hardening at 15%. The competencies are the same. Use the Linux Foundation page when you decide where to spend your hours.

| | |
| --- | --- |
| Exam | Certified Kubernetes Security Specialist (CKS) |
| Format | Online, proctored, performance-based. You solve tasks on a command line. |
| Duration | 2 hours |
| Pass mark | 67% |
| Kubernetes version | v1.35 |
| Validity | 2 years |
| Prerequisite | You have passed CKA. It does not have to be unexpired to schedule CKS. |
| Price | $445 for the exam, including one retake |
| Time to schedule | 12 months from purchase |
| Included | Two Killer.sh simulator attempts. Each attempt stays open for 36 hours. The simulation has 20 to 25 questions, the same set each time. |

Your score arrives by email, usually within 24 hours.

## Syllabus

Six domains. Three of them are 20% each. Those are the ones that decide the result.

### Minimize Microservice Vulnerabilities, 20%

The workload itself.

- Pod Security Standards. A restricted Pod does not run as root, and it does not get capabilities it does not need.
- Secrets. Create them, mount them, and keep them out of the image.
- Isolation. Namespaces for tenancy, and sandboxed containers when a process needs a harder boundary.
- Pod-to-pod encryption with Cilium or Istio. The [Cilium docs](https://docs.cilium.io/) and the [Istio docs](https://istio.io/latest/docs/) are allowed in the exam browser.

### Supply Chain Security, 20%

The path from source to the node.

- A small base image. Less in the image means less to patch.
- The supply chain itself. An SBOM, the CI system, and the artifact repository.
- A registry you trust, and signatures you verify before the cluster runs the image.
- Static analysis of a workload or an image, with tools such as Kubesec and KubeLinter.

### Monitoring, Logging and Runtime Security, 20%

What the cluster is doing after the deploy.

- Behavioral detection, including Falco. The [Falco docs](https://falco.org/docs/) are allowed during the exam.
- Threats across the host, the application, the network, and the workload.
- An audit log that shows who called the API. You can read it and say what happened.
- Immutable containers at runtime. A container that can rewrite itself is a container you did not mean to ship.

### Cluster Hardening, 15%

Who is allowed to ask the API for anything.

- RBAC that grants the smallest permission that still lets the workload run.
- ServiceAccounts. Disable the default where you can, and keep new ones narrow.
- Restricted access to the Kubernetes API.
- Upgrades. An old cluster is an open list of known vulnerabilities.

### Cluster Setup, 15% on the Linux Foundation page

How the cluster is assembled before any application arrives.

- Network policies that restrict access at the cluster level.
- A CIS benchmark review of etcd, the kubelet, kube-dns, and the API server.
- Ingress with TLS.
- Node metadata and cloud endpoints kept away from workloads.
- Platform binaries verified before you deploy them. The [bom documentation](https://kubernetes-sigs.github.io/bom/cli-reference/) is allowed in the exam browser.

### System Hardening, 10% on the Linux Foundation page

The host under the kubelet.

- A smaller operating system footprint.
- Least privilege for identities on that host.
- Less reach from the network into the node.
- Kernel tools. AppArmor and seccomp profiles that limit what a container can call.

## How to prepare

KCSA was the vocabulary. CKS is the same list, typed into a cluster.

1. Read the competencies on the [Linux Foundation CKS page](https://training.linuxfoundation.org/certification/certified-kubernetes-security-specialist/).
2. On a cluster, tighten one thing at a time. A NetworkPolicy, a restricted Pod Security level, an RBAC binding that is too wide, a seccomp profile, an Ingress TLS secret.
3. Practice from the docs you are allowed to open. Besides the Kubernetes docs and blog, CKS allows Falco, bom, etcd, the NGINX Ingress Controller, Cilium, and Istio. The full list is on the [resources page](https://docs.linuxfoundation.org/tc-docs/certification/certification-resources-allowed). Learn those sites well enough to find a page in under a minute.
4. Use both Killer.sh attempts. The question set does not change between them, so the second attempt is where you finish inside the time.

The pass mark is 67%, one point higher than CKA and CKAD. Treat the three 20% domains as the exam.

## Resources

Use them in this order.

1. **The official curriculum.** [CNCF CKS page](https://www.cncf.io/training/certification/cks/) and the [Linux Foundation CKS page](https://training.linuxfoundation.org/certification/certified-kubernetes-security-specialist/). The open curriculum is in [cncf/curriculum](https://github.com/cncf/curriculum/tree/master/cks). If the weights disagree, follow the Linux Foundation page.
2. **The docs you are allowed to read during the exam.** [Kubernetes documentation](https://kubernetes.io/docs/), the [Kubernetes blog](https://kubernetes.io/blog/), [Falco](https://falco.org/docs/), [bom](https://kubernetes-sigs.github.io/bom/cli-reference/), [etcd](https://etcd.io/docs/), the [NGINX Ingress Controller](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/), [Cilium](https://docs.cilium.io/), and [Istio](https://istio.io/latest/docs/).
3. **The simulator that comes with the exam.** Two Killer.sh attempts, described on the Linux Foundation CKS page. Use both.
4. **A course to follow.** The [Certified Kubernetes Security Specialist (CKS) course on KodeKloud](https://learn.kodekloud.com/learn/courses/certified-kubernetes-security-specialist-cks) walks the exam in order, so you can practice the six domains without assembling the syllabus yourself. Kubernetes Security Essentials (LFS260) can also be added to the exam from the Linux Foundation page. You do not have to buy a course if the docs and the two Killer.sh attempts are already enough.

## Before you book

The performance exams share the same room rules as CKA.

- One monitor, a private room, and a government photo ID whose name matches your exam account.
- A computer where you can install the PSI secure browser. A virtual machine and a locked-down work laptop are both poor choices.
- The desk stays clear. The allowed docs are the reference.
- Plug the laptop in.
- A pass is 67% or higher. The result comes by email after the sitting.

Then go sit it. This is the exam that turns the first four into Kubestronaut.

The next post is a pause. **Becoming a Kubestronaut** is not another exam. It is the evening you notice what you just finished, before the Golden half of the road.
