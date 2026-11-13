# Lab 7: Docker Setup and PostgreSQL Containers

**Execution environment:** Your assigned Ubuntu server  
**Submission repository:** `DevOps`  
**Submission directory:** `Labs/Lab07`  
**Screenshot directory:** `Labs/Lab07/screenshots`

Install Docker and Docker Compose on your Ubuntu server if they are missing. If both tools are already installed, go directly to the version checks in Task 1. Then use Docker to run two versions of PostgreSQL in separate containers.

```text
DevOps/
└── Labs/
    └── Lab07/
        ├── README.md
        └── screenshots/
            ├── docker_installation.png
            ├── postgres_missing_password.png
            ├── postgres_96_running.png
            └── postgres_both_versions.png
```

The instructor supplies this README. Create your own `screenshots/` directory in your `DevOps/Labs/Lab07` submission.

## Lab Objective

By the end of the published tasks, you will be able to:

- Install Docker Engine and the Docker Compose plug-in on Ubuntu if needed.
- Check the installed Docker and Docker Compose versions.
- Pull a PostgreSQL image by its tag and identify a missing initialization password.
- Start two PostgreSQL containers in the background and identify their image tags with `docker ps`.

## Prerequisites

- Use your assigned Ubuntu server and the account prepared in the earlier labs.
- Your Ubuntu username matches your registered GitHub username, and your hostname is `ubuntu`.
- Have internet access to pull the PostgreSQL images and `sudo` access if Docker installation is needed.
- The Compose download command below is for an `x86_64` Ubuntu server.

## Task List

- [Screenshot Rules](#screenshot-rules)
- [Required Screenshot Filenames](#required-screenshot-filenames)
- [Getting Started](#getting-started)
- [Task 1: Install Docker and Docker Compose](#task-1-install-docker-and-docker-compose)
- [Task 2: Run Two PostgreSQL Versions in Docker](#task-2-run-two-postgresql-versions-in-docker)
- [Task 3](#task-3)
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

2. Show your `YourGitHubUsername@ubuntu` prompt, the required commands and their output.
3. In Task 1, capture both version checks in one screenshot. No screenshots of the installation steps are required. In Task 2, capture the evidence listed under the task.
4. Save your screenshots inside `Labs/Lab07/screenshots/`.
5. Use the exact filenames and capitalization given below.
6. Keep the terminal text clear and readable.
7. Keep your screenshots unedited, with the required commands and their relevant output visible.
8. Do not expose real passwords, access tokens, private keys or other secrets. The supplied example value `password` may appear in Task 2 evidence.
9. Capture your own work. Do not copy or reuse another student's screenshots.
10. Commit and push your complete `Labs/Lab07` directory to your `DevOps` repository when all tasks are published and completed.

### Required Screenshot Filenames

Lab 7 currently requires these 4 guided-task screenshots. Save them inside `Labs/Lab07/screenshots/` using the exact filenames below:

```text
docker_installation.png
postgres_missing_password.png
postgres_96_running.png
postgres_both_versions.png
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

**Screenshot Required:**

- `docker_installation.png` — one screenshot showing `docker --version`, `docker compose version`, both outputs and your Ubuntu terminal prompt.

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

   This attempt is expected to exit with an error because `POSTGRES_PASSWORD` was not supplied for the new database. Capture the command and the error before continuing.

3. Start a new PostgreSQL 9.6 container with the password set. The `-d` flag runs it in the background and returns control to your terminal:

   ```bash
   docker run -d -e POSTGRES_PASSWORD=password postgres:9.6
   ```

4. List the running containers:

   ```bash
   docker ps
   ```

   Check that `postgres:9.6` appears in the `IMAGE` column and its status is `Up`.

5. Start a PostgreSQL 10.10 container in the background:

   ```bash
   docker run -d -e POSTGRES_PASSWORD=password postgres:10.10
   ```

6. List the running containers again:

   ```bash
   docker ps
   ```

   Confirm that `postgres:9.6` and `postgres:10.10` both appear in the `IMAGE` column with an `Up` status.

**Screenshot Required:**

- `postgres_missing_password.png` — show `docker run postgres:9.6` and the missing-password error.
- `postgres_96_running.png` — show the first `docker ps` command and the running `postgres:9.6` container.
- `postgres_both_versions.png` — show the final `docker ps` command and both running PostgreSQL containers.

---

## Task 3

Continue

---

## Summary

- Docker and Docker Compose are installed or were already available on your Ubuntu server.
- You checked both tool versions and captured them in one screenshot.
- You observed the PostgreSQL initialization error, supplied the password, and confirmed that both PostgreSQL versions were running.

## Final Submission Checklist

- [ ] Both version commands show their installed versions in my Ubuntu terminal.
- [ ] My `YourGitHubUsername@ubuntu` prompt and both command outputs are readable in `Labs/Lab07/screenshots/docker_installation.png`.
- [ ] I captured the missing-password error, the running PostgreSQL 9.6 container, and both containers running together.
- [ ] All four screenshots use the exact required filenames in `Labs/Lab07/screenshots/`.
- [ ] My complete `DevOps/Labs/Lab07` directory is visible on GitHub after all tasks are complete.

## Submission

Save your Task 1 and Task 2 screenshots now. Submit the URL of your `DevOps/Labs/Lab07` directory after all Lab 7 tasks are published and completed.

## Official References

- [Docker: Install the Docker Compose plug-in on Linux](https://docs.docker.com/compose/install/linux/)
- [Docker: Linux post-installation steps](https://docs.docker.com/engine/install/linux-postinstall/)
- [Docker: Run containers in the foreground or background](https://docs.docker.com/engine/containers/run/#foreground-and-background)
- [Docker: List running containers](https://docs.docker.com/reference/cli/docker/container/ls/)
- [Docker Hub: PostgreSQL Official Image](https://hub.docker.com/_/postgres)
