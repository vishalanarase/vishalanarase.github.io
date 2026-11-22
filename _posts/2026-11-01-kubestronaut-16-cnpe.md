---
title: "CNPE: Building the Platform"
author: vishalanarase
date: 2026-11-01 12:00:00 +0530
categories: [The Road to Golden Kubestronaut]
tags: [kubestronaut, cnpe]
render_with_liquid: false
keywords: 'cnpe, certified cloud native platform engineer, kubestronaut, platform engineering, cncf'
image: assets/images/kubestronaut/title/cnpe.jpg
---

This is the platform lab on [The Road to Golden Kubestronaut](/posts/kubestronaut-00-introduction). [CNPA](/posts/kubestronaut-15-cnpa) was the concept exam. **CNPE** asks you to build the platform.

The Certified Cloud Native Platform Engineer exam is performance-based. It has been required for Golden Kubestronaut since 1 March 2026. For two hours you work in a Linux remote desktop, in the terminal and in web interfaces, on architecture, GitOps, self-service APIs, observability, and policy.

By now this should feel like a review of the road behind you. Treat it that way, and still practice. A review you cannot type is not a pass.

## Where this exam sits

CNPE reuses CKA, CGOA, CAPA, CBA, KCA, PCA, and OTCA. The new part is holding them in one system a developer can use. Linux is the last exam after this one. The platform still runs on an operating system, and LFCS is that exam. Finish the platform lab first, while the GitOps and policy work is fresh.

## Exam snapshot

Domains match on the [CNCF CNPE page](https://www.cncf.io/training/certification/cnpe/) and the [Linux Foundation CNPE page](https://training.linuxfoundation.org/certification/certified-cloud-native-platform-engineer-cnpe/).

| | |
| --- | --- |
| Exam | Certified Cloud Native Platform Engineer (CNPE) |
| Format | Online, proctored, performance-based |
| Duration | 2 hours |
| Validity | 2 years |
| Prerequisite | None. CNPA is the right preparation. It is not required. |
| Price | $445 for the exam, including one retake |
| Time to schedule | 12 months from purchase |
| Included | Two Killer.sh simulator attempts. Each attempt stays open for 36 hours. |

## Syllabus

### GitOps and Continuous Delivery, 25%

Tied for the largest domain.

- GitOps for applications and for infrastructure.
- CI/CD that lands on Kubernetes.
- Progressive delivery. Blue/green and canary. You did the idea in CKAD, ICA, and CAPA. Here you wire it.

### Platform APIs and Self-Service, 25%

The other large domain.

- CRDs that represent a platform service.
- A workflow a developer can start without filing a ticket.
- Operators that reconcile those requests.
- Automation around that self-service path.

### Observability and Operations, 20%

- Monitoring, alerting, logging, and tracing, in one place.
- Metrics that say whether the platform is getting better.
- An incident. Find it and fix it. CKA troubleshooting still applies.

### Platform Architecture and Infrastructure, 15%

- Networking, storage, and compute choices that a platform has to make on purpose.
- Cost, right-sizing, and scaling.
- Multi-tenancy. One cluster, many teams, and the limits that keep them apart.

### Security and Policy Enforcement, 15%

- Service-to-service communication that is authenticated.
- RBAC across the platform.
- Audit trails, SBOMs, and compliance reports.
- Policy engines and admission controllers. Kyverno belongs here.
- Scanning in the pipeline, before the deploy.

## How to prepare

1. Read the competencies on the [Linux Foundation CNPE page](https://training.linuxfoundation.org/certification/certified-cloud-native-platform-engineer-cnpe/).
2. Build a small platform path on a cluster. A Git repository, Argo CD syncing it, a Kyverno policy rejecting privileged pods, and one dashboard that shows the app is up.
3. Add a CRD that means "please create an environment" and a controller, even a tiny one, that acts on it.
4. Use both Killer.sh attempts. The docs allowed during the exam are the Kubernetes docs and blog, plus task-specific links in the exam. The list is on the [resources page](https://docs.linuxfoundation.org/tc-docs/certification/certification-resources-allowed).

## Resources

1. **The official curriculum.** [CNCF CNPE page](https://www.cncf.io/training/certification/cnpe/) and the [Linux Foundation CNPE page](https://training.linuxfoundation.org/certification/certified-cloud-native-platform-engineer-cnpe/). The PDF is [CNPE_Curriculum.pdf](https://github.com/cncf/curriculum/blob/master/CNPE_Curriculum.pdf) in [cncf/curriculum](https://github.com/cncf/curriculum).
2. **The docs for the lab.** [Kubernetes documentation](https://kubernetes.io/docs/) for the tasks, and the project docs you already used for Argo, Kyverno, and Prometheus.
3. **The simulator.** Two Killer.sh attempts, included with registration.

## Before you book

One monitor, a private room, a matching photo ID, a clear desk, and a computer that can install the secure browser. Not a virtual machine. Plug the laptop in. You have two hours.

The next post is **[LFCS](/posts/kubestronaut-17-lfcs)**, the Linux Foundation Certified System Administrator exam. The operating system under every cluster you have touched on this road.
