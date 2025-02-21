---
title: Docker Compose - Coordinating the Crew 🚢
author: vishalanarase
date: 2025-02-21 10:00:00 +0530
categories: [Docking the Ship Navigating Docker for Developers]
tags: [docker]
render_with_liquid: false
image: assets/images/docker/title/docker-compose.webp
---

Welcome back to *Docking the Ship: Navigating Docker for Developers*! In the world of modern software development, containers have become the backbone of scalable and isolated applications. Docker, with its lightweight, portable containers, has revolutionized the way we deploy and manage applications. But what happens when your application requires multiple services that need to work together? This is where **Docker Compose** comes in—allowing you to define, run, and manage multi-container applications with ease.

In this blog post, we’ll explore Docker Compose and demonstrate how you can use it to manage a multi-container application built in **Golang**. We’ll break down how to write a `docker-compose.yml` file, manage various services, and run a multi-container application with **MySQL** as the database.

## What is Docker Compose?

Docker Compose is a tool that allows you to define and manage multi-container applications using a simple YAML configuration file (`docker-compose.yml`). It abstracts away much of the complexity involved in managing multiple containers, making it easier to manage inter-service communication, networking, and even scaling services.

With Docker Compose, you can:
- Define services, networks, and volumes.
- Set up multi-container applications in an easy-to-understand manner.
- Manage everything with a single command.

![](assets/images/docker/docker-compose.png)

---

## Example Golang Application: A Simple Web API with MySQL

Let’s say we have a simple Golang web API that needs two services to run:
1. **The Golang API Service**: A basic API that serves requests.
2. **A Database Service**: In this case, we’ll use **MySQL** to store some simple data.

Without Docker Compose, you would have to manually run each container, configure network links, and handle dependencies by yourself. Docker Compose allows us to easily manage these services in a single file.

Let’s start

