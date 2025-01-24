---
title: Ask Gordon - Docker’s AI Assistant for Smarter Container Management 🤖
author: vishalanarase
date: 2025-01-25 11:00:00 +0530
categories: [Docking the Ship Navigating Docker for Developers]
tags: [docker]
render_with_liquid: false
image: assets/images/docker/title/docker-gordon-ai.webp
---

Welcome back to *Docking the Ship: Navigating Docker for Developers*! As a developer immersed in the Docker ecosystem, I’m always on the lookout for tools that can enhance my workflow. Over the past couple of weeks, I’ve been using Docker’s Ask Gordon AI assistant, and I’ve got to say—it’s a game-changer, especially for Dockerfile optimization. Here’s my take on what it brings to the table and how it’s made my life easier.

### **What is Ask Gordon?**
Ask Gordon is Docker’s AI-powered assistant that integrates seamlessly into Docker Desktop and the CLI. It’s designed to help developers like us navigate the Docker ecosystem with ease, offering contextual assistance tailored to our projects. Whether you’re dealing with Dockerfiles, containers, or images, Ask Gordon is there to provide actionable insights.

---
### **Why I Started Using Ask Gordon**
Optimizing Dockerfiles has always been a crucial part of my workflow. A well-optimized Dockerfile can significantly improve build times and reduce image sizes, which is especially important when working on large-scale projects. I was intrigued by Ask Gordon’s promise to simplify this process, so I decided to give it a shot.

---
### **My Experience with Dockerfile Optimization**
One of the first things I noticed was how Ask Gordon integrates with my local environment. It analyzes my Dockerfiles and provides specific recommendations—not generic advice, but tailored suggestions based on the actual code. For instance:

- **Multi-Stage Builds**: Ask Gordon pointed out areas where I could use multi-stage builds to reduce image size.
- **Layer Caching**: It highlighted inefficiencies in the order of instructions that were impacting caching.
- **Unused Dependencies**: Gordon flagged dependencies that were included in the image but not used, helping me clean up my builds.

The best part? These suggestions are practical and easy to implement. I’ve seen noticeable improvements in build performance and image size since adopting its recommendations.

---
### **How Ask Gordon Works**
Ask Gordon’s strength lies in its contextual awareness. It doesn’t just throw random tips at you; it understands your local setup, including files, directories, and images. Here’s how it works in practice:

---
- **Command Line Integration**: Using the `docker ai` command, I can query Ask Gordon directly from the CLI. It’s quick and doesn’t disrupt my workflow.

- Here is my Dockerfile:

```Dockerfile
❯ cat Dockerfile
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

- Ask Gordon provides the following suggestions:

```bash
❯ docker ai "How can I optimize my Dockerfile"

  ### Optimized Dockerfile:

     # syntax=docker/dockerfile:1.4
     FROM golang:1.23-alpine AS builder

     WORKDIR /app

     RUN --mount=type=cache,target=/root/.cache \
         --mount=type=cache,target=/go/pkg/mod \
         --mount=type=bind,target=. \
         go build -o /webserver .

     FROM alpine:3.18

     WORKDIR /app

     COPY --from=builder /webserver /app/webserver

     EXPOSE 8081

     CMD ["./webserver"]

  ### Explanation of Optimizations:

    1. Use of BuildKit Features (--mount):

        • The RUN command in the builder stage now uses Docker BuildKit's --mount feature to optimize caching and dependency management:
            • --mount=type=cache,target=/root/.cache: Caches general build-related files (e.g., Go build cache) to speed up subsequent builds.
            • --mount=type=cache,target=/go/pkg/mod: Caches Go module dependencies, so they don't need to be downloaded again in subsequent builds if they
  haven't changed.
            • --mount=type=bind,target=.: Binds the current directory (application source code) directly into the container, avoiding the need for a
  separate COPY step for the source code. This reduces intermediate layers and simplifies the Dockerfile.

    2. Removed Redundant COPY Commands:

        • The original Dockerfile had separate COPY commands for go.mod, go.sum, and the rest of the application code. These steps are now replaced by the
  --mount=type=bind directive, which directly binds the source code into the container during the build process. This reduces the number of layers and
  simplifies the build process.

    3. Updated Base Image for Runtime Stage:

        • The runtime stage now uses alpine:3.18 instead of alpine:latest. Pinning the version ensures reproducibility and avoids potential issues caused
  by changes in the latest tag over time.

    4. Simplified Binary Output Path:

        • The Go binary is now built directly to /webserver instead of /app/webserver. This eliminates the need for an additional directory structure in
  the builder stage and simplifies the COPY command in the runtime stage.

    5. Removed Unnecessary Layers:

        • The original Dockerfile had multiple COPY and RUN commands, which created additional layers. The optimized Dockerfile consolidates these steps
  using BuildKit's --mount feature, reducing the number of layers and improving build performance.

  ### Benefits of the Optimizations:

    • Faster Builds: Caching Go modules and build artifacts significantly reduces build times for subsequent builds.
    • Smaller Image Size: Fewer layers and a more streamlined build process result in a smaller final image.
    • Reproducibility: Pinning the runtime base image version ensures consistent behavior across builds.
    • Simplified Dockerfile: The use of BuildKit features and fewer commands makes the Dockerfile easier to read and maintain.
