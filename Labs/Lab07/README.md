# Lab 7: Install Docker and Docker Compose

**Execution environment:** Your assigned Ubuntu server  
**Submission repository:** `DevOps`  
**Submission directory:** `Labs/Lab07`  
**Screenshot directory:** `Labs/Lab07/screenshots`

Install Docker and Docker Compose on your Ubuntu server if they are missing. If both tools are already installed, go directly to the version checks in Task 1.

```text
DevOps/
└── Labs/
    └── Lab07/
        ├── README.md
        └── screenshots/
            └── docker_installation.png
```

The instructor supplies this README. Create your own `screenshots/` directory in your `DevOps/Labs/Lab07` submission.

## Lab Objective

By the end of Task 1, you will be able to:

- Install Docker Engine and the Docker Compose plug-in on Ubuntu if needed.
- Check the installed Docker and Docker Compose versions.

## Prerequisites

- Use your assigned Ubuntu server and the account prepared in the earlier labs.
- Your Ubuntu username matches your registered GitHub username, and your hostname is `ubuntu`.
- Have `sudo` access and an internet connection if installation is needed.
- The Compose download command below is for an `x86_64` Ubuntu server.

## Task List

- [Screenshot Rules](#screenshot-rules)
- [Required Screenshot Filenames](#required-screenshot-filenames)
- [Getting Started](#getting-started)
- [Task 1: Install Docker and Docker Compose](#task-1-install-docker-and-docker-compose)
- [Task 2](#task-2)
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
3. Capture both version checks in one screenshot. No screenshots of the installation steps are required.
4. Save your screenshot inside `Labs/Lab07/screenshots/`.
5. Use the exact filename and capitalization given below.
6. Keep the terminal text clear and readable.
7. Keep the screenshot unedited, with both commands and their full version output visible.
8. Do not expose passwords, access tokens, private keys or other secrets.
9. Capture your own work. Do not copy or reuse another student's screenshot.
10. Commit and push your complete `Labs/Lab07` directory to your `DevOps` repository when all tasks are published and completed.

### Required Screenshot Filenames

Lab 7 currently requires one guided-task screenshot. Save it inside `Labs/Lab07/screenshots/` using the exact filename below:

```text
docker_installation.png
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

## Task 2

Continue

---

## Summary

- Docker and Docker Compose are installed or were already available on your Ubuntu server.
- You checked both versions and captured them in one screenshot.

## Final Submission Checklist

- [ ] Both version commands show their installed versions in my Ubuntu terminal.
- [ ] My `YourGitHubUsername@ubuntu` prompt and both command outputs are readable in `Labs/Lab07/screenshots/docker_installation.png`.
- [ ] My complete `DevOps/Labs/Lab07` directory is visible on GitHub after all tasks are complete.

## Submission

Save your Task 1 screenshot now. Submit the URL of your `DevOps/Labs/Lab07` directory after all Lab 7 tasks are published and completed.

## Official References

- [Docker: Install the Docker Compose plug-in on Linux](https://docs.docker.com/compose/install/linux/)
- [Docker: Linux post-installation steps](https://docs.docker.com/engine/install/linux-postinstall/)
