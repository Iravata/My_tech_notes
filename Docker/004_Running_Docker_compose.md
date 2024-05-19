## 🙋 What is docker compose and why should we use them?

Docker Compose is a tool that simplifies the process of managing multi-container Docker applications. It allows you to define and run multiple containers as a single service using a YAML file (`docker-compose.yml`). Here are some advantages of using Docker Compose over standalone Docker:

### Advantages of Using Docker Compose

1. **Multi-Container Management:**
   - Docker Compose makes it easy to manage multiple containers as a single application. This is especially useful for applications that have several interconnected services, such as a web server, database, and caching layer.

2. **Simplified Configuration:**
   - With Docker Compose, you can define all your container configurations in a single `docker-compose.yml` file. This file can include container definitions, network configurations, volume mounts, and environment variables, making it easier to manage and version control.

3. **Service Dependencies:**
   - Docker Compose allows you to define dependencies between services. You can specify which services need to start first, ensuring that your application starts in the correct order.

4. **Networking:**
   - Docker Compose automatically creates a network for your application, enabling seamless communication between containers without the need for manual network configuration.

5. **Environment Configuration:**
   - You can use environment variables in your `docker-compose.yml` file to manage different configurations for different environments (e.g., development, staging, production).

6. **Ease of Use:**
   - With Docker Compose, you can bring up an entire application stack with a single command (`docker-compose up`). This reduces the complexity of managing multiple `docker run` commands and options.

7. **Portability:**
   - The `docker-compose.yml` file can be easily shared and versioned, ensuring that the application stack is consistent across different environments and team members.

8. **Scaling:**
   - Docker Compose allows you to scale services easily. You can specify the number of instances for a particular service using the `docker-compose up --scale` command.

9. **Volumes and Data Persistence:**
   - Docker Compose makes it straightforward to define and manage volumes for data persistence. You can specify volume mounts in the `docker-compose.yml` file, ensuring that your data is retained even when containers are restarted.

### Example `docker-compose.yml` File

Here’s a simple example of a `docker-compose.yml` file for a Node.js application with a MongoDB database:

```yaml
version: '3'
services:
  web:
    image: node:14
    working_dir: /app
    volumes:
      - .:/app
    command: npm start
    ports:
      - "3000:3000"
    depends_on:
      - db

  db:
    image: mongo:latest
    volumes:
      - mongo-data:/data/db
    ports:
      - "27017:27017"

volumes:
  mongo-data:
```

### Using Docker Compose

1. **Start Services:**
   - Run the following command in the directory containing your `docker-compose.yml` file:

     ```sh
     docker-compose up
     ```

   - This will start all the services defined in the `docker-compose.yml` file.

2. **Stop Services:**
   - To stop the services, use:

     ```sh
     docker-compose down
     ```

3. **Scaling Services:**
   - To scale a service, for example, running three instances of the `web` service:

     ```sh
     docker-compose up --scale web=3
     ```

4. **Running in Detached Mode:**
   - To start the services in detached mode (running in the background), use:

     ```sh
     docker-compose up -d
     ```

### Conclusion

Docker Compose offers significant advantages for managing multi-container Docker applications, including simplified configuration, easy management of service dependencies, and improved portability and scalability. It is an essential tool for developing, testing, and deploying complex applications with multiple interconnected services.

### Rebuilding a Docker Compose Container

To rebuild a Docker Compose container, you typically need to do the following:

1. **Make changes to the Dockerfile or your application code.**
2. **Run the `docker-compose up --build` command.**

This command will rebuild the images specified in your `docker-compose.yml` file and restart the containers.

**Example:**

```sh
docker-compose up --build
```

If you want to ensure that the existing containers are removed and rebuilt, you can use the `--force-recreate` option:

```sh
docker-compose up --build --force-recreate
```
---
## 🙋 Explain the restart policies in docker compose?
### Restart Policies in Docker Compose

Docker provides several restart policies to control the restart behavior of containers. These policies can be specified in your `docker-compose.yml` file under the `restart` option. Here are the available restart policies:

1. **no:**
   - Containers will not be restarted if they stop or crash.
   - Example:

     ```yaml
     services:
       web:
         image: my-web-app
         restart: "no"
     ```

2. **always:**
   - Containers will always be restarted if they stop, regardless of the exit status.
   - Example:

     ```yaml
     services:
       web:
         image: my-web-app
         restart: always
     ```

