---
title: Docker Images - Crafting Your Vessel 🐳
author: vishalanarase
date: 2025-01-03 18:00:00 +0530
categories: [Docking the Ship Navigating Docker for Developers]
tags: [docker]
render_with_liquid: false
image: assets/images/docker/title/crafting-your-vessel.png
---

Welcome back to **"Docking the Ship: Navigating Docker for Developers"**! After installing Docker and running `docker images`, it’s time to learn about Docker images. In the containerization world, Docker images are the building blocks for containers. Just as a ship requires a blueprint to sail, your application needs a Docker image to run in a containerized environment. In this post, we’ll explore how Docker images are created, how Dockerfile works, and how you can customize your own Docker images for your applications.

## What is a Docker Image?

A Docker image is a blueprint or template for creating Docker containers. It contains everything needed to run an application: code, libraries, dependencies, and the runtime environment. Essentially, a Docker image is a snapshot of a filesystem that can be executed in a container.

Docker images are made up of layers. Each layer represents a change or addition to the image, such as installing a new package, adding files, or configuring the environment. These layers are stacked on top of each other to form the complete image.

### Why Are Docker Images Important?

Docker images are crucial because they:

- **Provide Consistency**: Docker images ensure that the application behaves the same way across different environments. Whether you’re running the image on your local machine, a staging server, or in production, Docker guarantees that the application will run consistently.
- **Enable Portability**: Docker images are portable. You can create an image once and run it anywhere—on your laptop, in the cloud, or on a remote server—without worrying about differences in the underlying environment.
- **Facilitate Versioning**: Each Docker image is tagged with a version, making it easy to roll back to a previous version or track updates over time.

### Understanding Docker Image Layers

Docker images are built from layers, and each layer represents a set of changes to the image. These layers are cached and reused, which makes Docker image builds more efficient.

For example, consider the following Dockerfile snippet for a Go web application:

```dockerfile
# Start with the official Golang image as a base
FROM golang:1.23-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy the rest of the application code
COPY . .

# Build the Go application
RUN go build -o main .
```

In this case:

1. **`FROM golang:1.23-alpine`**: This instruction pulls a base image (in this case, a Go image with Alpine Linux). This base image is the first layer of the image.
2. **`WORKDIR /app`**: This creates a new layer that sets the working directory inside the container.
3. **`COPY . .`**: This adds another layer that copies your application’s source code into the container.
4. **`RUN go build -o main .`**: This step compiles your Go application, creating a new layer with the built binary.

Each of these instructions creates a separate layer. Docker caches each layer, so if nothing changes between builds, Docker can skip the layers that have already been built, making subsequent builds faster.

#### Layer Caching

Layer caching is one of the key features of Docker that makes it efficient. Docker caches each layer after it’s built. If you modify your application code and rebuild the image, Docker will only rebuild the layers that are affected by the changes. For example, if you change the application code but not the base image or dependencies, Docker will reuse the layers for the base image and dependencies, and only rebuild the layer containing the application code.

### Simple Golang Web Application

This Go web application listens on port `8081` and responds with the message "Hello, Dockerized World!" when accessed at the root URL (`/`). It uses the `net/http` package to handle HTTP requests and starts a server with `http.ListenAndServe`. The server runs indefinitely, printing an error message if it fails to start. This simple app demonstrates how to set up a basic HTTP server in Go.

```go
package main

import (
	"fmt"
	"net/http"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintf(w, "Hello, Dockerized World!")
	})

	fmt.Println("Starting server on :8081")
	if err := http.ListenAndServe(":8081", nil); err != nil {
		fmt.Printf("Error starting server: %v\n", err)
	}
}
```

