---
title: Docker Networks - Connecting The Containers 🔆
author: vishalanarase
date: 2025-01-24 10:00:00 +0530
categories: [Docking the Ship Navigating Docker for Developers]
tags: [docker]
render_with_liquid: false
image: assets/images/docker/title/docker-network.webp
---

Welcome back to *Docking the Ship: Navigating Docker for Developers*! Docker has become an essential tool in modern software development, allowing developers to package applications and all their dependencies into isolated, portable containers. However, as applications scale and involve multiple containers (e.g., a web application and a database), managing communication between these containers becomes a crucial part of the deployment process. This is where **Docker Networks** come into play, providing a robust mechanism to control how containers communicate with each other.

In this post, we’ll dive into Docker Networks, explain the theory behind them, and provide a hands-on example where we set up two containers: one running a simple **Go (Golang) web application** and another running a **MySQL database**. These containers will be connected via a Docker network, allowing them to communicate seamlessly.

---

## What Are Docker Networks?

Docker networks are a way to configure and manage communication between Docker containers. They provide the following key benefits:

- **Isolation**: Containers within a network can communicate with each other, but those outside the network are isolated unless explicitly allowed.
- **Easy Communication**: Containers in the same network can reach each other by their container names, making it easier to define relationships between services.
- **Security**: Docker networks help ensure that only containers that need to interact can do so, helping you maintain secure isolation between services.

Docker provides several types of networks, including **bridge**, **host**, **overlay**, **macvlan**, and **none**. In this example, we will use the **bridge network**, which is the default option, to connect our containers on the same host machine.

### Types of Docker Networks:
1. **Bridge** (Default): Isolates containers on the same host but allows them to communicate.
2. **Host**: Uses the host's network directly, bypassing network isolation.
3. **Overlay**: Useful for multi-host communication (e.g., Docker Swarm).
4. **Macvlan**: Assigns containers with their own IP addresses, making them appear as separate physical devices on the network.
5. **None**: No networking—completely isolated containers.

For this example, we will use a **bridge** network to link our Go web application and MySQL database.

--- 

## Theory: How Docker Networking Works

When containers are launched in Docker, they are by default connected to the default **bridge network**. This creates a private internal network on your host machine where containers can communicate with each other via IP addresses or container names.

When you connect a container to a custom network (e.g., `my-network`), Docker creates a virtual network bridge, and each container gets a unique IP address within that network. Containers can then communicate with each other using these IP addresses or simply by referring to each other by container name.

## Docker Network Setup: Example with Golang Application and MySQL

Let’s create a **Go web application** and a **MySQL database** that communicate with each other over a Docker network. We will use a Docker **bridge network** to allow these containers to talk to each other.

![](assets/images/docker/docker-network.png)

### Step 1: Create a Docker Network

- First, we will create a custom network named `my-network` to connect our containers.

```bash
docker network create my-network
```

- This command creates a new bridge network called `my-network` that we will use to connect our containers.

```bash
$ docker network create my-network
6d4f0a15db3ea33be2056a90846eb12b680e8f3cdd2af74b62b16b31c8a6f062
```

- To list all networks, use the following command:

```bash
docker network ls
NETWORK ID     NAME             DRIVER    SCOPE
149951fe52ff   bridge           bridge    local
1bd73725b9c7   host             host      local
6d4f0a15db3e   my-network       bridge    local
91f52914c6b5   none             null      local
```

### Step 2: Create a Go Web Application

Now, let's create a simple **Go web application** that connects to a MySQL database. Here’s the code for a basic Go app that uses the `github.com/go-sql-driver/mysql` package to connect to a MySQL database and serve some data via HTTP.

