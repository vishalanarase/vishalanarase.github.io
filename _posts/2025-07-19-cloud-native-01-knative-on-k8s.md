---
title: Building Serverless Functions on Kubernetes using Knative
author: vishalanarase
date: 2025-07-19 10:00:00 +0530
categories: [Cloud Native]
tags: [cloud-native, knative, kubernetes, serverless]
render_with_liquid: false
keywords: 'AWS Lambda to Kubernetes migration, Lambda to Kubernetes, Serverless Kubernetes, Knative migration, FaaS on Kubernetes, Kubernetes serverless, EKS Lambda migration, Cloud-native serverless, Event-driven Kubernetes, Serverless challenges Kubernetes, Knative vs Lambda, OpenFaaS alternative, Fission alternative,Kubeless alternative'
image: assets/images/cloud-native/title/knative-on-k8s.png
---

## Introduction

The evolution of cloud computing has taken us from bare metal to virtual machines, to containers, and now **serverless**. As organizations adopt Kubernetes for container orchestration, there's growing interest in combining it with Function-as-a-Service (FaaS) to maximize developer productivity and operational efficiency. But can Kubernetes truly be serverless? With [Knative](https://knative.dev/), the answer is yes.

Knative is a Kubernetes-based platform designed to build, deploy, and manage modern serverless workloads with ease. It brings serverless capabilities like scale-to-zero, on-demand scaling, and advanced traffic routing directly to Kubernetes, enabling teams to run event-driven, stateless applications seamlessly on their clusters.

In this blog post, we'll explore how Knative brings serverless capabilities to Kubernetes, how it compares to other FaaS platforms, and how to build and deploy your first serverless function on Kubernetes.

## What is Function-as-a-Service (FaaS)

FaaS is a cloud computing model where developers write functions that are executed in response to events. Unlike traditional applications, FaaS abstracts infrastructure management, letting you focus purely on code. The serverless architecture became popular with one of the most popular services in AWS, i.e., Lambda. With the evolution of microservices and event-driven architectures, it made sense to reduce the size of execution code and package it in a small unit of compute. Moreover, with the advancements of cloud and Kubernetes, the autoscaling combined with serverless architectures makes the whole piece cost-effective.

### Key Characteristics

- Code runs in ephemeral containers
- Event-driven execution
- Automatic scaling (including to zero)
- Pay-per-use model in public clouds

### How FaaS differs from traditional microservices

| Feature    | FaaS                         | Traditional Microservices       |
| :--------- | :--------------------------- | :------------------------------ |
| Scaling    | Auto-scales (including to 0) | Manual or HPA-based scaling     |
| Runtime    | Short-lived, stateless       | Long-running, stateful possible |
| Deployment | Event-triggered              | Always-on                       |

FaaS is ideal for short-lived tasks like image processing, webhook handlers, or scheduled jobs, whereas microservices handle long-lived business processes.

### Why Kubernetes needs a serverless layer

While Kubernetes is an incredibly powerful and flexible orchestration platform, it was not originally designed with Function-as-a-Service (FaaS) or serverless workloads in mind. By default, Kubernetes requires users to define deployments, manage YAML manifests, and configure scaling policies. All of which can slow down development and add operational overhead. Introducing a serverless layer on top of Kubernetes addresses these gaps and unlocks new capabilities that make it even more appealing for modern, cloud-native applications.

- One major benefit is **developer velocity**. Serverless abstracts away much of the boilerplate YAML, deployment configurations, and infrastructure management that come with Kubernetes.
- Another key advantage is **cost efficiency**. Traditional Kubernetes clusters often keep resources allocated even when they are idle, leading to unnecessary costs. A serverless layer enables scale-to-zero, where workloads can scale down completely when not in use, freeing up cluster resources and reducing cloud bills significantly.
- Finally, Kubernetes with a serverless layer aligns well with **event-driven architecture (EDA)**. Serverless encourages building loosely coupled, decoupled systems that react to events, such as HTTP requests, messages from queues, or cloud events. This design pattern makes applications more resilient, modular, and easier to evolve.
- This combination of Kubernetes and serverless fits particularly well in architectures where **microservices and event-driven patterns** dominate. Use cases like APIs that experience sporadic traffic, background job processing, IoT event streams, and real-time data pipelines all benefit from the elastic, on-demand nature of serverless. It's especially suited for cloud-native organizations adopting distributed, decoupled architectures that demand both flexibility and efficiency.

