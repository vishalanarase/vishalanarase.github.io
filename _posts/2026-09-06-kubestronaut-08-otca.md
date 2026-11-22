---
title: "OTCA: Traces, Metrics, and Logs"
author: vishalanarase
date: 2026-09-06 12:00:00 +0530
categories: [The Road to Golden Kubestronaut]
tags: [kubestronaut, otca]
render_with_liquid: false
keywords: 'otca, opentelemetry certified associate, kubestronaut, opentelemetry, cncf'
image: assets/images/kubestronaut/title/otca.jpg
---

This is the next exam on [The Road to Golden Kubestronaut](/posts/kubestronaut-00-introduction). [PCA](/posts/kubestronaut-07-pca) taught you Prometheus. **OTCA** is the wider story: traces, metrics, and logs, collected once and sent where they need to go.

The OpenTelemetry Certified Associate exam is multiple choice. You learn the project, not one vendor's backend.

## Where this exam sits

Prometheus answers "what is the number now." A trace answers "which hop was slow." A log answers "what did this process say." OpenTelemetry is the instrumentation and the pipeline in front of those backends. After this exam, Cilium and Istio will talk about observability again. You will already know what they are exporting.

## Exam snapshot

Domains are from the [open curriculum](https://github.com/cncf/curriculum/tree/master/otca). Duration and pass mark are the [multiple-choice rules](https://docs.linuxfoundation.org/tc-docs/certification/faq-mc). Confirm the price on the [Linux Foundation OTCA page](https://training.linuxfoundation.org/certification/opentelemetry-certified-associate-otca/).

| | |
| --- | --- |
| Exam | OpenTelemetry Certified Associate (OTCA) |
| Format | Online, proctored, multiple choice |
| Duration | 90 minutes |
| Pass mark | 75% |
| Validity | 2 years |
| Prerequisite | None |
| Included | No hands-on simulator. |

The five domains are an even 20% each. Do not skip logs because metrics felt familiar from PCA.

## Syllabus

### OpenTelemetry Fundamentals, 20%

Signals, context, and why one project replaced a pile of agents. A resource describes the service. A signal is metrics, traces, or logs. Context is how a trace stays one trace as it crosses a network.

### Instrumentation and SDKs, 20%

Automatic instrumentation hooks a language runtime. Manual instrumentation is code you write when the automatic span is not the span you meant. The API is stable. The SDK implements it. Know which one you depend on.

### Metrics, 20%

OTLP metrics and how they relate to Prometheus. Instruments: counter, gauge, histogram. Views and aggregation decide what is actually exported. This is the chapter that should feel like PCA, with new names.

### Traces, 20%

A trace is a tree of spans. Each span has a name, a start, an end, attributes, and a parent. Context propagation is how the parent survives HTTP and gRPC. Sampling decides which traces you keep. Without propagation, every service starts a new story.

### Logs and Exporters, 20%

Logs can join a trace when they carry the trace id. Exporters send signals to a backend. The Collector receives, processes, and exports. Processors batch, filter, and add attributes. You should be able to describe a pipeline: receiver, processor, exporter.

## How to prepare

1. Read the [OpenTelemetry docs overview](https://opentelemetry.io/docs/what-is-opentelemetry/).
2. Sketch one request through two services. Mark the spans, the place metrics are recorded, and the log line that should carry the trace id.
3. Read one Collector config and name the receiver, the processor, and the exporter.
4. Book the exam when those three pictures are boring.

## Resources

1. **The official curriculum.** [cncf/curriculum otca](https://github.com/cncf/curriculum/tree/master/otca) and the [Linux Foundation OTCA page](https://training.linuxfoundation.org/certification/opentelemetry-certified-associate-otca/).
2. **The project docs.** [OpenTelemetry documentation](https://opentelemetry.io/docs/), especially concepts, instrumentation, and the [Collector](https://opentelemetry.io/docs/collector/).
3. **Registration.** The exam attempt and one retake. Confirm the current price on the exam page.

## Before you book

Same room as the other multiple-choice exams. One monitor, a private room, a matching government photo ID, a clear desk, and no documentation open beside you. A pass is 75% or higher.

The next post is **[CCA](/posts/kubestronaut-09-cca)**, the Cilium Certified Associate exam. You can see the cluster now. Next you learn how the packets move.
