# Lab 7: Docker Containers and Images

**Execution environment:** Your assigned Ubuntu server  
**Submission repository:** `DevOps`  
**Submission directory:** `Labs/Lab07`  
**Screenshot directory:** `Labs/Lab07/screenshots`

Install Docker and Docker Compose on your Ubuntu server if they are missing. If both tools are already installed, go directly to the version checks in Task 1. Then run PostgreSQL and NGINX in Docker, manage containers and images, and save changes from an NGINX container to a new image.

```text
DevOps/
└── Labs/
    └── Lab07/
        ├── README.md
        └── screenshots/
            ├── docker_installation.png
            ├── postgres_missing_password.png
            ├── postgres_96_running.png
            ├── postgres_both_versions.png
            ├── docker_info.png
            ├── docker_search_ubuntu.png
            ├── ubuntu_container_status.png
            ├── postgres_96_cleanup.png
            ├── nginx_containers_started.png
            ├── nginx_latest_page.png
            ├── nginx_older_page.png
            ├── nginx_latest_logs.png
            ├── nginx_attach_detach.png
            ├── nginx_exec_shell.png
            ├── nginx_diff.png
            ├── nginx_inspect.png
            └── nginx_commit.png
```

The instructor supplies this README. Create your own `screenshots/` directory in your `DevOps/Labs/Lab07` submission.

## Lab Objective

By the end of the published tasks, you will be able to:

- Install Docker Engine and the Docker Compose plug-in on Ubuntu if needed.
- Check the installed Docker and Docker Compose versions.
- Pull a PostgreSQL image by its tag and identify a missing initialization password.
- Start two PostgreSQL containers in the background and identify their image tags with `docker ps`.
- Inspect the Docker installation, list local images and search Docker Hub.
- Compare running and stopped containers, then restart, stop and remove a lab container and remove its image tag.
- Run two NGINX containers on different host ports, open both welcome pages and view the container logs.
- Attach to the main process, open a separate shell, inspect filesystem changes and save a new image.

## Prerequisites

- Use your assigned Ubuntu server and the account prepared in the earlier labs.
- Your Ubuntu username matches your registered GitHub username, and your hostname is `ubuntu`.
- Have internet access to pull the images and `sudo` access if Docker installation is needed.
- The Compose download command below is for an `x86_64` Ubuntu server.
- Have a browser that can reach your Ubuntu server on ports `6001` and `6002`. These ports must be free before starting Task 4.

## Task List