### Challenges of FaaS on Kubernetes

#### Cold Starts

One of the key challenges of running Function-as-a-Service (FaaS) on Kubernetes is dealing with cold starts. In a serverless environment, the platform often scales your application to zero when idle. When a new request arrives, Kubernetes has to spin up a pod from scratch, introducing noticeable latency before the function becomes ready to serve traffic. This delay, known as a cold start, can significantly impact user experience for latency-sensitive applications.

#### Autoscaling

Traditional Kubernetes autoscaling, such as the Horizontal Pod Autoscaler (HPA), isn't designed to handle the dynamic nature of FaaS workloads. HPA relies on metrics like CPU or memory utilization, and it struggles with scaling down to zero or reacting quickly to sudden burst traffic. To address this, Knative introduced its own **Knative Pod Autoscaler (KPA)**, which scales based on request volume and supports scale-to-zero, making it better suited for serverless patterns.

#### Traffic Routing

Routing traffic effectively during updates or deployments is another challenge. FaaS often requires advanced traffic management to enable strategies like blue/green deployments, canary rollouts, or gradual traffic shifting between versions. Native Kubernetes services and ingresses are limited in this regard, so additional components and configurations are necessary to achieve the smooth traffic control expected in serverless environments.

#### Stateful vs Stateless Concerns

Serverless platforms like Knative are primarily designed for stateless functions, where each invocation is independent of previous ones. However, many real-world applications require maintaining state across requests. To support such scenarios, developers need to implement external state management or use patterns like event-driven workflows, for example, with **Knative Eventing**.

#### Operational Overhead

Finally, adopting FaaS on Kubernetes introduces operational complexity. Running and maintaining the necessary components, such as Istio or Kourier for networking, along with monitoring and scaling logic, increases the operational burden on platform teams. While serverless abstracts away much of the infrastructure from the developer's perspective, it still requires careful configuration and observability to ensure reliable and efficient operation at scale.

## Enter Knative - Serverless Kubernetes

### Core components of Knative

![](assets/images/cloud-native/knative/core-components.png)

#### Serving

Knative Serving is the part of Knative that turns your Kubernetes cluster into a **serverless platform for stateless HTTP apps**, using Kubernetes CRDs to control the lifecycle and behavior of your workloads.

![](assets/images/cloud-native/knative/serving.png)

The primary Knative Serving resources are Services, Routes, Configurations, and Revisions.

- Services

The `Service` resource (`service.serving.knative.dev`) manages the full lifecycle of your serverless app, coordinating `Routes`, `Configurations`, and `Revisions` behind the scenes. It ensures your app always has a route and creates a new revision with every update. You can direct traffic to the latest revision or pin it to a specific one for controlled rollouts.

- Routes

The `Route` resource (`route.serving.knative.dev`) defines how traffic is directed to one or more revisions. It enables strategies like canary deployments, A/B testing, and traffic splitting between app versions, making gradual rollouts easy and flexible.

- Configurations

The `Configuration` (`configuration.serving.knative.dev`) resource defines your deployment's desired state, separating code from configuration as per Twelve-Factor principles.

- Revisions

A `Revision` resource (`revision.serving.knative.dev`) is an immutable snapshot of your app's code and config at a point in time. Revisions scale dynamically with traffic, even to zero when idle, and can be retained to roll back to previous versions when needed.

#### Eventing

Knative Eventing is an event-driven application platform for Kubernetes that enables building reactive, distributed systems using a set of APIs to connect event producers (sources) with event consumers (sinks).

![](assets/images/cloud-native/knative/eventing.png)

Event-driven platform on Kubernetes, connecting event producers (sources) with event consumers (sinks) through a flexible routing layer.

- Event Ingress

Knative Eventing brings events into the system from internal or external sources. Key components like `ApiServerSource`, `PingSource`, `KafkaSource`, or `GoogleCloudPubSubSource` generate CloudEvents from Kubernetes changes, schedules, or external messaging systems, enabling your cluster to react to diverse asynchronous signals.

