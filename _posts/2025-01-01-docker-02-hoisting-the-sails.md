---
title: Docker Basics: Hoisting the Sails 🚢
author: vishalanarase
date: 2025-01-01 16:00:00 +0530
categories: [Docking the Ship Navigating Docker for Developers]
tags: [docker]
render_with_liquid: false
---

![](assets/images/docker/title/hoisting-sail.webp)

Welcome back to **"Docking the Ship: Navigating Docker for Developers"**! After learning what Docker is and why it’s a game changer, it’s time to dive into the fundamentals. In this post, we’ll focus on installing Docker, setting up your environment, and running your first container. Ready to hoist the sails? Let’s get started!  

---

## **Why Learn the Basics?**  
Understanding Docker basics is essential to getting comfortable with its powerful yet simple tools. These foundational skills will enable you to explore more advanced features and workflows in the future.  

---

## **1. Installing Docker**  

To get started with Docker, you’ll first need to install it on your system. Here’s a quick overview:  

### **Step 1: Download Docker**  
- Visit the Docker Desktop Download Page and download the version for your OS (Windows, macOS, or Linux).  
- For Linux users, refer to the official documentation for installation instructions.  

### **Step 2: Install and Set Up**  
- Run the installer and follow the setup instructions.  
- For Windows and macOS, Docker Desktop provides an easy-to-use GUI and CLI tools.  

### **Step 3: Verify Installation**  
After installation, verify that Docker is running by opening a terminal and typing:  

```bash
docker --version
```  

If it returns the Docker version, you're all set to move forward! 🚀  


Output: Docker version on my machine
```bash
Docker version 20.10.12, build e91ed57
```

---

## **2. Running Your First Container**  

Now that Docker is installed, it’s time to run your first container! Containers are lightweight, portable environments that run your applications.  

### **Step 1: Pull an Image**  
Docker uses images to create containers. Let’s start with the classic "Hello, World!" example. Pull the `hello-world` image by running:  

```bash
docker pull hello-world
```
Output:
```bash
Using default tag: latest
latest: Pulling from library/hello-world
478afc919002: Pull complete
Digest: sha256:5b3cc85e16e3058003c13b7821318369dad01dac3dbb877aac3c28182255c724
Status: Downloaded newer image for hello-world:latest
docker.io/library/hello-world:latest
```

### **Step 2: Run the Container**  
Create and run a container from the image:  

```bash
docker run hello-world
```

This command downloads the image (if not already available) and starts a container that displays a "Hello, World!" message.  

```bash
Hello from Docker!
 ```

### **Step 3: Check Running Containers**  
To view active containers, use:  

```bash
docker ps
``` 

No containers running? That’s okay—`hello-world` finishes after it outputs the message. To see all containers (running or stopped):  

```bash
docker ps -a
```
Output:
```bash
CONTAINER ID   IMAGE                               COMMAND                  CREATED          STATUS                        PORTS                               NAMES
9588e7e356ab   hello-world                         "/hello"                 21 minutes ago   Exited (0) 21 minutes ago                                         jovial_dirac
```
---

## **3. Understanding Essential Docker Commands**  

Here’s a quick reference to some basic Docker commands you’ll use frequently:  

- **`docker run`**: Create and start a container from an image.  
- **`docker ps`**: List currently running containers.  
- **`docker ps -a`**: List all containers (including stopped ones).  
- **`docker stop [container_id]`**: Stop a running container.  
- **`docker rm [container_id]`**: Remove a stopped container.  
- **`docker images`**: List all downloaded images.  
- **`docker rmi [image_id]`**: Remove an image from your system.  

---

## **4. Hands-On Practice**  

Try running a few additional containers to get more comfortable:  

- **Nginx Web Server:**  
  
  ```bash
  docker run -d -p 8080:80 nginx
  ```
  Output:
  ```bash
  Unable to find image 'nginx:latest' locally
  latest: Pulling from library/nginx
  f5c6876bb3d7: Pull complete
  bdb964b66a74: Pull complete
  b5368be906ec: Pull complete
  755bf136756e: Pull complete
  aafc2e219c20: Pull complete
  261c6a94b398: Pull complete
  a8066ea4829b: Pull complete
  Digest: sha256:42e917aaa1b5bb40dd0f6f7f4f857490ac7747d7ef73b391c774a41a8b994f15
  Status: Downloaded newer image for nginx:latest
  7dce55307298b2134286524d26e1286f3e1c972cf0b5c3208a9ccf4a54511f2e
  ```

  Visit `http://localhost:8080` in your browser to see the Nginx welcome page.  

  ![](assets/images/docker/nginx-welcome.png)

- **BusyBox Shell:**  
  
  ```bash
  docker run -it busybox
  ```
  Output:
  ```bash
  Unable to find image 'busybox:latest' locally
  latest: Pulling from library/busybox
  559c60843878: Pull complete
  Digest: sha256:2919d0172f7524b2d8df9e50066a682669e6d170ac0f6a49676d54358fe970b5
  Status: Downloaded newer image for busybox:latest
  / # whoami
  root
  / # exit
  ```

  This gives you an interactive shell inside a lightweight BusyBox container. Type `exit` to leave.  

---

## **5. Troubleshooting Common Issues**  

Sometimes things don’t go as planned. Here are quick fixes for common problems:  

- **Permission Issues:**  
  If you encounter permission errors, ensure your user belongs to the Docker group (Linux).  
  
  ```bash
  sudo usermod -aG docker $USER
  ```

- **Docker Daemon Not Running:**  
  Ensure the Docker service is active.  
  
  ```bash
  sudo systemctl start docker
  ```  

- **Network Issues:**  
  Check your firewall or proxy settings if pulling images fails.  

---

## **Key Takeaways**  
- Docker containers are lightweight, fast, and portable.  
- Images are the building blocks of containers.  
- Mastering basic Docker commands is the first step toward becoming proficient in containerization.  

---

### Resources:
- [Docker Official Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Docker Getting Started Guide](https://docs.docker.com/get-started/)

---
## **What’s Next?**  

In the next post, We’ll dive deeper into images—how they are built, customized, and optimized. Stay tuned as we continue our Docker journey!  

---

Got questions or feedback? Drop a comment below or reach out—I’d love to hear your thoughts!

Let’s continue this journey together!
