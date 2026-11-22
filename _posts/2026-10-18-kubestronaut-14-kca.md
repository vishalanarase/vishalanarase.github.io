---
title: "KCA: Policy the Cluster Enforces"
author: vishalanarase
date: 2026-10-18 12:00:00 +0530
categories: [The Road to Golden Kubestronaut]
tags: [kubestronaut, kca]
render_with_liquid: false
keywords: 'kca, kyverno certified associate, kyverno, kubestronaut, cncf'
image: assets/images/kubestronaut/title/kca.jpg
---

This is the policy exam on [The Road to Golden Kubestronaut](/posts/kubestronaut-00-introduction). [CBA](/posts/kubestronaut-13-cba) was the developer door. **KCA** is Kyverno, policy written as Kubernetes resources.

The Kyverno Certified Associate exam is multiple choice. You learn how to install Kyverno, how to write a policy, and how the cluster admits or rejects a request because of it.

## Where this exam sits

[CKS](/posts/kubestronaut-05-cks) asked you to harden a cluster by hand. Kyverno keeps that hardening in place after you close the laptop. A policy can require non-root containers, block a latest tag, or add a default label. CNPA, next, is the platform those policies belong to. Writing policies is 32% of this exam. Start there.

## Exam snapshot

Domains are from the [open curriculum](https://github.com/cncf/curriculum/tree/master/kca). Multiple-choice rules are in the [FAQ](https://docs.linuxfoundation.org/tc-docs/certification/faq-mc). Confirm the price on the [Linux Foundation KCA page](https://training.linuxfoundation.org/certification/kyverno-certified-associate-kca/).

| | |
| --- | --- |
| Exam | Kyverno Certified Associate (KCA) |
| Format | Online, proctored, multiple choice |
| Duration | 90 minutes |
| Pass mark | 75% |
| Validity | 2 years |
| Prerequisite | None |

## Syllabus

### Writing Policies, 32%

The heart of the exam. A policy matches a resource and validates, mutates, or generates something. Validation rejects a bad request. Mutation fixes it on the way in. Generation creates a related object, such as a default NetworkPolicy for a new namespace. You should be able to read a policy and say which of the three it is.

### Fundamentals of Kyverno, 18%

Kyverno is a Kubernetes-native policy engine. Policies are custom resources. The admission webhook sits in the request path. Know what happens between `kubectl apply` and the object landing in etcd.

### Installation, Configuration, and Upgrades, 18%

Install it, point it at the namespaces you care about, and upgrade it without dropping the webhook. A webhook that fails closed will stop every deploy. A webhook that fails open will stop protecting you. Know which mode you chose.

### Kyverno CLI, 12%

Test a policy against a manifest before you install it. The CLI is how you catch a broken policy on your laptop.

### Applying Policies, 10%

Cluster-wide versus namespaced. How a policy selects resources. What an exception looks like when one team has a real reason to step outside the rule.

### Policy Management, 10%

Reports, background scans, and how you know which resources already violate a policy you just wrote. Writing the policy is half. Seeing the cluster against it is the other half.

## How to prepare

1. Install Kyverno on a kind cluster. The [installation guide](https://kyverno.io/docs/installation/) is short.
2. Write one validate policy that rejects privileged containers, and one mutate policy that adds a label.
3. Test both with the CLI, then apply a bad Pod and a good Pod.
4. Book the exam when you can read a policy you did not write and name the match, the action, and the result.

## Resources

1. **The official curriculum.** [cncf/curriculum kca](https://github.com/cncf/curriculum/tree/master/kca) and the [Linux Foundation KCA page](https://training.linuxfoundation.org/certification/kyverno-certified-associate-kca/).
2. **The project docs.** [Kyverno documentation](https://kyverno.io/docs/), especially policy types and the CLI.
3. **Registration.** The exam attempt and one retake. Confirm the current price on the exam page.

## Before you book

One monitor, a private room, a matching photo ID, a clear desk. A pass is 75% or higher.

The next post is **[CNPA](/posts/kubestronaut-15-cnpa)**, the platform engineering associate exam. Multiple choice. What a platform is for, before the hands-on platform exam asks you to build one.