- Event Routing

It is the core of Knative Eventing, directing events to the right destinations using `Brokers` and `Triggers`. Brokers collect all events at an addressable endpoint, while Triggers filter and route them to consumers based on attributes, enabling flexible, decoupled subscriptions without altering producers.

- Event Egress

Delivers routed events to sinks, which are HTTP endpoints like Knative Services, Kubernetes Services, Deployments, or external APIs. **Sinks** process the events and execute the business logic, keeping producers and consumers independent while ensuring reliable delivery and response.

<YoutubeEmbed id='DrmOpjAunlQ' title='credits: knative' />

#### Functions

Knative Functions offers a streamlined programming model for running serverless functions on Kubernetes using Knative, designed to remove the complexity of dealing directly with Kubernetes, Knative resources, container images, or Dockerfiles. With Knative Functions, developers can focus purely on writing stateless, event-driven business logic while the platform handles the underlying details.

Using the `func` CLI, you can easily create, build, and deploy your function as a Knative Service with minimal effort. When you build or execute a function, the system automatically generates an Open Container Initiative (OCI) compliant container image, stores it in a container registry, and keeps it up to date each time you make changes to your code. This allows for a seamless developer experience while adhering to container standards. Developers can work with Knative Functions either through the standalone `func` CLI or via the `kn func` plugin integrated into the Knative CLI, enabling flexible workflows and efficient management of function lifecycles.

### Comparing Knative with Other Serverless Solutions

Knative brings serverless capabilities to its own Kubernetes clusters. Whether on-premises or in the cloud. It's worth noting that many organizations start their serverless journey with managed platforms like **AWS Lambda**, **Azure Functions**, or **Google Cloud Run Functions**. These solutions are fully managed and excellent for quickly building event-driven applications without managing infrastructure.

However, they come with trade-offs, particularly around vendor lock-in, less control over the underlying environment, and cost/concurrency challenges as workloads scale.

Knative, on the other hand, provides similar serverless patterns, scale-to-zero, event-driven workflows. While running on your own Kubernetes infrastructure.

For teams already invested in Kubernetes or those seeking greater flexibility and cloud neutrality, Knative can be an attractive alternative. It allows you to bring serverless applications closer to where your applications and data already live.

### Setting Up Knative on Cluster

To set up Knative on your Kubernetes cluster, you'll need a few prerequisites.

- **A Kubernetes Cluster** – You can use any Kubernetes cluster (Minikube, GKE, EKS, etc.) Here, we'll use **Kind** for local development.
- **kubectl** – Installed and configured to interact with your cluster.
- **Knative CLI (kn)** – For managing Knative resources.
- **A Networking Layer** – Knative requires a networking layer (Istio, Contour, or Kourier). We'll use **Kourier** for simplicity.

#### Install Knative using quickstart

