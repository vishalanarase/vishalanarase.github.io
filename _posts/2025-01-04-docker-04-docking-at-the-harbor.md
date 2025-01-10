---
title: Docker Registry - Docking at the Harbor 🐳
author: vishalanarase
date: 2025-01-04 18:00:00 +0530
categories: [Docking the Ship Navigating Docker for Developers]
tags: [docker]
render_with_liquid: false
image: assets/images/docker/title/docker-registry.webp
---

Welcome Back to "Docking the Ship: Navigating Docker for Developers"! In the journey of mastering Docker, understanding Docker registries is a crucial milestone. Think of Docker registries as the harbors where your Docker images are securely stored, efficiently shared, and effectively managed.  

In this post, we’ll dive into the essentials of working with Docker registries. From exploring Docker Hub, the default registry for most developers, to setting up your own private registries, you’ll learn how to manage and distribute your container images seamlessly.  

Let’s get started and dock your images at the right harbor! 🚢

---

## **What is a Docker Registry?**

A Docker registry is a storage and distribution system for Docker images. It allows developers to:

- **Store** images securely.
- **Share** images with others.
- **Pull** images for deployment in different environments.

### Popular Docker Registries

- **Docker Hub**: The default and most popular public registry.
- **Amazon Elastic Container Registry (ECR)**: A private registry service by AWS.
- **Google Container Registry (GCR)**: Google Cloud’s container registry.
- **Self-Hosted Registries**: Custom registries hosted on-premises or in the cloud.

---

## **Core Concepts**

![](assets/images/docker/docker-registry-concepts.png)

### 1. **Pushing Images to a Registry**

#### Prequisites (Docker Hub or Private Registry Account)