```

- Dockerfile after Ask Gordon's optimization:

```Dockerfile
❯ cat Dockerfile
# syntax=docker/dockerfile:1.4
FROM golang:1.23-alpine AS builder

WORKDIR /app

RUN --mount=type=cache,target=/root/.cache \
    --mount=type=cache,target=/go/pkg/mod \
    --mount=type=bind,target=. \
    go build -o /webserver .

FROM alpine:3.18

WORKDIR /app

COPY --from=builder /webserver /app/webserver

EXPOSE 8081

CMD ["./webserver"]
```

---
- **Docker Desktop**: In the Desktop interface, I can select specific files or directories for analysis, and Gordon provides targeted insights.

- Here are some examples of how I used Ask Gordon:

  - How can I optimize my Dockerfile?

    ![](assets/images/docker/gordon-desktop-1.png)

  - How do I run redis?

    ![](assets/images/docker/gordon-desktop-2.png)

  - How do I write a Dockerfile for a Go application?

    ![](assets/images/docker/gordon-desktop-3.png)

  - Remove all my unused images and containers

    ![](assets/images/docker/gordon-desktop-4.png)

---
### **Privacy and Security**
One concern I had initially was data security. Docker has been transparent about how Ask Gordon handles data. The tool accesses files and metadata only when necessary, and all data transfer is encrypted. Plus, nothing is stored persistently or shared with third parties. This gave me the confidence to use it without worrying about sensitive information being exposed.

---
### **What I Love About Ask Gordon**
- **Ease of Use**: The integration is seamless, and the commands are intuitive.
- **Actionable Insights**: The recommendations are practical and immediately applicable.
- **Time Savings**: Tasks that used to take hours now take minutes, thanks to Gordon’s guidance.

---
### **Areas for Improvement**
While I’ve been impressed overall, there are a few areas where Ask Gordon could improve:

- **Performance**: Sometimes, the tool can be a bit slow, especially with large projects. Optimizing performance would make it even more valuable.
- **Documentation**: More detailed documentation and tutorials would help users get the most out of the tool.
- **Community Support**: A more active community or forum would be beneficial for troubleshooting and sharing best practices.
- **Advanced Features**: More advanced features like automated code reviews and deployment suggestions would be a game-changer.
- **Plugin Support**: Support for more plugins and integrations would make it even more versatile. vscode etc.

---
### **Final Thoughts**
Ask Gordon has quickly become an indispensable part of my Docker workflow. It’s not just an AI assistant; it feels like a teammate that’s always ready to help me optimize and streamline my projects. If you’re a developer working with Docker, I highly recommend giving it a try. It’s still in Beta, so there’s room for growth, but even in its current state, it’s a powerful tool.

Have you tried Ask Gordon? I’d love to hear your experiences and how it’s helped you in your projects. Let’s make the most of this exciting addition to the Docker ecosystem!

Try Ask Gordon by becoming a beta tester today and let me know what you think!
