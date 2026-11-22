---
title: "CGOA: Git as the Source of Truth"
author: vishalanarase
date: 2026-09-27 12:00:00 +0530
categories: [The Road to Golden Kubestronaut]
tags: [kubestronaut, cgoa]
render_with_liquid: false
keywords: 'cgoa, certified gitops associate, gitops, kubestronaut, cncf'
image: assets/images/kubestronaut/title/cgoa.jpg
---

This is the delivery exam on [The Road to Golden Kubestronaut](/posts/kubestronaut-00-introduction). [ICA](/posts/kubestronaut-10-ica) was the mesh. **CGOA** is how you ship without clicking apply.

The curriculum and the Linux Foundation call this the Certified GitOps Associate exam. You will also see GitOps Certified Associate. The abbreviation is CGOA. It is multiple choice. You learn the principles before the next exam names the Argo tools.

## Where this exam sits

GitOps means the desired state lives in Git, and a controller reconciles the cluster until it matches. That is different from a pipeline that runs `kubectl apply` and hopes. CAPA, next, is Argo CD, Workflows, Rollouts, and Events. CGOA is the idea those tools implement. CNPE will ask you to build the workflow for real.

## Exam snapshot

Domains are an even four-way split in the [open curriculum](https://github.com/cncf/curriculum/tree/master/cgoa). Multiple-choice rules are in the [FAQ](https://docs.linuxfoundation.org/tc-docs/certification/faq-mc). Confirm the price on the [Linux Foundation CGOA page](https://training.linuxfoundation.org/certification/certified-gitops-associate-cgoa/).

| | |
| --- | --- |
| Exam | Certified GitOps Associate (CGOA) |
| Format | Online, proctored, multiple choice |
| Duration | 90 minutes |
| Pass mark | 75% |
| Validity | 2 years |
| Prerequisite | None |

## Syllabus

### GitOps Fundamentals, 25%

Desired state, declared in Git. A pull-based reconciliation loop. Drift is what happens when someone changes the cluster and Git disagrees. The controller's job is to make the cluster match Git, not the other way around.

### GitOps Principles and Practices, 25%

The OpenGitOps principles: declarative, versioned and immutable, pulled automatically, continuously reconciled. How this sits next to CI. CI builds and tests. GitOps delivers. You can have both. They are not the same step.

### GitOps Tooling and Implementation, 25%

You do not have to memorize one product's screens. You do need the shape: a repository, a controller, a target cluster, and a way to promote from one environment to the next. Argo CD is the tool the next exam covers. Here, know what any such controller must do.

### GitOps Security and Observability, 25%

Who can merge to the branch that deploys to production. Signed commits. Secrets that do not live in plain Git. How you notice a reconcile failed. A green pipeline and a drifted cluster can both be true. The exam wants you to see the difference.

## How to prepare

1. Read the [OpenGitOps principles](https://opengitops.dev/).
2. Put a Deployment in Git and let a controller apply it. Change the cluster by hand. Watch the controller put it back.
3. Explain CI versus GitOps out loud, in four sentences.
4. Book the exam when drift and reconcile are ordinary words.

## Resources

1. **The official curriculum.** [cncf/curriculum cgoa](https://github.com/cncf/curriculum/tree/master/cgoa) and the [Linux Foundation CGOA page](https://training.linuxfoundation.org/certification/certified-gitops-associate-cgoa/).
2. **The principles.** [OpenGitOps](https://opengitops.dev/).
3. **Registration.** The exam attempt and one retake. Confirm the current price on the exam page.

## Before you book

One monitor, a private room, a matching photo ID, a clear desk. A pass is 75% or higher.

The next post is **[CAPA](/posts/kubestronaut-12-capa)**, the Argo project. CGOA was the idea. CAPA is the tools people mean when they say GitOps out loud.