- Docker installed on your machine.
- For Docker Hub: Create an account at [Docker Hub](https://hub.docker.com/).
- For private registries (e.g., self-hosted or cloud-based): Ensure the registry is set up and accessible.

Pushing an image involves uploading it to a registry so it can be shared or deployed. Here’s how:

- You can clone the repository from [here](https://github.com/vishalanarase/navigating-docker):

    ```bash
    # Clone the repository
    git clone https://github.com/vishalanarase/navigating-docker

    # Change to the project directory
    cd navigating-docker

    # Checkout to the specific version
    git checkout v1.0.0
    ```

- We will be using docker hub for this example. I have accouunt with username `vishalanarase`.

1. **Build the Image**
    ```bash
    docker build -t <local-image> .
    ```
    Example:
    ```bash
    docker build -t navigating-docker .
    ```
    <details>
    <summary>
    Output
    </summary>
    ```bash
    [+] Building 6.3s (12/12) FINISHED                                                                                                       docker:desktop-linux
     => [internal] load build definition from Dockerfile                                                                                                     0.0s
     => => transferring dockerfile: 515B                                                                                                                     0.0s
     => [internal] load metadata for docker.io/library/golang:1.23-alpine                                                                                    2.0s
     => [auth] library/golang:pull token for registry-1.docker.io                                                                                            0.0s
     => [internal] load .dockerignore                                                                                                                        0.0s
     => => transferring context: 2B                                                                                                                          0.0s
     => [1/6] FROM docker.io/library/golang:1.23-alpine@sha256:6c5c9590f169f77c8046e45c611d3b28fe477789acd8d3762d23d4744de69812                              0.0s
     => [internal] load build context                                                                                                                        0.0s
     => => transferring context: 14.03kB                                                                                                                     0.0s
     => CACHED [2/6] WORKDIR /app                                                                                                                            0.0s
     => CACHED [3/6] COPY go.mod go.sum ./                                                                                                                   0.0s
     => CACHED [4/6] RUN go mod download                                                                                                                     0.0s
     => [5/6] COPY . .                                                                                                                                       0.0s
     => [6/6] RUN go build -o webserver .                                                                                                                    4.1s
     => exporting to image                                                                                                                                   0.2s
     => => exporting layers                                                                                                                                  0.2s
     => => writing image sha256:4e87789a83fc40d5144f6d5454acdc19cfb72a7072dece5497c0542c11222b85                                                             0.0s
     => => naming to docker.io/library/navigating-docker                                                                                                     0.0s

    View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/rh1rnssr1hpyuv7w8pbq0pjlw

    What's next:
        View a summary of image vulnerabilities and recommendations → docker scout quickview
    ```

    </details>

    ```bash
   docker images
   REPOSITORY                        TAG       IMAGE ID       CREATED          SIZE
   navigating-docker                 latest    4e87789a83fc   5 minutes ago    324MB
    ```
1. **Tag the Image**
   ```bash
   docker tag <local-image> <registry>/<repository>:<tag>
   ```
   Example:
   ```bash
   docker tag navigating-docker vishalanarase/navigating-docker:latest
   ```
   ```bash
   docker images
   REPOSITORY                        TAG       IMAGE ID       CREATED          SIZE
   navigating-docker                 latest    4e87789a83fc   25 minutes ago   324MB
   vishalanarase/navigating-docker   latest    4e87789a83fc   25 minutes ago   324MB
   ```
2. **Log in to the Registry**
   ```bash
   docker login
   ```

   - Provide your credentials (username and password for Docker Hub or token for private registries).

3. **Push the Image**
   ```bash
   docker push <registry>/<repository>:<tag>
   ```
   Example:
   ```bash
   docker push vishalanarase/navigating-docker:latest
   ```
   Output:
   ```bash
   The push refers to repository [docker.io/vishalanarase/navigating-docker]
   e17732a9ad81: Pushed
   3483483b5065: Pushed
   26cfdac52a38: Pushed
   af41e374bf82: Pushed
   e18e5638e423: Pushed
   847f00bb03e1: Mounted from library/golang
   a724eb87f2bc: Mounted from library/golang
   9215e8e3ab8c: Mounted from library/golang
   977340364f39: Mounted from library/alpine
   latest: digest: sha256:902c518d2dd4d89e565ca96c9d20cc09faeca663486c2bb6683e3befad37fc06 size: 2404
   ```

### 2. **Pulling Images from a Registry**

Now, First I will delete the local image and pull from the Docker Hub registry.
```bash
docker image rm vishalanarase/navigating-docker
Untagged: vishalanarase/navigating-docker:latest
Untagged: vishalanarase/navigating-docker@sha256:902c518d2dd4d89e565ca96c9d20cc09faeca663486c2bb6683e3befad37fc06
Deleted: sha256:4e87789a83fc40d5144f6d5454acdc19cfb72a7072dece5497c0542c11222b85
```

Pulling an image downloads it from a registry to your local environment:
```bash
docker pull <registry>/<repository>:<tag>
```
Example:
```bash
docker pull vishalanarase/navigating-docker:latest
```
Output:
```bash
latest: Pulling from vishalanarase/navigating-docker
cb8611c9fe51: Already exists
bd9615f269cd: Already exists
39f8f326b044: Already exists
859344cb3f85: Already exists
4f4fb700ef54: Already exists
2a927826cb18: Already exists
bd2ff5f12cf5: Already exists
9bcbe2868212: Already exists
d2a3aa0d6430: Already exists
eac0338a0e1d: Already exists
Digest: sha256:902c518d2dd4d89e565ca96c9d20cc09faeca663486c2bb6683e3befad37fc06
Status: Downloaded newer image for vishalanarase/navigating-docker:latest
docker.io/vishalanarase/navigating-docker:latest

What's next:
    View a summary of image vulnerabilities and recommendations → docker scout quickview vishalanarase/navigating-docker:latest
```

### 3. **Tagging Images**

Tags are essential for versioning and organizing your images. Use meaningful tags like `v1.0`, `latest`, or `production`.

```bash
docker tag <image-id> <registry>/<repository>:<tag>
```

Now, let's tag the `navigating-docker` image with the tag `v1.0.0`.
I already did `git checkout v1.0.0` in the repo so that it will build the v1.0.0 version of the image.

```bash
docker tag 4e87789a83fc vishalanarase/navigating-docker:v1.0.0
```

List the images

```bash
docker images
REPOSITORY                        TAG       IMAGE ID       CREATED          SIZE
vishalanarase/navigating-docker   latest    4e87789a83fc   36 minutes ago   324MB
vishalanarase/navigating-docker   v1.0.0    4e87789a83fc   36 minutes ago   324MB
```

Now, let's push the `v1.0.0` tagged image to Docker Hub.

```bash
docker push vishalanarase/navigating-docker:v1.0.0
```
Output:
```bash
The push refers to repository [docker.io/vishalanarase/navigating-docker]
e17732a9ad81: Layer already exists
3483483b5065: Layer already exists
26cfdac52a38: Layer already exists
af41e374bf82: Layer already exists
e18e5638e423: Layer already exists
5f70bf18a086: Layer already exists
847f00bb03e1: Layer already exists
a724eb87f2bc: Layer already exists
9215e8e3ab8c: Layer already exists
977340364f39: Layer already exists
v1.0.0: digest: sha256:902c518d2dd4d89e565ca96c9d20cc09faeca663486c2bb6683e3befad37fc06 size: 2404
```

---

## **Setting Up a Private Docker Registry**

For organizations requiring more control and security, a private registry is a great option. Here’s how to set one up:

1. **Run the Registry Container**
   ```bash
   docker run -d -p 5000:5000 --name registry registry:2
   ```
   ```bash
   docker run -d -p 5000:5000 --name registry registry:2
   Unable to find image 'registry:2' locally
   2: Pulling from library/registry
   0dfcae9cb3f0: Pull complete
   cfe29ef241d9: Pull complete
   d2787542bdb4: Pull complete
   4b69fee0ac89: Pull complete
   bbb2de197705: Pull complete
   Digest: sha256:543dade69668e02e5768d7ea2b0aa4fae6aa7384c9a5a8dbecc2be5136079ddb
   Status: Downloaded newer image for registry:2
   f12cd4a2fea38bcb7dcb8365c7c9db3cd9c9f5c3ba20b6680c30085c94f3e70d
   ```

2. **Push Images to the Private Registry**
   Tag the image to point to your private registry:
   ```bash
   docker tag my-app:latest localhost:5000/my-app:latest
   ```
   ```bash
   docker tag 4e87789a83fc localhost:5000/navigating-docker:latest
   ```
   Push the image:
   ```bash
   docker push localhost:5000/navigating-docker:latest
   ```

3. **Pull Images from the Private Registry**
   ```bash
   docker pull localhost:5000/navigating-docker:latest
   ```

### How to Use Your Own Registry

   Follow the [blog](https://www.docker.com/blog/how-to-use-your-own-registry-2/) from offical docker page

---

## **Best Practices for Working with Registries**

- **Use Secure Connections**: Always use HTTPS for private registries.
- **Leverage Image Tagging**: Use tags to manage versions and environments (e.g., `dev`, `staging`, `prod`).
- **Clean Up Unused Images**: Regularly remove old or unused images to save storage.
- **Enable Authentication**: Protect your registry with authentication and authorization mechanisms.
- **Automate with CI/CD**: Integrate image pushes and pulls into your CI/CD pipelines.

---

## **Key Takeaways**

1. **Understanding Docker Registries**:  
   - Docker registries are essential for storing and sharing Docker images.  
   - They act as central hubs where developers can push, pull, and manage container images.  

2. **Working with Docker Hub**:  
   - Docker Hub is the default registry for Docker and provides public and private repositories.  
   - You learned how to push and pull images to/from Docker Hub and the importance of tagging images correctly.  

3. **Creating Custom Docker Registries**:  
   - Setting up a private Docker registry is straightforward using Docker's `registry` image.  
   - Custom registries provide greater control over access, security, and image storage.  

4. **Tagging and Managing Images**:  
   - Proper tagging helps in versioning and organizing images efficiently.  

In the next post, we’ll explore [Docker Containers: The Heart of Navigation](https://vishalanarase.github.io/posts/docker-05-the-heart-of-navigation/). Stay tuned! 🚀

---

**Have questions or feedback?** Drop them in the comments or connect with me. Let’s navigate the Docker seas together! 🌊

