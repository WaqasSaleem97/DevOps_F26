# Lab 6: Set Up Nexus Repository and Configure User Access

**Estimated duration:** 2 hours  
**Previous environment:** Continue using the IntelliJ IDEA Docker environment and Maven project from Lab 5  
**Submission repository:** `DevOps`  
**Submission directory:** `Labs/Lab06`  
**Screenshot directory:** `Labs/Lab06/screenshots`

This lab continues from Lab 5. Reopen the existing IntelliJ IDEA environment and the `java-maven-app` Maven project. Run the Nexus and verification commands from the **terminal inside IntelliJ IDEA**. Nexus Repository runs as a separate Docker container on the same Ubuntu Docker host.

```text
DevOps/
└── Labs/
    └── Lab06/
        ├── README.md
        ├── Lab6_Solution.pdf
        └── screenshots/
```

The instructor supplies this README. Create your own screenshots directory and solution PDF.

## Lab Objective

By the end of the currently published part of this lab, you will be able to:

- Reuse the IntelliJ IDEA environment and Maven project prepared in Lab 5.
- Run Nexus Repository in Docker with persistent storage.
- Complete the initial Nexus administrator setup without exposing credentials.
- Disable anonymous access.
- Create a Nexus role for the Maven snapshots repository.
- Create an active local user and assign the role.
- Verify the user's credentials through the Nexus Status API.

## Prerequisites

- Complete Lab 5 and keep the existing `Intellij-Idea-docker` environment.
- Keep the Lab 5 Maven project at `/config/IdeaProjects/java-maven-app`.
- Docker must be running, and the IntelliJ terminal must be able to run `docker ps`.
- Ports `8006` and `8007` must be available on the Ubuntu Docker host.
- Know the IP address of the Ubuntu computer or server on which Docker is running.
- Use a strong password for the Nexus `admin` account and a different strong password for the `nexus` user.

## Task List