-  You can clone the repository from [here](https://github.com/vishalanarase/navigating-docker):

	```bash
    # Clone the repository
    git clone https://github.com/vishalanarase/navigating-docker

    # Change to the project directory
    cd navigating-docker

    # Checkout to the specific version
    git checkout v1.1.0
    ```

**File structure**:
```
navigating-docker
├── Dockerfile
├── LICENSE
├── README.md
├── go.mod
├── go.sum
└── main.go
```

#### 1. `main.go` — Go Web Application Code

```go
package main

import (
	"database/sql"
	"fmt"
	"net/http"

	_ "github.com/go-sql-driver/mysql"
)

func dbConnection() (*sql.DB, error) {
	// Connect to MySQL database
	dsn := "root:my-secret-pw@tcp(mysql-container:3306)/my_database"
	db, err := sql.Open("mysql", dsn)
	if err != nil {
		return nil, err
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

	// Query the database for a list of users
	rows, err := db.Query("SELECT id, name FROM users")
	if err != nil {
		http.Error(w, fmt.Sprintf("Error fetching data from the database: %v", err), http.StatusInternalServerError)
		return
	}
	defer rows.Close()

	// Display the results
	var users []string
	for rows.Next() {
		var id int
		var name string
		if err := rows.Scan(&id, &name); err != nil {
			http.Error(w, fmt.Sprintf("Error reading rows: %v", err), http.StatusInternalServerError)
			return
		}
		users = append(users, fmt.Sprintf("%d: %s", id, name))
	}

	// Respond with the list of users
	fmt.Fprintf(w, "Users: \n%s", users)
}

func main() {
	http.HandleFunc("/", handler)

	fmt.Println("Starting server on :8081")
	if err := http.ListenAndServe(":8081", nil); err != nil {
		fmt.Printf("Error starting server: %v\n", err)
	}
}
```

#### 2. `go.mod` — Go Modules

```go
module github.com/vishalanarase/navigating-docker

go 1.23

require github.com/go-sql-driver/mysql v1.8.0
```

#### 3. `Dockerfile` — Dockerfile to Build Go Application

Here’s a `Dockerfile` that builds and runs our Go web application inside a container:

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

- This Dockerfile performs the following tasks:
  1. Builds the Go application inside a builder container.
  2. Creates a smaller Alpine-based container with the compiled Go binary.
  3. Runs the Go web server when the container starts.

### Step 3: Build and Run the MySQL Container

- Before running the Go application, we need to set up the MySQL container. This container will use the `mysql` Docker image and connect to the same network.

```bash
$ docker run --name mysql-container --network my-network -e MYSQL_ROOT_PASSWORD=my-secret-pw -e MYSQL_DATABASE=my_database -d mysql:latest
```

This command does the following:
- `--name mysql-container`: Names the container `mysql-container`.
- `--network my-network`: Connects the container to the `my-network` network.
- `-e MYSQL_ROOT_PASSWORD=my-secret-pw`: Sets the root password for MySQL.
- `-e MYSQL_DATABASE=my_database`: Creates a new database called `my_database`.
- `-d mysql:latest`: Runs the MySQL container in detached mode using the latest MySQL image.

- After running the above command, you should see an output similar to the following:

```bash
$ docker run --name mysql-container --network my-network -e MYSQL_ROOT_PASSWORD=my-secret-pw -e MYSQL_DATABASE=my_database -d mysql:latest
53c684c161d1e47a1e36b3f01874d2bf33861c32fd33712c0155935f82411a1a
```



### Step 4: Populate MySQL with Some Data

You can now populate the MySQL database with some sample data. Connect to the MySQL container and insert data:

```bash
$ docker exec -it mysql-container mysql -uroot -pmy-secret-pw
```

```sql
USE my_database;

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100)
);

INSERT INTO users (name) VALUES ('Alice'), ('Bob'), ('Charlie');
```


```bash
$ docker exec -it mysql-container mysql -uroot -pmy-secret-pw
```

You should see a MySQL CLI prompt, Run the following SQL commands:

```bash
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 9.1.0 MySQL Community Server - GPL

Copyright (c) 2000, 2024, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> USE my_database;
Database changed
mysql> CREATE TABLE users (
    ->     id INT AUTO_INCREMENT PRIMARY KEY,
    ->     name VARCHAR(100)
    -> );
Query OK, 0 rows affected (0.05 sec)

mysql> INSERT INTO users (name) VALUES ('Alice'), ('Bob'), ('Charlie');
Query OK, 3 rows affected (0.03 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> SELECT * from users;
+----+---------+
| id | name    |
+----+---------+
|  1 | Alice   |
|  2 | Bob     |
|  3 | Charlie |
+----+---------+
3 rows in set (0.00 sec)
```

### Step 5: Build and Run the Go Web Application Container

Now, we’ll build and run the Go application in a Docker container:

```bash
$ docker build -t go-web-app .
$ docker run --name go-web-app --network my-network -p 8081:8081 -d go-web-app
```

Build the Docker image for the Go web application:

```bash
$ docker build -t go-web-app .
```
<details>
<summary>
Output
</summary>
```bash
[+] Building 17.1s (15/15) FINISHED                                                                                                      docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                     0.0s
 => => transferring dockerfile: 741B                                                                                                                     0.0s
 => [internal] load metadata for docker.io/library/golang:1.23-alpine                                                                                    1.9s
 => [internal] load metadata for docker.io/library/alpine:latest                                                                                         1.0s
 => [internal] load .dockerignore                                                                                                                        0.0s
 => => transferring context: 2B                                                                                                                          0.0s
 => [builder 1/6] FROM docker.io/library/golang:1.23-alpine@sha256:47d337594bd9e667d35514b241569f95fb6d95727c24b19468813d596d5ae596                      9.9s
 => => resolve docker.io/library/golang:1.23-alpine@sha256:47d337594bd9e667d35514b241569f95fb6d95727c24b19468813d596d5ae596                              0.0s
 => => sha256:4f92ea2a58530aa31be3e611f075346f0aa7ba98cc5e5b792cd1dffe3e1650c0 126B / 126B                                                               0.3s
 => => sha256:47d337594bd9e667d35514b241569f95fb6d95727c24b19468813d596d5ae596 10.29kB / 10.29kB                                                         0.0s
 => => sha256:e8630f35a0930fb45171797c621b77d47315081c86ef0b9d5e6ac92fe580a620 1.92kB / 1.92kB                                                           0.0s
 => => sha256:ec5d4cf8479eaf7d580ff5256466047ebf8ba3a59ff1d9c60225a3fb29a9527d 2.10kB / 2.10kB                                                           0.0s
 => => sha256:1056f4fea241387de443606efbb463ff34c3d723317da5aaff9ca1d42468d46e 297.87kB / 297.87kB                                                       0.5s
 => => sha256:8a9d514b7aa85ba6aa1665a90e4a24f9a4312a9ae69f1e26235cf49d08b80c7b 70.68MB / 70.68MB                                                         6.1s
 => => sha256:4f4fb700ef54461cfa02571ae0db9a0dc1e0cdb5577484a6d75e68dc38e8acc1 32B / 32B                                                                 0.8s
 => => extracting sha256:1056f4fea241387de443606efbb463ff34c3d723317da5aaff9ca1d42468d46e                                                                0.1s
 => => extracting sha256:8a9d514b7aa85ba6aa1665a90e4a24f9a4312a9ae69f1e26235cf49d08b80c7b                                                                3.5s
 => => extracting sha256:4f92ea2a58530aa31be3e611f075346f0aa7ba98cc5e5b792cd1dffe3e1650c0                                                                0.0s
 => => extracting sha256:4f4fb700ef54461cfa02571ae0db9a0dc1e0cdb5577484a6d75e68dc38e8acc1                                                                0.0s
 => [internal] load build context                                                                                                                        0.0s
 => => transferring context: 10.64kB                                                                                                                     0.0s
 => CACHED [stage-1 1/3] FROM docker.io/library/alpine:latest@sha256:56fa17d2a7e7f168a043a2712e63aed1f8543aeafdcee47c58dcffe38ed51099                    0.0s
 => [stage-1 2/3] WORKDIR /app                                                                                                                           0.0s
 => [builder 2/6] WORKDIR /app                                                                                                                           0.4s
 => [builder 3/6] COPY go.mod go.sum ./                                                                                                                  0.0s
 => [builder 4/6] RUN go mod download                                                                                                                    0.5s
 => [builder 5/6] COPY . .                                                                                                                               0.0s
 => [builder 6/6] RUN go build -o webserver .                                                                                                            4.2s
 => [stage-1 3/3] COPY --from=builder /app/webserver /app/webserver                                                                                      0.0s
 => exporting to image                                                                                                                                   0.0s
 => => exporting layers                                                                                                                                  0.0s
 => => writing image sha256:dce3b866b28b98b02942c5bfecb0dec77d87ced4262b1226ac20c0cbbf360566                                                             0.0s
 => => naming to docker.io/library/go-web-app                                                                                                            0.0s

View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/t6531gzu2t8qb7ml4p02vbg11

What's next:
    View a summary of image vulnerabilities and recommendations → docker scout quickview
```

</details>

Run the container with the following command:

```bash
$ docker run --name go-web-app --network my-network -p 8081:8081 -d go-web-app
bae61d8806315d13acc79dbd272127c224e680576c35b6229e14052cc57ebd2b
```

Verify that the container is running:

```bash
$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                    NAMES
bae61d880631   go-web-app     "./webserver"            4 seconds ago    Up 4 seconds    0.0.0.0:8081->8081/tcp   go-web-app
53c684c161d1   mysql:latest   "docker-entrypoint.s…"   21 minutes ago   Up 21 minutes   3306/tcp, 33060/tcp      mysql-container
```

### Step 6: Test the Application

The Go application should now be accessible at `http://localhost:8081`. Open your browser and navigate to `http://localhost:8081`. You should see the list of users pulled from the MySQL database:

```txt
Users:
1: Alice
2: Bob
3: Charlie
```

* **Browser output:**

![](assets/images/docker/network-output.png)

### Step 7: Verify the Ip Address of the Containers

Look at the IP address of the containers:
```bash
$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                    NAMES
bae61d880631   go-web-app     "./webserver"            12 minutes ago   Up 12 minutes   0.0.0.0:8081->8081/tcp   go-web-app
53c684c161d1   mysql:latest   "docker-entrypoint.s…"   33 minutes ago   Up 33 minutes   3306/tcp, 33060/tcp      mysql-container
```

Verify the IP address of the mysql containers:
```bash
$ docker exec -it mysql-container bash
bash-5.1# cat /etc/hosts
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
172.21.0.2	53c684c161d1
bash-5.1#
```

Verify the IP address of the go-web-app containers:
```bash
$ docker exec -it go-web-app sh
/app # cat /etc/hosts
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
172.21.0.3	bae61d880631
/app # ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:AC:15:00:03
          inet addr:172.21.0.3  Bcast:172.21.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:43 errors:0 dropped:0 overruns:0 frame:0
          TX packets:35 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:4113 (4.0 KiB)  TX bytes:2812 (2.7 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:4 errors:0 dropped:0 overruns:0 frame:0
          TX packets:4 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:297 (297.0 B)  TX bytes:297 (297.0 B)

/app #
```

### Step 8: Verify the Docker Network

To verify that both containers are connected to the same Docker network, run:

```bash
$ docker network inspect my-network
```

This will show you information about the containers connected to the `my-network` network.

```bash
$ docker network inspect my-network
[
    {
        "Name": "my-network",
        "Id": "6d4f0a15db3ea33be2056a90846eb12b680e8f3cdd2af74b62b16b31c8a6f062",
        "Created": "2025-01-23T14:04:14.556068508Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.21.0.0/16",
                    "Gateway": "172.21.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "53c684c161d1e47a1e36b3f01874d2bf33861c32fd33712c0155935f82411a1a": {
                "Name": "mysql-container",
                "EndpointID": "e0ee2283e249de8c54d46893c5d7fa69f3bf9367f7d5f31c2274f5a54eb44710",
                "MacAddress": "02:42:ac:15:00:02",
                "IPv4Address": "172.21.0.2/16",
                "IPv6Address": ""
            },
            "bae61d8806315d13acc79dbd272127c224e680576c35b6229e14052cc57ebd2b": {
                "Name": "go-web-app",
                "EndpointID": "6494c67bdcdd0a13c60a3f501459fbc6139b6833f3c12c4c36fae4ce71cf6a0c",
                "MacAddress": "02:42:ac:15:00:03",
                "IPv4Address": "172.21.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```
---

## Conclusion

Docker Networks are a powerful way to manage communication between containers. In this post, we explored how Docker networking works by creating a simple Go web application and a MySQL container, both running on the same custom Docker network. This example demonstrates the ease with which containers can communicate when they share a network, making it possible to design microservice-based

In the next post, we’ll explore [Docker Compose: Coordinating the Crew](#) for managing multi-container applications. Stay tuned, and happy docking! 🚢

--- 

**Have questions or feedback?** Drop them in the comments or connect with me. Let’s navigate the Docker seas together! 🌊

---

## Resources & References
- [Docker Official Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)

---
