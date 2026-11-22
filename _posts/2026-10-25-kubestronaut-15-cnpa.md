---
title: "CNPA: What a Platform Is For"
author: vishalanarase
date: 2026-10-25 12:00:00 +0530
categories: [The Road to Golden Kubestronaut]
tags: [kubestronaut, cnpa]
render_with_liquid: false
keywords: 'cnpa, cloud native platform engineering associate, kubestronaut, platform engineering, cncf'
image: assets/images/kubestronaut/title/cnpa.jpg
---

This is the platform concept exam on [The Road to Golden Kubestronaut](/posts/kubestronaut-00-introduction). [KCA](/posts/kubestronaut-14-kca) was policy. **CNPA** is why a platform exists, and how the pieces you already studied fit on one.

The Certified Cloud Native Platform Engineering Associate exam is multiple choice. It has been required for Golden Kubestronaut since 15 October 2025. It is also the long multiple-choice exam: two hours, not ninety minutes.

## Where this exam sits

You have already met the parts. GitOps and Argo deliver. Backstage is a portal. Kyverno governs. Prometheus and OpenTelemetry show what happened. Cilium and Istio connect and secure. CNPA asks you to talk about them as a platform a developer can use without opening a ticket. CNPE, next, is the same map as a lab.

## Exam snapshot

Domains are from the [Linux Foundation CNPA page](https://training.linuxfoundation.org/certification/certified-cloud-native-platform-engineering-associate-cnpa/) and match the [open curriculum](https://github.com/cncf/curriculum/tree/master/cnpa). The [multiple-choice FAQ](https://docs.linuxfoundation.org/tc-docs/certification/faq-mc) gives CNPA two hours. Other multiple-choice exams get ninety minutes.

| | |
| --- | --- |
| Exam | Certified Cloud Native Platform Engineering Associate (CNPA) |
| Format | Online, proctored, multiple choice |
| Duration | 120 minutes |
| Pass mark | 75% |
| Validity | 2 years |
| Prerequisite | None |
| Price | $250 for the exam, including one retake |
| Time to schedule | 12 months from purchase |

## Syllabus

### Platform Engineering Core Fundamentals, 36%

More than a third of the exam. Do not rush it.

- Declarative resource management.
- DevOps practices as they show up on a platform team.
- Environments and infrastructure concepts.
- What a platform is, and what it is not. A platform is a product for developers. A pile of clusters is not yet a platform.
- Continuous integration, continuous delivery, and GitOps. You studied the GitOps half in CGOA. Here it sits inside the platform story.

### Platform Observability, Security, and Conformance, 20%

- Traces, metrics, logs, and events. PCA and OTCA pay off here.
- Secure service communication. CCA and ICA pay off here.
- Policy engines. KCA pays off here.
- Kubernetes security essentials, and security in the pipeline. KCSA and CKS pay off here.

### Continuous Delivery and Platform Engineering, 16%

- What a CI pipeline is responsible for.
- Incident response when the platform itself is the thing that broke.
- How CI and CD relate, and how GitOps carries the application environment.

### Platform APIs and Provisioning Infrastructure, 12%

- The Kubernetes reconciliation loop.
- CRDs as the self-service API.
- Operators that provision something when a developer asks.
- Infrastructure that shows up because a custom resource was created.

### IDPs and Developer Experience, 8%

- A catalog and a portal. CBA was this, from the inside.
- An API a developer can call without knowing which cluster they landed on.
- Where AI-assisted automation shows up, as a helper, not as the platform.

### Measuring Your Platform, 8%

- Whether the platform made the team faster.
- DORA metrics: deployment frequency, lead time, change failure rate, and time to restore. Know what each one measures. A platform that cannot show these numbers cannot tell if it helped.

## How to prepare

1. Read the [CNCF Platforms white paper](https://tag-app-delivery.cncf.io/whitepapers/platforms/). It is the essay this exam is built around.
2. Walk the domains and write one tool you already know next to each. Prometheus for metrics. Argo CD for GitOps. Backstage for the portal. Kyverno for policy.
3. Be able to define an internal developer platform in a few sentences a developer would recognize.
4. Book the exam. You have two hours, and the core domain is more than a third of the score.

## Resources

1. **The official curriculum.** [Linux Foundation CNPA page](https://training.linuxfoundation.org/certification/certified-cloud-native-platform-engineering-associate-cnpa/) and [cncf/curriculum cnpa](https://github.com/cncf/curriculum/tree/master/cnpa).
2. **The essay.** [CNCF Platforms white paper](https://tag-app-delivery.cncf.io/whitepapers/platforms/).
3. **Registration.** $250, one retake, 12 months to schedule. No hands-on simulator.

## Before you book

One monitor, a private room, a matching photo ID, a clear desk. A pass is 75% or higher. This sitting is two hours. Plug the laptop in.

The next post is **[CNPE](/posts/kubestronaut-16-cnpe)**, the hands-on platform exam. Save it until the concepts in this post feel like a review.