Follow the guide [here](https://knative.dev/docs/install/quickstart-install/), and you will get the required things up and running.

- Create kind cluster using knative quickstart plugin

```bash
❯ kn quickstart kind
```

- Get cluster

```bash
❯ kind get clusters
knative
```

- Kind cluster

```bash
❯ k cluster-info
Kubernetes control plane is running at https://127.0.0.1:56608
CoreDNS is running at https://127.0.0.1:56608/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

- kn CLI version

```bash
❯ kn version
Version:      v1.18.0
Build Date:   2025-04-23 19:42:52
Git Revision: 96721e59
Supported APIs:
* Serving
  - serving.knative.dev/v1 (knative-serving v1.18.0)
* Eventing
  - sources.knative.dev/v1 (knative-eventing v1.18.0)
  - eventing.knative.dev/v1 (knative-eventing v1.18.0)
```

- Verifying the knative-serving setup

```bash
❯ kubectl get pods -n knative-serving
NAME                                      READY   STATUS    RESTARTS   AGE
activator-76dfb9f767-nhv2h                1/1     Running   0          2d16h
autoscaler-77f5c4668d-d96bb               1/1     Running   0          2d16h
controller-79f47f6984-h5g7v               1/1     Running   0          2d16h
net-kourier-controller-666c5cf64b-26q6r   1/1     Running   0          2d16h
webhook-c8f58f6f7-ffxl4                   1/1     Running   0          2d16h
```

### Deploying First Function

To deploy your first function on Knative, you can use the [`func`](https://github.com/knative/func) CLI, which simplifies building and deploying serverless functions without worrying about containers or YAML. For example, you can quickly scaffold and write a sample function in Go, build it, and deploy it as a Knative Service.

Alternatively, you can define your function directly with a **Knative Service YAML**, giving you full control over configuration and deployment. Once deployed, you can observe Knative's autoscaling in action.

The function scales down to zero when idle and automatically scales back up when requests arrive, demonstrating true serverless behavior.

#### Create a New Function

```bash
❯ func create -l go -t http knative-hello
Created go function in /Users/vishal/knative/knative-hello
❯ tree knative-hello
knative-hello
├── func.yaml
├── go.mod
├── handle_test.go
├── handle.go
└── README.md

0 directories, 5 files
```

#### Deploy the Function using func CLI

```bash
❯ func deploy --registry docker.io/vishalanarase --builder=pack
Building function image
Still building
Still building
Yes, still building
🙌 Function built: index.docker.io/vishalanarase/knative-hello:latest
Pushing function image to the registry "index.docker.io" using the "vishalanarase" user credentials
🎯 Creating Triggers on the cluster
✅ Function deployed in namespace "default" and exposed at URL:
   http://knative-hello.default.127.0.0.1.sslip.io
```

- Knative Service YAML (Option for func deploy)

```yaml
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: hello
spec:
  template:
    spec:
      containers:
        - image: docker.io/yourrepo/hello
```

#### List Knative service/function

```bash
❯ kubectl get ksvc
NAME            URL                                               LATESTCREATED         LATESTREADY           READY   REASON
knative-hello   http://knative-hello.default.127.0.0.1.sslip.io   knative-hello-00001   knative-hello-00001   True
```

#### Invoke the function

```bash
❯ curl http://knative-hello.default.127.0.0.1.sslip.io
"GET / HTTP/1.1\r\nHost: knative-hello.default.127.0.0.1.sslip.io\r\nAccept: */*\r\nForwarded: for=10.244.0.10;proto=http\r\nK-Proxy-Request: activator\r\nUser-Agent: curl/8.7.1\r\nX-Forwarded-For: 10.244.0.10, 10.244.0.5\r\nX-Forwarded-Proto: http\r\nX-Request-Id: 23a9b1b7-5793-4dc2-92ea-954111643d07\r\n\r\n"
```

#### Autoscaling behavior demo

- Check Initial State (Scaled to Zero)

```bash
❯ k get po
NAME                                       READY   STATUS      RESTARTS      AGE
cert-manager-bd75fc784-f72tm               1/1     Running     3 (57m ago)   7h20m
cert-manager-cainjector-6b666b746c-sb2pg   1/1     Running     0             7h20m
cert-manager-startupapicheck-wktx4         0/1     Completed   1             7h20m
cert-manager-webhook-8468858c77-bkqp4      1/1     Running     0             7h20m
cluster-api-operator-554cc6f994-chkv2      1/1     Running     0             7h18m
```

- Triggering autoscaling by sending traffic using [`hey`](https://github.com/rakyll/hey)

```bash
❯ hey -z 30s -c 50 http://knative-hello.default.127.0.0.1.sslip.io
Summary:
  Total:    30.0566 secs
  Slowest:    14.5674 secs
  Fastest:    0.0020 secs
  Average:    0.4136 secs
  Requests/sec:    120.7722

  Total data:    1259610 bytes
  Size/request:    347 bytes

Response time histogram:
  0.002 [1]    |
  1.459 [3578] |■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  2.915 [1]    |
  4.372 [0]    |
  5.828 [0]    |
  7.285 [0]    |
  8.741 [0]    |
  10.198 [0]   |
  11.654 [0]   |
  13.111 [0]   |
  14.567 [50]  |■


Latency distribution:
  10% in 0.0342 secs
  25% in 0.0714 secs
  50% in 0.1502 secs
  75% in 0.3239 secs
  90% in 0.5606 secs
  95% in 0.7317 secs
  99% in 13.9150 secs

Details (average, fastest, slowest):
  DNS+dialup:    0.0123 secs, 0.0020 secs, 14.5674 secs
  DNS-lookup:    0.0123 secs, 0.0000 secs, 0.8900 secs
  req write:    0.0000 secs, 0.0000 secs, 0.0011 secs
  resp wait:    0.4012 secs, 0.0019 secs, 13.6756 secs
  resp read:    0.0001 secs, 0.0000 secs, 0.0027 secs

Status code distribution:
  [200]    3630 responses
```

- Observe Pods Scaling Up

```bash
❯ k get po
NAME                                              READY   STATUS      RESTARTS       AGE
knative-hello-00003-deployment-7d754c97f4-4kj9k   2/2     Running     0              74s
knative-hello-00003-deployment-7d754c97f4-4r5jc   2/2     Running     0              76s
knative-hello-00003-deployment-7d754c97f4-54whm   2/2     Running     0              74s
knative-hello-00003-deployment-7d754c97f4-5srql   2/2     Running     0              76s
knative-hello-00003-deployment-7d754c97f4-67lcz   2/2     Running     0              76s
knative-hello-00003-deployment-7d754c97f4-6b7g5   2/2     Running     0              74s
knative-hello-00003-deployment-7d754c97f4-6rvqz   2/2     Running     0              76s
knative-hello-00003-deployment-7d754c97f4-7bnv4   2/2     Running     0              75s
knative-hello-00003-deployment-7d754c97f4-7d8wx   2/2     Running     0              70s
knative-hello-00003-deployment-7d754c97f4-7md6k   2/2     Running     0              75s
knative-hello-00003-deployment-7d754c97f4-7zxq8   2/2     Running     0              75s
knative-hello-00003-deployment-7d754c97f4-85pff   2/2     Running     0              76s
knative-hello-00003-deployment-7d754c97f4-954h6   2/2     Running     0              75s
```

- Check Knative Metrics

```bash
❯ kubectl get podautoscaler
NAME                  DESIREDSCALE   ACTUALSCALE   READY   REASON
knative-hello-00003   70             55            True
```

- Stop traffic and wait Pods Scaled Back to Zero

```bash
❯ k get po
NAME                                             READY   STATUS        RESTARTS      AGE
cert-manager-bd75fc784-f72tm                     1/1     Running       3 (62m ago)   7h25m
cert-manager-cainjector-6b666b746c-sb2pg         1/1     Running       0             7h25m
cert-manager-startupapicheck-wktx4               0/1     Completed     1             7h25m
cert-manager-webhook-8468858c77-bkqp4            1/1     Running       0             7h25m
cluster-api-operator-554cc6f994-chkv2            1/1     Running       0             7h23m
```

- Check Knative Metrics to verify it scale back to zero

```bash
❯ kubectl get podautoscaler
NAME                  DESIREDSCALE   ACTUALSCALE   READY   REASON
knative-hello-00003   0              0             False   NoTraffic
```

#### Advanced Features

- Updated the function and list Revisions

```bash
❯ kn revisions list
NAME                  SERVICE         TRAFFIC   TAGS   GENERATION   AGE   CONDITIONS   READY   REASON
knative-hello-00003   knative-hello   100%             3            22h   3 OK / 4     True
knative-hello-00002   knative-hello                    2            22h   3 OK / 4     True
knative-hello-00001   knative-hello                    1            22h   3 OK / 4     True
```

- Traffic splitting and canary deployments

```bash
❯ kn service update knative-hello --traffic knative-hello-00001=40 --traffic knative-hello-00002=30 --traffic knative-hello-00003=30
Updating Service 'knative-hello' in namespace 'default':

  0.067s The Route is still working to reflect the latest desired specification.
  0.168s Ingress has not yet been reconciled.
  0.222s Waiting for load balancer to be ready
  0.386s Ready to serve.

Service 'knative-hello' with latest revision 'knative-hello-00003' (unchanged) is available at URL:
http://knative-hello.default.127.0.0.1.sslip.io
```

```bash
❯ kn revisions list
NAME                  SERVICE         TRAFFIC   TAGS   GENERATION   AGE   CONDITIONS   READY   REASON
knative-hello-00003   knative-hello   30%              3            22h   3 OK / 4     True
knative-hello-00002   knative-hello   30%              2            22h   3 OK / 4     True
knative-hello-00001   knative-hello   40%              1            22h   3 OK / 4     True
```

#### Monitoring with Prometheus + Grafana

- Install the Prometheus Stack by using Helm:

```bash
❯ helm install prometheus prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --create-namespace
NAME: prometheus
LAST DEPLOYED: Tue Jul  8 18:16:14 2025
NAMESPACE: monitoring
STATUS: deployed
REVISION: 1
```

- Apply the ServiceMonitors and PodMonitors to collect metrics from Knative.

```bash
❯ kubectl apply -f https://raw.githubusercontent.com/knative-extensions/monitoring/main/servicemonitor.yaml
servicemonitor.monitoring.coreos.com/controller created
servicemonitor.monitoring.coreos.com/autoscaler created
servicemonitor.monitoring.coreos.com/activator created
servicemonitor.monitoring.coreos.com/webhook created
servicemonitor.monitoring.coreos.com/broker-filter created
servicemonitor.monitoring.coreos.com/broker-ingress created
podmonitor.monitoring.coreos.com/eventing-controller created
podmonitor.monitoring.coreos.com/imc-controller created
podmonitor.monitoring.coreos.com/ping-source created
podmonitor.monitoring.coreos.com/apiserver-source created
```

- Import Grafana dashboards

```bash
❯ kubectl apply -f https://raw.githubusercontent.com/knative-extensions/monitoring/main/grafana/dashboards.yaml
configmap/knative-eventing-dashboards created
configmap/knative-serving-dashboards created
```

- Access the Grafana instance locally

By default, the Grafana instance is only exposed on a private service named prometheus-grafana.

Enter the command:

```bash
kubectl port-forward -n monitoring svc/prometheus-grafana 3000:80
```

Access the dashboards in your browser via [http://localhost:3000]

Use the default credentials to login:

```bash
username: admin
password: prom-operator
```

![](assets/images/cloud-native/knative/knative-cp.png)

This diagram visualizes the Knative Serving control plane's efficiency.

![](assets/images/cloud-native/knative/knative-scaling.png)

Scaling up pods in response to incoming traffic, ensuring optimal performance and cost efficiency on Kubernetes.

![](assets/images/cloud-native/knative/knative-request.png)

This image illustrates how Knative Serving routes HTTP requests to specific revisions of a service.

## When to choose Knative-based FaaS

- You're Already Using Kubernetes
  - If your infrastructure runs on Kubernetes, Knative integrates seamlessly without needing external cloud services.
  - Avoid vendor lock-in while keeping the benefits of serverless (scale-to-zero, event-driven execution).
- Need Fine-Grained Control Over Scaling
  - Knative's autoscaling (KPA) allows custom scaling based on Concurrency (requests-per-pod), RPS (Requests Per Second), CPU/Memory metrics
  - Unlike some managed FaaS (e.g., AWS Lambda), you can tweak scaling behavior precisely.
- Hybrid or On-Prem Deployments
  - Works in air-gapped environments, private clouds, or edge setups where public cloud FaaS isn't an option.
- Event-Driven Workloads with Custom Triggers
  - Knative Eventing lets you route events from Kafka, RabbitMQ, CloudEvents, Kubernetes-native sources (e.g., ConfigMap changes,) and Custom event producers
- CI/CD Pipelines with GitOps
  - Deploy functions via kubectl or func CLI alongside other K8s resources.
  - Works with ArgoCD, Flux, or Tekton for GitOps workflows.

## Conclusion

Knative has paved the way for serverless workloads on Kubernetes, combining scalability, event-driven patterns, and advanced deployment strategies into a seamless developer experience. As cloud-native technologies evolve, we can expect even more tailored platforms and patterns for emerging use cases like edge computing and AI.

There are alternatives, options like [**Kubeless**](https://github.com/vmware-archive/kubeless) (it's not actively maintained), [**OpenFaaS**](https://www.openfaas.com/), and [**Fission**](https://fission.io/) also bring serverless capabilities to Kubernetes, each with unique strengths and trade-offs. Knative is just one of many tools shaping the future of serverless on Kubernetes.

