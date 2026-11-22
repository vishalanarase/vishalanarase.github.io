---
title: "CBA: A Front Door for Developers"
author: vishalanarase
date: 2026-10-11 12:00:00 +0530
categories: [The Road to Golden Kubestronaut]
tags: [kubestronaut, cba]
render_with_liquid: false
keywords: 'cba, certified backstage associate, backstage, kubestronaut, cncf'
image: assets/images/kubestronaut/title/cba.jpg
---

This is the portal exam on [The Road to Golden Kubestronaut](/posts/kubestronaut-00-introduction). [CAPA](/posts/kubestronaut-12-capa) was the Argo engines. **CBA** is Backstage, the catalog and the front door developers actually open.

The Certified Backstage Associate exam is multiple choice. You learn how a Backstage app is built, configured, filled with a catalog, and extended with plugins.

## Where this exam sits

A platform that only exists as YAML never gets used. Backstage is one way to put a software catalog, templates, and docs in front of engineers. Kyverno, next, is policy. The platform exams after that assume you know both the door and the rules.

## Exam snapshot

Domains and competencies are from the [Linux Foundation CBA page](https://training.linuxfoundation.org/certification/certified-backstage-associate-cba/) and the [curriculum PDF](https://github.com/cncf/curriculum/blob/master/CBA_Curriculum.pdf). Multiple-choice rules are in the [FAQ](https://docs.linuxfoundation.org/tc-docs/certification/faq-mc).

| | |
| --- | --- |
| Exam | Certified Backstage Associate (CBA) |
| Format | Online, proctored, multiple choice |
| Duration | 90 minutes |
| Pass mark | 75% |
| Validity | 2 years |
| Prerequisite | None |
| Price | $250 for the exam, including one retake |
| Time to schedule | 12 months from purchase |

Customizing Backstage is a third of the exam. It is the chapter to start early, because it assumes you can read a little React and TypeScript.

## Syllabus

### Customizing Backstage, 32%

- Frontend plugins and backend plugins, and which side a change belongs on.
- Changing plugin behavior.
- A small change to React in the Backstage app.
- Material UI components, the look of those pages.

### Development Workflow, 24%

- Build and run a Backstage project locally.
- The local development loop.
- Compile TypeScript.
- Install dependencies with npm or Yarn.
- Build a container image of the app with Docker.

### Backstage Infrastructure, 22%

- The framework itself.
- Configuration.
- Deploying Backstage for real use.
- The client and server split. The browser is not the whole system.

### Backstage Catalog, 22%

- Why a catalog exists. Software, owners, and dependencies in one place.
- How entities get in: annotations, locations you register by hand, and automated ingestion.
- What to check when an entity never appears.

## How to prepare

1. Create a Backstage app and run it locally once. The [getting started guide](https://backstage.io/docs/getting-started/) is enough.
2. Register one component in the catalog with a `catalog-info.yaml` file.
3. Find one plugin and say whether it is frontend, backend, or both.
4. Book the exam when the catalog page and the local dev loop are familiar.

## Resources

1. **The official curriculum.** [CBA curriculum PDF](https://github.com/cncf/curriculum/blob/master/CBA_Curriculum.pdf) and the [Linux Foundation CBA page](https://training.linuxfoundation.org/certification/certified-backstage-associate-cba/).
2. **The project docs.** [Backstage docs](https://backstage.io/docs/overview/what-is-backstage).
3. **Registration.** The exam attempt, one retake, and 12 months to schedule. There is no hands-on simulator.

## Before you book

One monitor, a private room, a matching photo ID, a clear desk. A pass is 75% or higher.

The next post is **[KCA](/posts/kubestronaut-14-kca)**, the Kyverno exam. Policy written as Kubernetes resources, so the cluster enforces what CKS taught you to want.