- [Screenshot Rules](#screenshot-rules)
- [Required Screenshot Filenames](#required-screenshot-filenames)
- [Getting Started](#getting-started)
- [Task 1: Install Docker and Docker Compose](#task-1-install-docker-and-docker-compose)
- [Task 2: Run Two PostgreSQL Versions in Docker](#task-2-run-two-postgresql-versions-in-docker)
- [Task 3: Inspect Docker and Manage Containers and Images](#task-3-inspect-docker-and-manage-containers-and-images)
- [Task 4: Run Two NGINX Containers on Different Ports](#task-4-run-two-nginx-containers-on-different-ports)
- [Task 5: Work Inside a Container and Save a New Image](#task-5-work-inside-a-container-and-save-a-new-image)
- [Summary](#summary)
- [Final Submission Checklist](#final-submission-checklist)
- [Submission](#submission)
- [Official References](#official-references)

---

## Screenshot Rules

Read these rules before starting.

**Exam evaluation screenshots are not graded.** They are for practice only and do not count toward the lab marks.

1. Use an Ubuntu account whose terminal prompt shows your registered GitHub username before `@` and `ubuntu` as the hostname:

   ```text
   YourGitHubUsername@ubuntu:~$
   ```

   The comparison is case-insensitive, but the spelling must match the `user_name` registered in the Student Marks Portal.

2. For terminal evidence, show your `YourGitHubUsername@ubuntu` prompt, the required commands and their output. For browser evidence, show the page and its address bar with your Ubuntu terminal prompt visible beside it.
3. In Task 1, capture both version checks in one screenshot. No screenshots of the installation steps are required. In Tasks 2–5, follow the screenshot instructions under the corresponding steps. When a step asks for multiple commands in one screenshot, keep them together.
4. Save your screenshots inside `Labs/Lab07/screenshots/`.
5. Use the exact filenames and capitalization given below.
6. Keep the terminal text clear and readable.
7. Keep your screenshots unedited, with the required commands and their relevant output visible.
8. Do not expose real passwords, access tokens, private keys or other secrets. The supplied example value `password` may appear in Task 2 evidence.
9. Capture your own work. Do not copy or reuse another student's screenshots.
10. Commit and push your complete `Labs/Lab07` directory to your `DevOps` repository when all tasks are published and completed.

### Required Screenshot Filenames

Lab 7 requires these 17 guided-task screenshots. Save them inside `Labs/Lab07/screenshots/` using the exact filenames below:

```text
docker_installation.png
postgres_missing_password.png
postgres_96_running.png
postgres_both_versions.png
docker_info.png
docker_search_ubuntu.png
ubuntu_container_status.png
postgres_96_cleanup.png
nginx_containers_started.png
nginx_latest_page.png
nginx_older_page.png
nginx_latest_logs.png
nginx_attach_detach.png
nginx_exec_shell.png
nginx_diff.png
nginx_inspect.png
nginx_commit.png
```

---

## Getting Started

Open your assigned Ubuntu server terminal. In your existing `DevOps` repository, create the Lab 7 screenshot directory:

```bash
cd ~/DevOps
mkdir -p Labs/Lab07/screenshots
```

---

## Task 1: Install Docker and Docker Compose

1. If Docker and Docker Compose are already installed, skip to Step 5.

2. If Docker is missing, update the Ubuntu package index and install Docker Engine and `curl`:

   ```bash
   sudo apt update -y
   sudo apt install -y docker.io curl
   ```

3. If Docker Compose is missing, install the CLI plug-in. Make sure `curl` is available before running these commands:

   ```bash
   sudo mkdir -p /usr/local/lib/docker/cli-plugins
   sudo curl -SL https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64 -o /usr/local/lib/docker/cli-plugins/docker-compose
   sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
   ```

4. If you installed Docker in Step 2, start the Docker service, add your Ubuntu account to the `docker` group and sign out:

   ```bash
   sudo systemctl start docker
   sudo usermod -aG docker $USER
   exit
   ```

   Reconnect to the Ubuntu server so the new group membership takes effect.

5. Run both version checks in the same Ubuntu terminal:

   ```bash
   docker --version
   docker compose version
   ```

   Both commands must display their installed versions.

   - Screenshot as `docker_installation.png`, showing both version commands, their outputs and your Ubuntu terminal prompt.

**Screenshot Required:**

- `docker_installation.png`

---

## Task 2: Run Two PostgreSQL Versions in Docker

Run all commands from the same Ubuntu terminal. The value `password` is a dummy password for these lab containers.

1. Pull the PostgreSQL 9.6 image:

   ```bash
   docker pull postgres:9.6
   ```

2. Try to run the image without setting a password:

   ```bash
   docker run postgres:9.6
   ```

   This attempt is expected to exit with an error because `POSTGRES_PASSWORD` was not supplied for the new database.

   - Screenshot as `postgres_missing_password.png`, showing `docker run postgres:9.6` and the missing-password error.

3. Start a new PostgreSQL 9.6 container with the password set. The `-d` flag runs it in the background and returns control to your terminal:

   ```bash
   docker run -d -e POSTGRES_PASSWORD=password postgres:9.6
   ```

4. List the running containers:

   ```bash
   docker ps
   ```

   Check that `postgres:9.6` appears in the `IMAGE` column and its status is `Up`.

   - Screenshot as `postgres_96_running.png`, showing `docker ps` and the running `postgres:9.6` container.

5. Start a PostgreSQL 10.10 container in the background:

   ```bash
   docker run -d -e POSTGRES_PASSWORD=password postgres:10.10
   ```

6. List the running containers again:

   ```bash
   docker ps
   ```

   Confirm that `postgres:9.6` and `postgres:10.10` both appear in the `IMAGE` column with an `Up` status.

   - Screenshot as `postgres_both_versions.png`, showing `docker ps` and both running PostgreSQL containers.

**Screenshot Required:**

- `postgres_missing_password.png`
- `postgres_96_running.png`
- `postgres_both_versions.png`

---

## Task 3: Inspect Docker and Manage Containers and Images

Use the same Ubuntu server as Task 2. Keep the running `postgres:9.6` container until Step 4.

1. Display information about the Docker installation:

   ```bash
   docker info
   ```

   - Screenshot as `docker_info.png`, showing the command and the Docker server details.

2. Search Docker Hub for Ubuntu images:

   ```bash
   docker search ubuntu
   ```

   - Screenshot as `docker_search_ubuntu.png`, showing the command and the search result for the official `ubuntu` image.

3. List local images, start an Ubuntu container, and compare the two container lists. Run the four commands in order. Give the new Ubuntu container a moment to exit before running `docker ps`.

   ```bash
   docker images
   docker run -d ubuntu
   docker ps
   docker ps -a
   ```

   The new Ubuntu container exits because its default process has no interactive session or other work to keep it running. Match the container ID returned by `docker run` to the `CONTAINER ID` column. The new container is absent from `docker ps` because that command lists only running containers. In `docker ps -a`, find it under the `ubuntu` image with an `Exited` status.

   - Screenshot as `ubuntu_container_status.png`. Show all four commands and their relevant output in **one screenshot**, including the running-only list and the `Exited` Ubuntu container in the full list.

4. Restart, stop and remove the PostgreSQL 9.6 container, then remove its image tag.

   In the `docker ps -a` output, find the **running** row with `postgres:9.6` in the `IMAGE` column. Copy the value from its `NAMES` column and substitute it for `<postgres-96-container-name>` in the first three commands.

   If the missing-password attempt from Task 2 still appears as a separate `Exited` row for `postgres:9.6`, remove that failed lab container first with `docker rm <failed-container-name-or-id>` using its actual name or ID. A stopped container can still prevent image removal. This preparation does not require a screenshot.

   ```bash
   docker restart <postgres-96-container-name>
   docker stop <postgres-96-container-name>
   docker rm <postgres-96-container-name>
   docker rmi postgres:9.6
   ```

   Leave the `postgres:10.10` container running.

   - Screenshot as `postgres_96_cleanup.png`. Show all four commands and their results in **one screenshot**.

**Screenshot Required:**

- `docker_info.png`
- `docker_search_ubuntu.png`
- `ubuntu_container_status.png`
- `postgres_96_cleanup.png`

---

## Task 4: Run Two NGINX Containers on Different Ports

Run these commands on the Ubuntu server. Each NGINX container listens on port 80 inside Docker. Publish them on different host ports so both pages can be opened from your browser.

1. Start the two containers:

   ```bash
   docker run -dit -p 6001:80 --name nginx-latest nginx
   docker run -d -p 6002:80 --name nginx-older nginx:1.30.6
   ```

   The first command uses the default `latest` tag. The second uses the published `1.30.6` tag. Both containers run in the background. The first also uses `-i` and `-t` so you can detach from it using the keyboard in Task 5.

   - Screenshot as `nginx_containers_started.png`, showing both `docker run` commands and their returned container IDs.

2. Open `http://<server-ip>:6001` in your browser. Replace `<server-ip>` with the address of your Ubuntu server. Confirm that you see the default NGINX welcome page from `nginx-latest`.

   - Screenshot as `nginx_latest_page.png`, showing the welcome page, the address bar with port `6001`, and your Ubuntu terminal prompt beside the browser.

3. Open `http://<server-ip>:6002` in another tab. Confirm that you see the default NGINX welcome page from `nginx-older`.

   - Screenshot as `nginx_older_page.png`, showing the welcome page, the address bar with port `6002`, and your Ubuntu terminal prompt beside the browser.

4. View the logs from `nginx-latest`:

   ```bash
   docker logs nginx-latest
   ```

   Look for the access log entry from opening the page on port `6001`.

   - Screenshot as `nginx_latest_logs.png`, showing `docker logs nginx-latest` and a request from your browser.

**Screenshot Required:**

- `nginx_containers_started.png`
- `nginx_latest_page.png`
- `nginx_older_page.png`
- `nginx_latest_logs.png`

---

## Task 5: Work Inside a Container and Save a New Image

Use the running `nginx-latest` container from Task 4.

1. Attach to the running NGINX process:

   ```bash
   docker attach nginx-latest
   ```

   This connects your terminal to the container's main process. It does not open a shell. Refresh the port `6001` page in your browser to see a new request in the attached terminal. Press **Ctrl+P**, then **Ctrl+Q** to detach and return to your Ubuntu prompt while NGINX keeps running.

   - Screenshot as `nginx_attach_detach.png`, showing `docker attach nginx-latest`, the request log and your Ubuntu prompt after detaching.

2. Open a separate shell inside the same container:

   ```bash
   docker exec -it nginx-latest /bin/sh
   ```

   Run the following commands inside the container. List the root directory, create two files in the NGINX web directory, remove `oldfile.html`, and view the environment variables. `exit` closes this shell and returns you to Ubuntu.

   ```sh
   ls /
   touch /usr/share/nginx/html/newfile.html
   touch /usr/share/nginx/html/oldfile.html
   rm /usr/share/nginx/html/oldfile.html
   printenv
   exit
   ```

   - Screenshot as `nginx_exec_shell.png`, showing the `docker exec` command, the commands run inside the shell, their relevant output and your return to the Ubuntu prompt.

3. On the Ubuntu server, compare the container filesystem with its starting state:

   ```bash
   docker diff nginx-latest
   ```

   Look for `A /usr/share/nginx/html/newfile.html`. The `A` marks an added file. `oldfile.html` was created and removed during the same exercise, so it should not appear as a remaining change.

   - Screenshot as `nginx_diff.png`, showing `docker diff nginx-latest` and the added `newfile.html`.

4. Inspect the container's configuration and state:

   ```bash
   docker inspect nginx-latest
   ```

   Look for the running state and the port mapping in the JSON output.

   - Screenshot as `nginx_inspect.png`, showing the command and the container state near the start of the output.

5. Save the container's filesystem changes as a new local image:

   ```bash
   docker commit nginx-latest nginx:myimage
   ```

   Docker creates the image tagged `nginx:myimage` and prints its image ID.

   - Screenshot as `nginx_commit.png`, showing the commit command and the returned image ID.

**Screenshot Required:**

- `nginx_attach_detach.png`
- `nginx_exec_shell.png`
- `nginx_diff.png`
- `nginx_inspect.png`
- `nginx_commit.png`

---

## Summary

- Docker and Docker Compose are installed or were already available on your Ubuntu server.
- You checked both tool versions and captured them in one screenshot.
- You observed the PostgreSQL initialization error, supplied the password, and confirmed that both PostgreSQL versions were running.
- You inspected Docker, searched Docker Hub for Ubuntu and found the exited Ubuntu container in `docker ps -a`.
- You restarted, stopped and removed the PostgreSQL 9.6 container, then removed its image tag.
- You opened both NGINX welcome pages on different host ports and checked the request log.
- You attached to NGINX, changed files in a separate shell, inspected the container and saved `nginx:myimage`.

## Final Submission Checklist

- [ ] Both version commands show their installed versions in my Ubuntu terminal.
- [ ] My `YourGitHubUsername@ubuntu` prompt and both command outputs are readable in `Labs/Lab07/screenshots/docker_installation.png`.
- [ ] I captured the missing-password error, the running PostgreSQL 9.6 container, and both containers running together.
- [ ] I saved separate screenshots for `docker info` and `docker search ubuntu`.
- [ ] I captured the four Ubuntu commands in one screenshot and the four PostgreSQL 9.6 commands in another.
- [ ] I captured both NGINX welcome pages with their ports visible and viewed the logs from `nginx-latest`.
- [ ] I detached from NGINX, ran the shell commands, checked the filesystem changes and saved `nginx:myimage`.
- [ ] All 17 screenshots use the exact required filenames in `Labs/Lab07/screenshots/`.
- [ ] My complete `DevOps/Labs/Lab07` directory is visible on GitHub after all tasks are complete.

## Submission

Submit the URL of your `DevOps/Labs/Lab07` directory after completing all five tasks and saving the required screenshots.

## Official References

- [Docker: Install the Docker Compose plug-in on Linux](https://docs.docker.com/compose/install/linux/)
- [Docker: Linux post-installation steps](https://docs.docker.com/engine/install/linux-postinstall/)
- [Docker: Run containers in the foreground or background](https://docs.docker.com/engine/containers/run/#foreground-and-background)
- [Docker: List running containers](https://docs.docker.com/reference/cli/docker/container/ls/)
- [Docker Hub: PostgreSQL Official Image](https://hub.docker.com/_/postgres)
- [Docker: Display Docker system information](https://docs.docker.com/reference/cli/docker/system/info/)
- [Docker: Search Docker Hub for images](https://docs.docker.com/reference/cli/docker/search/)
- [Docker: Restart a container](https://docs.docker.com/reference/cli/docker/container/restart/)
- [Docker: Remove an image](https://docs.docker.com/reference/cli/docker/image/rm/)
- [Docker Hub: NGINX Official Image](https://hub.docker.com/_/nginx)
- [Docker: View container logs](https://docs.docker.com/reference/cli/docker/container/logs/)
- [Docker: Attach to a running container](https://docs.docker.com/reference/cli/docker/container/attach/)
- [Docker: Run a command in a running container](https://docs.docker.com/reference/cli/docker/container/exec/)
- [Docker: View filesystem changes](https://docs.docker.com/reference/cli/docker/container/diff/)
- [Docker: Inspect a container](https://docs.docker.com/reference/cli/docker/container/inspect/)
- [Docker: Create an image from a container](https://docs.docker.com/reference/cli/docker/container/commit/)
