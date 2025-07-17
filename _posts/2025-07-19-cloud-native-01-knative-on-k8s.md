---
title: Building Serverless Functions on Kubernetes using Knative
author: vishalanarase
date: 2025-07-19 10:00:00 +0530
categories: [Cloud Native]
tags: [cloud-native, knative, kubernetes, serverless]
render_with_liquid: false
image: assets/images/cloud-native/title/knative-on-k8s.png
---

The evolution of cloud computing has taken us from bare metal to virtual machines, to containers, and now serverless. As organizations adopt Kubernetes for container orchestration, there's growing interest in combining it with Function-as-a-Service (FaaS) to maximize developer productivity and operational efficiency. But can Kubernetes truly be serverless? With [Knative](https://knative.dev/), the answer is yes.

Knative is a Kubernetes-based platform designed to build, deploy and manage modern serverless workloads with ease. It brings serverless capabilities like scale-to-zero, on-demand scaling and advanced traffic routing directly to Kubernetes, enabling teams to run event-driven, stateless applications seamlessly on their clusters.

In this blog post, we’ll explore how Knative brings serverless capabilities to Kubernetes, how it compares to other FaaS platforms and how to build and deploy your first serverless function on Kubernetes.