- You can clone the repository from [here](https://github.com/vishalanarase/navigating-docker):

```bash
 # Clone the repository
 git clone https://github.com/vishalanarase/navigating-docker

 # Change to the project directory
 cd navigating-docker

 # Checkout to the specific version
 git checkout v1.2.0
```

*File structure:*

```bash
navigating-docker
├── Dockerfile
├── LICENSE
├── README.md
├── docker-compose.yaml
├── go.mod
├── go.sum
└── main.go

0 directories, 7 files
```

---

### Step 1: Golang Application Code (API)

Create a simple Golang application in a file named `main.go`. This API will connect to MySQL and serve data:

```go
package main

import (
	"database/sql"
	"fmt"
	"log"
	"net/http"
	"os"

	_ "github.com/go-sql-driver/mysql"
)

func dbConnection() (*sql.DB, error) {
	// MySQL connection string
	connStr := fmt.Sprintf("%s:%s@tcp(%s:%s)/%s",
		os.Getenv("DB_USER"), os.Getenv("DB_PASSWORD"), os.Getenv("DB_HOST"),
		os.Getenv("DB_PORT"), os.Getenv("DB_NAME"))

	// Open MySQL database connection
	db, err := sql.Open("mysql", connStr)
	if err != nil {
		log.Fatal(err)
	}
	defer db.Close()

	// Test the database connection
	err = db.Ping()
	if err != nil {
		log.Fatal(err)
	}
	return db, nil
}

func handler(w http.ResponseWriter, r *http.Request) {
	// Connect to the database
	db, err := dbConnection()
	if err != nil {
		http.Error(w, fmt.Sprintf("Error connecting to the database: %v", err), http.StatusInternalServerError)
		return
	}
	defer db.Close()

	_, err = w.Write([]byte("Hello, Golang API is connected to MySQL!"))
	if err != nil {
		log.Fatal(err)
	}
}

func main() {
	http.HandleFunc("/", handler)

	fmt.Println("Starting server on :8081")
	if err := http.ListenAndServe(":8081", nil); err != nil {
		fmt.Printf("Error starting server: %v\n", err)
	}
}
```

---

### Step 2: Writing the Dockerfile for the Golang Application

To ensure your Golang application is containerized correctly, you’ll need a `Dockerfile` in the same directory. Here's a simple Dockerfile to build the application:

```Dockerfile
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

---

### Step 3: Define the `docker-compose.yml`

In the root of your Golang project, create a `docker-compose.yml` file. This file will contain all the necessary configurations to run both the Golang application and the MySQL database.

```yaml
version: "3.8"

services:
  # Golang Application Service
  api:
    build: .
    ports:
      - "8081:8081"    # Map port 8080 from container to the host machine
    environment:
      - DB_HOST=db     # Environment variable pointing to the database service
      - DB_PORT=3306   # MySQL port
      - DB_USER=api
      - DB_PASSWORD=api_pass
      - DB_NAME=api_db
    depends_on:
      - db             # This ensures the db service starts before the api

  # MySQL Database Service
  db:
    image: mysql:8.0
    environment:
      - MYSQL_ROOT_PASSWORD=password
      - MYSQL_USER=api
      - MYSQL_PASSWORD=api_pass
      - MYSQL_DATABASE=api_db
    ports:
      - "3306:3306"  # Expose MySQL port to host (optional, for testing)
    volumes:
      - mysql_data:/var/lib/mysql  # Persistent data storage for MySQL

volumes:
  mysql_data:   # Named volume for persistent MySQL data storage
```

#### Breaking It Down:

1. **version**: Defines the version of the Docker Compose file format. Here, we are using version `3.8`, which is widely used and stable.
2. **services**:
   - **api**: This service represents your Golang application. It is built from the current directory (`.`), which should have a `Dockerfile` for the Golang app. The application will listen on port `8081` on the host machine.
   - **db**: This service runs a MySQL container. We use the official MySQL image from Docker Hub, setting the necessary environment variables for user, password, and database name. The data is persisted using a named volume `mysql_data`.
3. **depends_on**: Ensures the `db` service starts before the `api` service. Docker Compose will wait for the database container to be fully up before starting the API.
4. **volumes**: This section defines a persistent volume to store MySQL data. This is crucial for ensuring data persists even when containers are restarted.


### Step 4: Running the Application with Docker Compose

Now that you have all the components in place, it’s time to run everything using Docker Compose!

**Build the containers**:
   ```bash
   docker-compose build
   ```

  <details>
  <summary>
  Output
  </summary>
  ```
  ❯ docker-compose build
  WARN[0000] /Users/vishal/workspace/vishalanarase/navigating-docker/docker-compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
  [+] Building 1.8s (18/18) FINISHED                                                                                                                                                           docker:desktop-linux
   => [api internal] load build definition from Dockerfile                                                                                                                                                     0.0s
   => => transferring dockerfile: 741B                                                                                                                                                                         0.0s
   => [api internal] load metadata for docker.io/library/alpine:latest                                                                                                                                         1.7s
   => [api internal] load metadata for docker.io/library/golang:1.23-alpine                                                                                                                                    1.7s
   => [api auth] library/alpine:pull token for registry-1.docker.io                                                                                                                                            0.0s
   => [api auth] library/golang:pull token for registry-1.docker.io                                                                                                                                            0.0s
   => [api internal] load .dockerignore                                                                                                                                                                        0.0s
   => => transferring context: 2B                                                                                                                                                                              0.0s
   => [api builder 1/6] FROM docker.io/library/golang:1.23-alpine@sha256:f8113c4b13e2a8b3a168dceaee88ac27743cc84e959f43b9dbd2291e9c3f57a0                                                                      0.0s
   => [api internal] load build context                                                                                                                                                                        0.0s
   => => transferring context: 13.28kB                                                                                                                                                                         0.0s
   => [api stage-1 1/3] FROM docker.io/library/alpine:latest@sha256:a8560b36e8b8210634f77d9f7f9efd7ffa463e380b75e2e74aff4511df3ef88c                                                                           0.0s
   => CACHED [api stage-1 2/3] WORKDIR /app                                                                                                                                                                    0.0s
   => CACHED [api builder 2/6] WORKDIR /app                                                                                                                                                                    0.0s
   => CACHED [api builder 3/6] COPY go.mod go.sum ./                                                                                                                                                           0.0s
   => CACHED [api builder 4/6] RUN go mod download                                                                                                                                                             0.0s
   => CACHED [api builder 5/6] COPY . .                                                                                                                                                                        0.0s
   => CACHED [api builder 6/6] RUN go build -o webserver .                                                                                                                                                     0.0s
   => CACHED [api stage-1 3/3] COPY --from=builder /app/webserver /app/webserver                                                                                                                               0.0s
   => [api] exporting to image                                                                                                                                                                                 0.0s
   => => exporting layers                                                                                                                                                                                      0.0s
   => => writing image sha256:9a4061aa3b5a6b0e48561b19b6b20a91cb2e7cda164f4a1d6d3e834786432ad6                                                                                                                 0.0s
   => => naming to docker.io/library/navigating-docker-api                                                                                                                                                     0.0s
   => [api] resolving provenance for metadata file                                                                                                                                                             0.0s
  [+] Building 1/1
   ✔ api  Built 
  ```
  </details>

**Start the application**:
   ```bash
   docker-compose up
   ```

  <details>
  <summary>
  Output
  </summary>
   ```
   ❯ docker-compose up
   WARN[0000] /Users/vishal/workspace/vishalanarase/navigating-docker/docker-compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
   [+] Running 12/12
    ✔ db Pulled                                                                                                                                                                                                 3.9s
      ✔ 903087d703a7 Already exists                                                                                                                                                                             0.0s
      ✔ 9dcae24b624f Already exists                                                                                                                                                                             0.0s
      ✔ d1014e296527 Already exists                                                                                                                                                                             0.0s
      ✔ 5dd6251984b1 Already exists                                                                                                                                                                             0.0s
      ✔ e176816781e5 Already exists                                                                                                                                                                             0.0s
      ✔ 63ce6cde00d5 Already exists                                                                                                                                                                             0.0s
      ✔ 46a5e0bee806 Already exists                                                                                                                                                                             0.0s
      ✔ 6882305ba978 Already exists                                                                                                                                                                             0.0s
      ✔ 7678a5083faf Already exists                                                                                                                                                                             0.0s
      ✔ f41ddadd1084 Already exists                                                                                                                                                                             0.0s
      ✔ dd907af04cee Already exists                                                                                                                                                                             0.0s
   [+] Running 2/2
    ✔ Container navigating-docker-db-1   Created                                                                                                                                                                0.0s
    ✔ Container navigating-docker-api-1  Created                                                                                                                                                                0.0s
   Attaching to api-1, db-1
   db-1   | 2025-02-21 05:51:30+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.41-1.el9 started.
   db-1   | 2025-02-21 05:51:30+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
   db-1   | 2025-02-21 05:51:30+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.41-1.el9 started.
   api-1  | Starting server on :8081
   db-1   | '/var/lib/mysql/mysql.sock' -> '/var/run/mysqld/mysqld.sock'
   db-1   | 2025-02-21T05:51:30.630482Z 0 [Warning] [MY-011068] [Server] The syntax '--skip-host-cache' is deprecated and will be removed in a future release. Please use SET GLOBAL host_cache_size=0 instead.
   db-1   | 2025-02-21T05:51:30.630746Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.41) starting as process 1
   db-1   | 2025-02-21T05:51:30.634230Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
   db-1   | 2025-02-21T05:51:30.715488Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
   db-1   | 2025-02-21T05:51:30.785391Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
   db-1   | 2025-02-21T05:51:30.785411Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
   db-1   | 2025-02-21T05:51:30.786530Z 0 [Warning] [MY-011810] [Server] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
   db-1   | 2025-02-21T05:51:30.795153Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Bind-address: '::' port: 33060, socket: /var/run/mysqld/mysqlx.sock
   db-1   | 2025-02-21T05:51:30.795161Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.41'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server - GPL.
   ```
  </details>

  Your MySQL database is now running, and your Golang API is connected to it. You can access your API at `http://localhost:8081`.

  ```bash
  ❯ curl http://localhost:8081
  Hello, Golang API is connected to MySQL!
  ```
  Docker Compose will pull the MySQL image, build your Golang API container, and bring everything up. You can now access your API at `http://localhost:8080`. It will be connected to MySQL as defined in the `docker-compose.yml` file.

---

### Conclusion

In this post, we demonstrated how to use **Docker Compose** to manage a multi-container application with **MySQL**. By defining services like a Golang API and MySQL database in a `docker-compose.yml` file, we were able to easily manage their orchestration, making it simpler to run and scale our application.

With Docker Compose, you’re not just simplifying container orchestration; you’re improving the overall developer experience. Whether you’re working on local development or testing your application, Docker Compose makes it easy to coordinate the various parts of your application like a well-organized crew.

Give it a try in your own projects, and let Docker Compose coordinate the crew of containers for you!

In the next post, we’ll explore [Optimizing Dockerfiles: Sailing Faster](#) for writing and optimizing Dockerfiles. Stay tuned, and happy docking! 🚢

--- 

**Have questions or feedback?** Drop them in the comments or connect with me. Let’s navigate the Docker seas together! 🌊

---

### Resources
- [Docker Compose Official Documentation](https://docs.docker.com/compose/)
- [Go MySQL Driver Documentation](https://github.com/go-sql-driver/mysql)
- [MySQL Docker Hub](https://hub.docker.com/_/mysql)