- [Getting Started](#getting-started)
- [Task 1: Run Nexus Repository and Complete the Initial Setup](#task-1-run-nexus-repository-and-complete-the-initial-setup)
- [Task 2: Create a Role and User and Verify Access](#task-2-create-a-role-and-user-and-verify-access)
- [Task 3: Continue](#task-3-continue)
- [Summary](#summary)
- [Final Submission Checklist](#final-submission-checklist)
- [Submission](#submission)
- [Official References](#official-references)

---

## Mandatory Screenshot and Autograding Rules

Read these rules before starting. Each required screenshot is evaluated separately.

1. Use an Ubuntu account whose terminal prompt shows your registered GitHub username before `@` and `ubuntu` as the hostname:

   ```text
   YourGitHubUsername@ubuntu:~$
   ```

   The comparison is case-insensitive, but the spelling must match the `user_name` registered in the Student Marks Portal.

2. Every terminal screenshot must clearly show:
   - `YourGitHubUsername@ubuntu` in the prompt;
   - the required command; and
   - its relevant result or output.

3. For Nexus browser pages, keep the Nexus page and a terminal showing `YourGitHubUsername@ubuntu` visible in the same screenshot. Keep the required Nexus fields and results readable.

4. Save every screenshot inside:

   ```text
   Labs/Lab06/screenshots/
   ```

5. Use the exact filename and capitalization given in this document. Do not add numbers or alternative spellings.

6. Screenshots may use any image dimensions, but their text and evidence must be clear and readable.

7. A screenshot containing only a command, only filled but unsaved form fields, or unrelated output is not proof of completion.

8. Do not expose either Nexus password, the generated `admin.password` value, access tokens, private keys, or other secrets in a screenshot or solution file.

9. Exact and visually near-duplicate screenshots are compared across students for the same task. If two students submit the same or slightly modified screenshot, both may receive zero for that task.

10. Commit and push the complete `Labs/Lab06` directory to your `DevOps` repository before the deadline. Files that remain only on your computer cannot be graded.

### Required Screenshot Filenames for Tasks 1 and 2

The currently published part of Lab 6 requires these seven screenshots:

```text
nexus_container_running.png
nexus_login_page.png
nexus_setup_complete.png
nexus_role_created.png
nexus_role_privilege.png
nexus_user_created.png
nexus_user_access.png
```

Each required screenshot has equal weight. Task 3 may add more required screenshots when its instructions are published.

---

## Getting Started

1. Create the Lab 6 evidence directory in your existing `DevOps` repository:

   ```bash
   cd ~/DevOps
   mkdir -p Labs/Lab06/screenshots
   ```

2. Start the IntelliJ IDEA Docker environment created in Lab 5 from the Ubuntu host terminal:

   ```bash
   cd ~/Intellij-Idea-docker
   docker compose up -d
   docker compose ps
   ```

3. Open the browser desktop, launch IntelliJ IDEA, and reopen the existing Maven project:

   ```text
   /config/IdeaProjects/java-maven-app
   ```

4. Open **View → Tool Windows → Terminal** in IntelliJ IDEA. Run the remaining terminal commands in that terminal unless a step explicitly says to use the browser.

---

## Task 1: Run Nexus Repository and Complete the Initial Setup

### 1. Start the Nexus container

In the IntelliJ terminal, run:

```bash
docker run -d \
  --name nexus \
  --restart unless-stopped \
  -p 8006:8081 \
  -p 8007:8007 \
  -v nexus-data:/nexus-data \
  sonatype/nexus3:latest
```

This command creates a container named `nexus`. Host port `8006` opens the Nexus web interface, host port `8007` is reserved for a later repository task, and the named volume `nexus-data` keeps Nexus data when the container restarts.

If you previously created the container and it is stopped, do not run the creation command again. Start the existing container with:

```bash
docker start nexus
```

### 2. Verify that Nexus is running

Run:

```bash
docker ps | grep nexus
```

Wait until the output shows the `nexus` container. The first startup can take several minutes. If the browser is not ready yet, inspect startup progress with:

```bash
docker logs -f nexus
```

Press **Ctrl+C** after Nexus finishes starting. This stops only the log display; it does not stop the container.

- Screenshot as `nexus_container_running.png`, showing `docker ps | grep nexus`, the running `nexus` container, and the published ports.

### 3. Open the Nexus login page

On a computer that can reach the Ubuntu Docker host, open:

```text
http://<ubuntu-server-ip>:8006
```

Replace `<ubuntu-server-ip>` with the real IP address. The first login username is:

```text
admin
```

- Screenshot as `nexus_login_page.png`, showing the Nexus login page and the URL. Keep your required terminal prompt visible beside the browser. Do not enter or display a password in the screenshot.

### 4. Retrieve the one-time administrator password

Return to the IntelliJ terminal and run:

```bash
docker exec nexus cat /nexus-data/admin.password
```

Copy the generated value privately and use it once to sign in as `admin`. **Do not take a screenshot of this command or its output.** Clear the visible terminal immediately afterward:

```bash
clear
```

### 5. Complete the setup wizard

Complete the pages shown by Nexus in this order:

1. Select **Next** on **Let's Get You Set Up**.
2. Enter and confirm a new strong password for the `admin` user, then select **Next**.
3. Continue with **Nexus Repository Community Edition**.
4. Read the End User License Agreement, scroll as required, and select **Agree**.
5. On **Configure Anonymous Access**, select **Disable anonymous access**, then select **Next**.
6. On **Setup Complete**, select **Finish**.

- Screenshot as `nexus_setup_complete.png`, showing the **Setup Complete** page and the Nexus URL. Keep your required terminal prompt visible and do not show either password.

**Screenshot Required:**

- `nexus_container_running.png`
- `nexus_login_page.png`
- `nexus_setup_complete.png`

**Summary:** Nexus is running in Docker with persistent data, the initial administrator password has been replaced, and anonymous access is disabled.

---

## Task 2: Create a Role and User and Verify Access

### 1. Create the `nx-java` role

Sign in as `admin`, open **Settings**, and go to **Security → Roles**. Select **Create Role** and enter:

| Field | Value |
| --- | --- |
| Type | `Nexus role` |
| Role ID | `nx-java` |
| Role Name | `nx-java` |
| Role Description | `Maven snapshots role` |

Under **Applied Privileges**, select **Modify Applied Privileges**. Search for `maven2-maven-snapshots`, select this privilege, and confirm the selection:

```text
nx-repository-view-maven2-maven-snapshots-*
```

Save the role. Reopen the saved role if necessary to show its details.

- Screenshot as `nexus_role_created.png`, showing the saved role with Role ID `nx-java` and Role Name `nx-java`.
- Screenshot as `nexus_role_privilege.png`, showing `nx-repository-view-maven2-maven-snapshots-*` in the role's saved applied privileges.

### 2. Create the local `nexus` user

Go to **Settings → Security → Users**, select **Create local user**, and enter:

| Field | Value |
| --- | --- |
| ID | `nexus` |
| First name | `nexus` |
| Last name | `dev` |
| Email | `nexus@example.com` |
| Password | A new strong password chosen by you |
| Status | `Active` |
| Granted role | `nx-java` |

Move `nx-java` from **Available** to **Granted**, then select **Create local user**. Do not reuse the administrator password.

- Screenshot as `nexus_user_created.png`, showing the saved local user `nexus`, Active status, and the granted `nx-java` role. Do not show a password.

### 3. Verify the user's access

In the IntelliJ terminal, run the following command after replacing `<ubuntu-server-ip>` with the real IP address:

```bash
curl -i -u nexus http://<ubuntu-server-ip>:8006/service/rest/v1/status
```

When curl displays `Enter host password for user 'nexus':`, enter the password created in the previous step. The password is not displayed. Do not place the password after `nexus:` in the command because that would save it in shell history and may expose it in the screenshot.

A successful request returns an HTTP status line containing:

```text
HTTP/1.1 200 OK
```

- Screenshot as `nexus_user_access.png`, showing the curl command, the `HTTP/1.1 200` response, and your required terminal prompt. The password must not be visible.

If the response is `401 Unauthorized`, verify the user ID, password, and Active status. If repository access later returns `403 Forbidden`, verify that `nx-java` is granted to the user and contains the required Maven snapshots privilege.

**Screenshot Required:**

- `nexus_role_created.png`
- `nexus_role_privilege.png`
- `nexus_user_created.png`
- `nexus_user_access.png`

**Summary:** The local `nexus` user is active, receives Maven snapshots access through the `nx-java` role, and can authenticate to Nexus.

---

## Task 3: Continue

Additional Task 3 instructions will be added here.

---

## Summary

- You reused the IntelliJ IDEA and Maven environment from Lab 5.
- You ran Nexus Repository in a Docker container with a persistent named volume.
- You replaced the generated administrator password and disabled anonymous access.
- You created the `nx-java` role with Maven snapshots privileges.
- You created the active local `nexus` user and assigned the role.
- You verified the user through the Nexus Status API without exposing its password.

## Final Submission Checklist

- [ ] I continued using the IntelliJ IDEA environment and Maven project from Lab 5.
- [ ] The `nexus` container is running with ports `8006` and `8007` published.
- [ ] Nexus data uses the persistent `nexus-data` volume.
- [ ] I changed the generated `admin` password and disabled anonymous access.
- [ ] The saved `nx-java` role contains `nx-repository-view-maven2-maven-snapshots-*`.
- [ ] The local `nexus` user is Active and has the `nx-java` role.
- [ ] The authenticated Status API request returned HTTP 200.
- [ ] All seven currently required screenshots use the exact filenames in `Labs/Lab06/screenshots/`.
- [ ] My required Ubuntu terminal identity is readable in every screenshot.
- [ ] No password, generated `admin.password` value, private key, or token appears in any screenshot or solution file.
- [ ] `Lab6_Solution.pdf` contains my identity, captions, and the required evidence in task order.
- [ ] My complete `DevOps/Labs/Lab06` directory is visible on GitHub.

## Submission

Submit the URL of your own `DevOps/Labs/Lab06` directory after the remaining Lab 6 tasks are published and completed.

Do not delete the `nexus` container or the `nexus-data` volume after Tasks 1 and 2. Task 3 will continue using them.

## Official References

- [Sonatype: Install Self-Hosted Nexus Repository](https://help.sonatype.com/en/install-nexus-repository.html)
- [Sonatype: Nexus Repository Status API](https://help.sonatype.com/en/status-api.html)
- [Sonatype: Roles](https://help.sonatype.com/en/roles.html)
- [Sonatype: Users](https://help.sonatype.com/en/users.html)
- [Sonatype: Privileges](https://help.sonatype.com/en/privileges.html)
- [Sonatype: Anonymous Access](https://help.sonatype.com/en/anonymous-access.html)
- [Docker Hub: sonatype/nexus3](https://hub.docker.com/r/sonatype/nexus3)
