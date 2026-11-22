---
title: "PCA: Seeing the Cluster"
author: vishalanarase
date: 2026-08-30 12:00:00 +0530
categories: [The Road to Golden Kubestronaut]
tags: [kubestronaut, pca]
render_with_liquid: false
keywords: 'pca, prometheus certified associate, kubestronaut, prometheus, cncf'
image: assets/images/kubestronaut/title/pca.jpg
---

This is the first exam of the Golden half on [The Road to Golden Kubestronaut](/posts/kubestronaut-00-introduction). [Kubestronaut](/posts/kubestronaut-06-kubestronaut) is behind you. **PCA** teaches you to see what the cluster is doing.

The Prometheus Certified Associate exam is multiple choice. You learn the architecture, the data model, and PromQL well enough to ask a cluster a precise question.

## Where this exam sits

Kubernetes told you that something was running. Prometheus tells you how it is behaving. The next exam, OTCA, widens that into traces, metrics, and logs. PCA is the metrics chapter, and it is worth knowing before the rest of the observability story.

## Exam snapshot

Domains below are from the [open curriculum](https://github.com/cncf/curriculum/tree/master/pca). Format, duration, and pass mark for multiple-choice exams are from the [Linux Foundation FAQ](https://docs.linuxfoundation.org/tc-docs/certification/faq-mc). Confirm the price and the domains on the [Linux Foundation PCA page](https://training.linuxfoundation.org/certification/prometheus-certified-associate-pca/) the week you book.

| | |
| --- | --- |
| Exam | Prometheus Certified Associate (PCA) |
| Format | Online, proctored, multiple choice |
| Duration | 90 minutes |
| Pass mark | 75% |
| Validity | 2 years |
| Prerequisite | None |
| Included | An exam preparation handbook when the product lists one. There is no hands-on simulator. |

You may not open external sites during the exam. Learn PromQL before you sit down.

## Syllabus

PromQL is the largest domain. Give it the largest share of your time.

### PromQL, 30%

The language. Select a series, filter by label, rate a counter, aggregate, and join two series. Know the difference between a counter and a gauge before you write the query. A counter only goes up. A gauge moves both ways. `rate` belongs on counters.

### Fundamentals and Concepts, 20%

What a metric is for. Why you scrape instead of push, and when Pushgateway exists as the exception. Labels are the index. A high-cardinality label will hurt you later, so know why people warn you about it.

### Prometheus Architecture, 20%

Prometheus scrapes targets, stores samples locally, and evaluates rules. Service discovery finds the targets. Exporters turn systems that do not speak Prometheus into targets that do. Alertmanager receives alerts and routes them. You should be able to draw that path from target to inbox.

### Instrumentation, 10%

Client libraries expose metrics from your own process. The four types you will meet are counter, gauge, histogram, and summary. Histograms are how you answer "how slow was this" without storing every latency.

### Alerting and Recording Rules, 10%

A recording rule saves an expensive query. An alerting rule turns a query into an alert. Write the alert on a symptom a human can act on. Alertmanager groups, inhibits, and sends.

### Ecosystem and Integrations, 10%

Grafana reads Prometheus. Kubernetes service discovery finds pods. Remote write sends samples somewhere else when local disk is not the whole story. You do not need every integration. You need to know which neighbor does which job.

## How to prepare

1. Read the domains on the [curriculum page](https://github.com/cncf/curriculum/tree/master/pca).
2. Run Prometheus, scrape one target, and answer five questions in PromQL: current value, rate, a label filter, a sum by label, and a histogram quantile.
3. Write one alerting rule and watch it fire on purpose.
4. Book the exam when you can explain a counter, a scrape, and an alert without looking them up.

The [Prometheus documentation](https://prometheus.io/docs/introduction/overview/) is the text. The query examples in those docs are the practice.

## Resources

1. **The official curriculum.** [cncf/curriculum pca](https://github.com/cncf/curriculum/tree/master/pca) and the [Linux Foundation PCA page](https://training.linuxfoundation.org/certification/prometheus-certified-associate-pca/).
2. **The project docs.** [Prometheus docs](https://prometheus.io/docs/introduction/overview/), especially [PromQL](https://prometheus.io/docs/prometheus/latest/querying/basics/) and [alerting](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/).
3. **Registration.** The exam attempt and one retake. Confirm the current price on the exam page. PCA has no performance simulator.

## Before you book

- One monitor, a private room, and a government photo ID whose name matches the exam account.
- A computer where you can install the PSI secure browser.
- A clear desk. No notes, and no docs on a second screen.
- A pass is 75% or higher. The result comes by email after the sitting.

The next post is **[OTCA](/posts/kubestronaut-08-otca)**, the OpenTelemetry Certified Associate exam. Prometheus was the metrics store. OpenTelemetry is how telemetry gets there, and everywhere else.
