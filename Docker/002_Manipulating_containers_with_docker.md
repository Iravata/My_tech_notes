---
title: "containers"
author: John Doe
date: March 22, 2005
output: pdf_document
---
<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [How to start a shell for a running Docker container ?](#how-to-start-a-shell-for-a-running-docker-container-)
  - [Steps to Start a Shell for a Running Container](#steps-to-start-a-shell-for-a-running-container)
  - [Example Workflow](#example-workflow)
  - [Notes](#notes)

<!-- /code_chunk_output -->


## How to start a shell for a running Docker container ?

To start a shell for a running Docker container, you can use the `docker exec` command. This command allows you to run a command in a running container. Here's how you can do it:

### Steps to Start a Shell for a Running Container

1. **List Running Containers:**
   - First, you need to identify the container you want to access. Use the following command to list all running containers:

     ```sh
     docker ps
     ```

   - This will display a list of running containers along with their container IDs and names.

2. **Start a Shell Session:**
   - Once you have the container ID or name, you can start a shell session. For most Linux-based containers, you can use either `bash` or `sh` as the shell. The command syntax is:

     ```sh
     docker exec -it <container_id_or_name> <shell>
     ```

   - Replace `<container_id_or_name>` with the actual container ID or name and `<shell>` with either `bash` or `sh` depending on what's available in the container.

   **Example using `bash`:**

   ```sh
   docker exec -it my-container-id bash
   ```

   **Example using `sh` (if `bash` is not available):**

   ```sh
   docker exec -it my-container-id sh
   ```

3. **Exit the Shell Session:**
   - To exit the shell session, you can use the `exit` command or press `Ctrl+D`.

### Example Workflow

1. **List Running Containers:**

   ```sh
   $ docker ps
   CONTAINER ID   IMAGE         COMMAND                  CREATED       STATUS       PORTS                    NAMES
   a1b2c3d4e5f6   my-node-app   "node server.js"         2 hours ago   Up 2 hours   0.0.0.0:3000->3000/tcp   my-node-app-container
   ```

2. **Start a Bash Shell:**

   ```sh
   $ docker exec -it a1b2c3d4e5f6 bash
   ```

   If `bash` is not available and you need to use `sh`:

   ```sh
   $ docker exec -it a1b2c3d4e5f6 sh
   ```

3. **Exit the Shell Session:**

   ```sh
   $ exit
   ```

### Notes
- The `-i` flag stands for interactive, and the `-t` flag allocates a pseudo-TTY, which together make it possible to interact with the container's shell.
- Ensure the container is running; otherwise, the `docker exec` command will fail.
- The availability of `bash` or `sh` depends on the base image used in the Dockerfile. For example, `bash` is usually available in most Ubuntu or Debian-based images, while `sh` is typically available in Alpine-based images.