You can clone the repository from [here](https://github.com/vishalanarase/navigating-docker):

```bash
# Clone the repository
git clone https://github.com/vishalanarase/navigating-docker

# Change to the project directory
cd navigating-docker

# Checkout to the specific version
git checkout v1.0.0

# Verify the files
tree .
.
├── Dockerfile
├── LICENSE
├── README.md
├── go.mod
├── go.sum
└── main.go
```

### Dockerfile for Application

The Dockerfile is the heart of creating a Docker image. It’s a text file that contains a series of instructions to build your image. Each instruction tells Docker how to set up the environment for your application.

Here’s a breakdown of a simple Dockerfile for a Go web application:

```dockerfile
# Start with the official Golang image as a base
FROM golang:1.23-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy the Go module files and download dependencies
COPY go.mod go.sum ./
RUN go mod download

# Copy the rest of the application code
COPY . .

# Build the Go application
RUN go build -o webserver .

# Expose the port the app runs on
EXPOSE 8081

# Command to run the application
CMD ["./webserver"]
```

### Explanation of Each Step

1. **FROM golang:1.23-alpine**:
   - This line specifies the base image for the Docker container. The `golang:1.23-alpine` image is an official Go image based on Alpine Linux, a lightweight Linux distribution. This image includes the Go programming language tools and the runtime environment, which are necessary to build and run Go applications.

2. **WORKDIR /app**:
   - This command sets the working directory inside the container to `/app`. Any subsequent commands (like copying files or running build commands) will be executed from this directory. It provides a clean, organized location for the application code inside the container.

3. **COPY go.mod go.sum ./**:
   - This copies the `go.mod` and `go.sum` files from the host machine into the `/app` directory inside the container. These files contain the module definitions and dependency information for the Go application. Copying them separately helps Docker optimize the build process by caching dependencies, so they don’t need to be downloaded again unless these files change.

4. **RUN go mod download**:
   - This command downloads the Go dependencies listed in the `go.mod` file. It ensures that all the required packages are available for the build process. This step is separated from the rest of the application code to take advantage of Docker's caching mechanism, improving build performance by avoiding unnecessary downloads in subsequent builds.

5. **COPY . .**:
   - This command copies the remaining application source code (everything except `go.mod` and `go.sum`) into the `/app` directory inside the container. It places the source code into the container, making it available for the build process.

6. **RUN go build -o webserver .**:
   - This command compiles the Go application and generates an executable binary named `webserver` in the `/app` directory. The `-o webserver` flag specifies the output file name. This step builds the Go application from the source code copied earlier.

7. **EXPOSE 8081**:
   - This exposes port `8081` on the container, indicating that the Go application will listen for incoming network requests on this port. It allows external services or users to access the application by connecting to this port.

8. **CMD ["./webserver"]**:
   - This specifies the default command to run when the container starts. It runs the `webserver` binary that was compiled in the previous step. The container will start the Go web application when it is launched, allowing it to handle incoming HTTP requests.

### Building Your Docker Image

After writing your Dockerfile, you need to build the Docker image. To do this, use the `docker build` command:

```bash
docker build -t navigating-docker:v1.0.0 .
```
Output:
```bash
[+] Building 6.4s (12/12) FINISHED                                                                                                       docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                     0.0s
 => => transferring dockerfile: 478B                                                                                                                     0.0s
 => [internal] load metadata for docker.io/library/golang:1.23-alpine                                                                                    2.2s
 => [auth] library/golang:pull token for registry-1.docker.io                                                                                            0.0s
 => [internal] load .dockerignore                                                                                                                        0.0s
 => => transferring context: 2B                                                                                                                          0.0s
 => [1/6] FROM docker.io/library/golang:1.23-alpine@sha256:6c5c9590f169f77c8046e45c611d3b28fe477789acd8d3762d23d4744de69812                              0.0s
 => [internal] load build context                                                                                                                        0.0s
 => => transferring context: 13.51kB                                                                                                                     0.0s
 => CACHED [2/6] WORKDIR /app                                                                                                                            0.0s
 => CACHED [3/6] COPY go.mod go.sum ./                                                                                                                   0.0s
 => CACHED [4/6] RUN go mod download                                                                                                                     0.0s
 => [5/6] COPY . .                                                                                                                                       0.0s
 => [6/6] RUN go build -o webserver .                                                                                                                    3.9s
 => exporting to image                                                                                                                                   0.2s
 => => exporting layers                                                                                                                                  0.2s
 => => writing image sha256:8590e8df5bf5005f607ca6ed470c6bbfa7ad03326f3476c9690baee084004d4b                                                             0.0s
 => => naming to docker.io/library/navigating-docker:v1.0.0                                                                                              0.0s

View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/8kgemvi6g35ccr88iqpchlgpg

What's next:
    View a summary of image vulnerabilities and recommendations → docker scout quickview
```

This command does the following:
- `-t navigating-docker:v1.0.0`: Tags the image with the name `navigating-docker:v1.0.0`.
- `.`: Specifies the build context, which is the current directory.

Docker will read the Dockerfile and execute the instructions to create the image. The process may take a few minutes depending on the complexity of your Dockerfile and the size of the base images.

### Verifying the Image

```bash
docker images
```
Output:
```bash
REPOSITORY                   TAG       IMAGE ID       CREATED         SIZE
navigating-docker            v1.0.0    8590e8df5bf5   2 minutes ago   324MB
```

### Running the Docker Container

Once the image is built, you can run it in a container using the `docker run` command:

```bash
docker run -p 8081:8081 navigating-docker:v1.0.0
```
Output:
```bash
Starting server on :8081
```

This command runs the `navigating-docker:v1.0.0` image in a container and maps port `8081` on your local machine to port `8081` in the container. This means you can access the Go web application by navigating to `http://localhost:8081` in your web browser or use command `curl http://localhost:8081` from CLI to see the output(`Hello, Dockerized World!`).

### Customizing Your Docker Images

Docker images are highly customizable. Here are some ways you can modify your Dockerfile to suit your application’s needs:

#### Installing Dependencies

If your Go application requires additional dependencies or tools, you can install them using the `RUN` instruction. For example, if you need to install `curl` inside the container, you can add the following line to your Dockerfile:

```dockerfile
RUN apk add --no-cache curl
```

This will install `curl` in the Alpine Linux environment.

#### Setting Environment Variables

You can set environment variables in the Dockerfile using the `ENV` instruction. This is useful for configuration values that may change between environments (e.g., development, staging, production). For example:

```dockerfile
ENV APP_ENV=production
```

You can access this environment variable within your Go application using `os.Getenv("APP_ENV")`.

#### Multi-Stage Builds

One of the most powerful features of Docker is multi-stage builds. In the example Dockerfile, we used a multi-stage build to separate the build environment from the runtime environment. This reduces the size of the final image by excluding unnecessary build tools.

 Docker is **multi-stage builds**. Multi-stage builds allow you to separate the build environment from the runtime environment, which can significantly reduce the size of the final image.

Create a Docker image for a Go web application. This example will use two stages: one for building the Go application and another for running it.

##### Dockerfile with Multi-Stage Build

Checkout the v1.0.1 tag of the Go web application repository.
```bash
git checkout v1.0.1
```
Take a look at Dockerfile
```dockerfile
# Stage 1: Build the Go application
FROM golang:1.23-alpine AS builder

# Set the working directory inside the container
WORKDIR /app

# Copy the Go module files and download dependencies
COPY go.mod go.sum ./
RUN go mod download

# Copy the rest of the application code
COPY . .

# Build the Go application
RUN go build -o webserver .

# Stage 2: Create the final runtime image
FROM alpine:latest

# Set the working directory inside the container
WORKDIR /app

# Copy the compiled Go binary from the builder stage
COPY --from=builder /app/webserver /app/webserver

# Expose the port the app runs on
EXPOSE 8081

# Command to run the application
CMD ["./webserver"]
```

###### **Stage 1: Build the Go application**

1. **FROM golang:1.23-alpine AS builder**:
   - This starts the first stage of the build process using the official Go image based on Alpine Linux. The `AS builder` tag allows us to refer to this stage later in the second stage.

2. **WORKDIR /app**:
   - This sets the working directory inside the container to `/app` for the build process.

3. **COPY go.mod go.sum ./**:
   - This copies the Go module files into the container, which are used to define the dependencies.

4. **RUN go mod download**:
   - This downloads the dependencies specified in the `go.mod` file.

5. **COPY . .**:
   - This copies the entire application source code into the container.

6. **RUN go build -o webserver .**:
   - This compiles the Go application into a binary named `webserver`.

###### **Stage 2: Create the final runtime image**

1. **FROM alpine:latest**:
   - This starts a new stage using the lightweight Alpine Linux image. This stage will be the final runtime environment for the application.

2. **WORKDIR /app**:
   - This sets the working directory inside the container to `/app` for the runtime environment.

3. **COPY --from=builder /app/webserver /app/webserver**:
   - This copies the compiled `webserver` binary from the `builder` stage into the final image.

4. **EXPOSE 8081**:
   - This exposes port `8081`, which the Go application will use to listen for incoming requests.

5. **CMD ["./webserver"]**:
   - This specifies the command to run the Go web application when the container starts.

### Building and Running the Docker Image

Once the Dockerfile is ready, you can build and run the image using the following commands.

#### Build the Docker Image

```bash
docker build -t navigating-docker:v1.0.1 .
```
Output:
```bash
[+] Building 1.8s (17/17) FINISHED                                                                                                       docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                     0.0s
 => => transferring dockerfile: 704B                                                                                                                     0.0s
 => [internal] load metadata for docker.io/library/alpine:latest                                                                                         1.7s
 => [internal] load metadata for docker.io/library/golang:1.23-alpine                                                                                    1.7s
 => [auth] library/golang:pull token for registry-1.docker.io                                                                                            0.0s
 => [auth] library/alpine:pull token for registry-1.docker.io                                                                                            0.0s
 => [internal] load .dockerignore                                                                                                                        0.0s
 => => transferring context: 2B                                                                                                                          0.0s
 => [builder 1/6] FROM docker.io/library/golang:1.23-alpine@sha256:6c5c9590f169f77c8046e45c611d3b28fe477789acd8d3762d23d4744de69812                      0.0s
 => [internal] load build context                                                                                                                        0.0s
 => => transferring context: 6.82kB                                                                                                                      0.0s
 => [stage-1 1/3] FROM docker.io/library/alpine:latest@sha256:21dc6063fd678b478f57c0e13f47560d0ea4eeba26dfc947b2a4f81f686b9f45                           0.0s
 => CACHED [stage-1 2/3] WORKDIR /app                                                                                                                    0.0s
 => CACHED [builder 2/6] WORKDIR /app                                                                                                                    0.0s
 => CACHED [builder 3/6] COPY go.mod go.sum ./                                                                                                           0.0s
 => CACHED [builder 4/6] RUN go mod download                                                                                                             0.0s
 => CACHED [builder 5/6] COPY . .                                                                                                                        0.0s
 => CACHED [builder 6/6] RUN go build -o webserver .                                                                                                     0.0s
 => CACHED [stage-1 3/3] COPY --from=builder /app/webserver /app/webserver                                                                               0.0s
 => exporting to image                                                                                                                                   0.0s
 => => exporting layers                                                                                                                                  0.0s
 => => writing image sha256:39933c0c3e4fca9b9db0e97f5d1452b7f5c793ff5a935606ae99bb9ab897b180                                                             0.0s
 => => naming to docker.io/library/navigating-docker:v1.0.1                                                                                              0.0s

View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/yo05tcn6hnqzj7zi3bte7s8fq

What's next:
    View a summary of image vulnerabilities and recommendations → docker scout quickview
```

This command builds the Docker image and tags it as `docker build -t navigating-docker:v1.0.1 .`. The build process will follow the two stages defined in the Dockerfile.

```bash
docker images
```
Output:
```bash
REPOSITORY                   TAG       IMAGE ID       CREATED          SIZE
navigating-docker            v1.0.1    39933c0c3e4f   12 minutes ago   15.4MB
```

#### Run the Docker Container

```bash
docker run -p 8081:8081 navigating-docker:v1.0.1
```
Output:
```bash
Starting server on :8081
```

This command runs the `navigating-docker:v1.0.1` image in a container and maps port `8081` on your local machine to port `8081` in the container. This means you can access the Go web application by navigating to `http://localhost:8081` in your web browser or use command `curl http://localhost:8081` from CLI to see the output(`Hello, Dockerized World!`).

### Benefits of Using Multi-Stage Builds

- **Smaller Final Image**: The final image contains only the compiled Go binary and the minimal runtime environment, making it much smaller than the image that includes the entire Go development environment.
- **Efficiency**: Docker reuses layers from previous builds, so if the source code hasn’t changed, the image will build faster. Only the stages that need rebuilding will be re-executed.
- **Cleaner Image**: The final image is free from build tools like `go` and `git`, which are only needed during the build process.

### Example of the Final Image Size

To highlight the difference in image sizes, let’s look at the sizes of images built with and without multi-stage builds:

1. **Without Multi-Stage Build**: If you were to use a single stage with the Go image, the final image would include the entire Go environment, making it larger (324 MB).
2. **With Multi-Stage Build**: After using multi-stage builds, the final image is small 15.4 MB, depending on the base image used (Alpine is known for its small size).

### Conclusion

In this post, we’ve learned how Docker images are created, how layers work, and how to customize your Dockerfile to optimize your application’s container. We also covered the powerful concept of multi-stage builds, which can significantly reduce the size of your Docker images.

In next

---
## **Key Takeaways**

1. **Docker Images as Blueprints**: Docker images are the foundation of containers, containing everything needed for an application to run, including the code, libraries, and runtime.

2. **Layer Caching for Efficiency**: Docker optimizes build times by caching layers, reusing unchanged layers to make subsequent builds faster.

3. **Multi-Stage Builds for Smaller Images**: Multi-stage builds separate the build environment from the runtime environment, resulting in smaller, optimized images with only the necessary components.

4. **Portability and Consistency**: Docker images ensure applications are portable and consistent across different environments, allowing seamless deployment from development to production.

---

## **What’s Next?**  

In the next post, We’ll dive deeper into Docker Registry —how images are built and pushed. Stay tuned as we continue our Docker journey!  

---
