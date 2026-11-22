---
title: "CAPA: The Argo Family"
author: vishalanarase
date: 2026-10-04 12:00:00 +0530
categories: [The Road to Golden Kubestronaut]
tags: [kubestronaut, capa]
render_with_liquid: false
keywords: 'capa, certified argo project associate, argo cd, kubestronaut, cncf'
image: assets/images/kubestronaut/title/capa.jpg
---

This is the tools exam on [The Road to Golden Kubestronaut](/posts/kubestronaut-00-introduction). [CGOA](/posts/kubestronaut-11-cgoa) was the GitOps idea. **CAPA** is the Argo family: Workflows, CD, Rollouts, and Events.

The Certified Argo Project Associate exam is multiple choice. You learn when to reach for which project, not how to pass a live cluster lab.

## Where this exam sits

People say "Argo" and mean four different things. This exam separates them. Backstage, next, is the door developers walk through. Argo is one of the engines behind that door.

## Exam snapshot

Five domains at 20% each, from the [open curriculum](https://github.com/cncf/curriculum/tree/master/capa). Multiple-choice rules are in the [FAQ](https://docs.linuxfoundation.org/tc-docs/certification/faq-mc). Confirm the price on the [Linux Foundation CAPA page](https://training.linuxfoundation.org/certification/certified-argo-project-associate-capa/).

| | |
| --- | --- |
| Exam | Certified Argo Project Associate (CAPA) |
| Format | Online, proctored, multiple choice |
| Duration | 90 minutes |
| Pass mark | 75% |
| Validity | 2 years |
| Prerequisite | None |

## Syllabus

### Argo Project Fundamentals, 20%

The map. Which project solves which problem, and how they share a Kubernetes-native style. Custom resources, controllers, and Git as an input.

### Argo Workflows, 20%

A workflow is a graph of steps, often containers, run to completion. Use it for jobs, pipelines, and fan-out work. It is not the thing that keeps a Deployment in sync with Git. That is Argo CD.

### Argo CD, 20%

The GitOps controller. An Application points at a repository and a cluster. Sync makes the cluster match. Health tells you whether the result is actually up. App of Apps and projects are how you scale that past one team.

### Argo Rollouts, 20%

Progressive delivery. Canary and blue-green, with analysis before you shift the rest of the traffic. A Deployment rolls fast. A Rollout can pause and measure. ICA already made you shift traffic in a mesh. Rollouts is the deployment object that can do the same job from Git.

### Argo Events, 20%

Something happened, so start something else. A webhook, a schedule, a message. Events is the trigger. Workflows is often the thing it starts. Know the boundary.

## How to prepare

1. Draw the four logos and write one sentence under each.
2. Read the getting-started page for [Argo CD](https://argo-cd.readthedocs.io/), [Workflows](https://argo-workflows.readthedocs.io/), [Rollouts](https://argo-rollouts.readthedocs.io/), and [Events](https://argoproj.github.io/argo-events/).
3. Install Argo CD, point it at a tiny Git repository, and sync once.
4. Book the exam when you can say which project you would open for a canary, a data pipeline, a webhook, and a drifted Deployment.

## Resources

1. **The official curriculum.** [cncf/curriculum capa](https://github.com/cncf/curriculum/tree/master/capa) and the [Linux Foundation CAPA page](https://training.linuxfoundation.org/certification/certified-argo-project-associate-capa/).
2. **The project docs.** [Argo CD](https://argo-cd.readthedocs.io/), [Argo Workflows](https://argo-workflows.readthedocs.io/), [Argo Rollouts](https://argo-rollouts.readthedocs.io/), and [Argo Events](https://argoproj.github.io/argo-events/).
3. **Registration.** The exam attempt and one retake. Confirm the current price on the exam page.

## Before you book

One monitor, a private room, a matching photo ID, a clear desk. A pass is 75% or higher.

The next post is **[CBA](/posts/kubestronaut-13-cba)**, the Backstage exam. A catalog and a front door for the engineers you support.