3. **on-failure:**
   - Containers will be restarted only if they exit with a non-zero exit status (indicating a failure).
   - You can optionally specify a maximum number of restarts.
   - Example:

     ```yaml
     services:
       web:
         image: my-web-app
         restart: on-failure
     ```

   - With a maximum number of restarts:

     ```yaml
     services:
       web:
         image: my-web-app
         restart: on-failure:5
     ```

4. **unless-stopped:**
   - Containers will always be restarted unless they are explicitly stopped or Docker itself is stopped.
   - Example:

     ```yaml
     services:
       web:
         image: my-web-app
         restart: unless-stopped
     ```

### Complete Example of a `docker-compose.yml` File with Restart Policies

Here is a complete example of a `docker-compose.yml` file that includes different restart policies:

```yaml
version: '3.8'

services:
  web:
    image: my-web-app
    build: .
    ports:
      - "3000:3000"
    restart: always
    depends_on:
      - db

  db:
    image: mongo:latest
    volumes:
      - mongo-data:/data/db
    restart: on-failure:5

volumes:
  mongo-data:
```

### Summary

To rebuild Docker Compose containers, use the `docker-compose up --build` command. Restart policies such as `no`, `always`, `on-failure`, and `unless-stopped` help manage container behavior upon crashes or stops, and can be defined in your `docker-compose.yml` file. This provides better control over container lifecycle management.

Here is a comparison of commonly used Docker commands and their Docker Compose equivalents, grouped by function for easy reference:

---
## 🙋 Some common analogues commands between Docker and docker compose.
### Container Management

| Function             | Docker Command                          | Docker Compose Command               |
|----------------------|-----------------------------------------|--------------------------------------|
| Start containers     | `docker run <image>`                    | `docker-compose up`                  |
| Stop containers      | `docker stop <container>`               | `docker-compose stop`                |
| Remove containers    | `docker rm <container>`                 | `docker-compose rm`                  |
| List containers      | `docker ps`                             | `docker-compose ps`                  |
| Restart containers   | `docker restart <container>`            | `docker-compose restart`             |

### Building and Rebuilding

| Function             | Docker Command                          | Docker Compose Command               |
|----------------------|-----------------------------------------|--------------------------------------|
| Build images         | `docker build -t <image> .`             | `docker-compose build`               |
| Rebuild containers   | `docker build --no-cache -t <image> .`  | `docker-compose up --build`          |

### Logs and Monitoring

| Function             | Docker Command                          | Docker Compose Command               |
|----------------------|-----------------------------------------|--------------------------------------|
| View logs            | `docker logs <container>`               | `docker-compose logs`                |
| View top processes   | `docker top <container>`                | `docker-compose top`                 |

### Networks and Volumes

| Function             | Docker Command                          | Docker Compose Command               |
|----------------------|-----------------------------------------|--------------------------------------|
| List networks        | `docker network ls`                     | `docker-compose network ls`          |
| Create network       | `docker network create <network>`       | Defined in `docker-compose.yml`      |
| List volumes         | `docker volume ls`                      | `docker-compose volume ls`           |
| Create volume        | `docker volume create <volume>`         | Defined in `docker-compose.yml`      |

### Lifecycle Commands

| Function             | Docker Command                          | Docker Compose Command               |
|----------------------|-----------------------------------------|--------------------------------------|
| Pull images          | `docker pull <image>`                   | `docker-compose pull`                |
| Push images          | `docker push <image>`                   | `docker-compose push`                |
| Remove images        | `docker rmi <image>`                    | Not directly available               |

### Utility Commands

| Function             | Docker Command                          | Docker Compose Command               |
|----------------------|-----------------------------------------|--------------------------------------|
| Execute command      | `docker exec -it <container> <command>` | `docker-compose exec <service> <command>` |
| Run one-off command  | `docker run <image> <command>`          | `docker-compose run <service> <command>` |

### Cleanup

| Function             | Docker Command                          | Docker Compose Command               |
|----------------------|-----------------------------------------|--------------------------------------|
| Remove stopped containers  | `docker container prune`          | `docker-compose rm`                  |
| Remove unused images       | `docker image prune`              | Not directly available               |
| Remove unused networks     | `docker network prune`            | Not directly available               |
| Remove unused volumes      | `docker volume prune`             | Not directly available               |

### Example Usage

- **Starting Containers:**
  ```sh
  docker-compose up
  ```

- **Stopping Containers:**
  ```sh
  docker-compose stop
  ```

- **Viewing Logs:**
  ```sh
  docker-compose logs
  ```

- **Executing a Command in a Running Container:**
  ```sh
  docker-compose exec web bash
  ```

This table provides a quick reference to understand the analogous commands between Docker and Docker Compose, grouped by their functions to make it easier to find what you need.