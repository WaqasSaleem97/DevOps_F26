# Lab 5: Build and Run Maven, Gradle, and Node.js Projects in IntelliJ IDEA

**Estimated duration:** 3 hours  
**IntelliJ setup repository:** [WaqasSaleem97/Intellij-Idea-docker](https://github.com/WaqasSaleem97/Intellij-Idea-docker)  
**Submission repository:** `DevOps`  
**Submission directory:** `Labs/Lab05`  
**Screenshot directory:** `Labs/Lab05/screenshots`

Clone the IntelliJ Docker repository on your local Ubuntu computer and launch its browser desktop. Use IntelliJ's **Clone Repository / Get from Version Control** action to download three projects, one at a time. Install and run the build tools from the **terminal inside IntelliJ IDEA**.

```text
DevOps/
└── Labs/
    └── Lab05/
        ├── README.md
        ├── Lab5_Solution.pdf
        └── screenshots/
```

The instructor supplies this README. Create your own screenshots directory and solution PDF.

## Lab Objective

By the end of this lab, you will be able to:

- Launch IntelliJ IDEA through the supplied Docker environment.
- Clone and open existing projects through IntelliJ.
- Install Maven, package a Java application and run its generated JAR.
- Use a project's Gradle Wrapper to build and run another Java application.
- Install Node.js and npm, download dependencies and start a Node.js application.
- Resolve a port conflict by changing the application port to 4000.

## Prerequisites

- Use the Ubuntu account and Git access prepared in the earlier labs.
- Docker Engine and Docker Compose are already installed on your local computer.
- Your host Ubuntu username matches your registered GitHub username, and the hostname is `ubuntu`.
- Your computer has a browser and a screenshot tool.
- Allow time and disk space for the first Docker image, IDE and dependency downloads.
- Use your normal Ubuntu login account to start the Docker environment.

The repository configures the IDE terminal prompt to display the host user's name with `@ubuntu`. The desktop's internal account is `abc`; use the supplied setup and your registered host login for the required prompt.

### Where to Run Commands

| Location | Commands used there |
| --- | --- |
| Ubuntu host terminal | Clone the IntelliJ Docker repository and run `setup.sh` |
| IntelliJ's Terminal tool window | Install Maven/Node.js, build the projects, run the JARs and use npm |

## Task List

- [Getting Started](#getting-started)
- [Task 1: Clone and Launch IntelliJ IDEA](#task-1-clone-and-launch-intellij-idea)
- [Task 2: Build and Run the Maven Project](#task-2-build-and-run-the-maven-project)
- [Task 3: Build and Run the Gradle Project](#task-3-build-and-run-the-gradle-project)
- [Task 4: Install and Run the Node.js Project](#task-4-install-and-run-the-nodejs-project)
- [Exam Evaluation Questions](#exam-evaluation-questions)
- [Summary](#summary)
- [Final Submission Checklist](#final-submission-checklist)
- [Submission](#submission)
- [Official References](#official-references)

---

## Mandatory Screenshot and Autograding Rules

Read these rules before starting. Each required screenshot is evaluated separately.

1. Before doing this lab, use an Ubuntu account whose terminal prompt shows your registered GitHub username before `@` and `ubuntu` as the hostname:

   ```text
   YourGitHubUsername@ubuntu:~$
   ```

   The comparison is case-insensitive, but spelling must match the `user_name` registered in the Student Marks Portal.

2. Every screenshot must clearly show:
   - `YourGitHubUsername@ubuntu` in the terminal prompt;
   - the command or GitHub operation required by that task; and
   - the relevant result or output.

3. For IntelliJ dialogs, Welcome screens and browser pages, keep a host Ubuntu terminal or IntelliJ terminal showing `YourGitHubUsername@ubuntu` visible in the same screenshot. For build and run commands, show IntelliJ's own Terminal tool window with that prompt and the relevant output.

4. Save every screenshot inside:

   ```text
   Labs/Lab05/screenshots/
   ```

5. Use the exact filename and capitalization given in this document. Do not rename files, add numbers, or use alternative spellings.

6. Screenshots must be readable by OCR. A missing, unclear, cropped, or unreadable screenshot receives zero for that check.

7. The required command and its relevant output must be visible. A screenshot containing only the command or only unrelated output may receive zero.

8. Exact and visually near-duplicate screenshots are compared across students for the same task. If two students submit the same or slightly modified screenshot, both receive zero for that task.

9. Do not expose passwords, private SSH keys, access tokens, recovery codes, or other secrets. It is acceptable to show a public-key entry or fingerprint without revealing private key contents.

10. Commit and push the complete `Labs/Lab05` directory to `DevOps` before the deadline. Files remaining only on your computer cannot be graded.

### Required Screenshot Filenames

These are the 21 required Lab 5 screenshot filenames:

```text
intellij_repo_clone.png
intellij_running.png
intellij_desktop.png
maven_project_clone.png
maven_installed.png
maven_package.png
maven_jar.png
maven_running.png
maven_stopped.png
maven_project_closed.png
gradle_project_clone.png
gradle_build.png
gradle_jar.png
gradle_running.png
gradle_stopped.png
gradle_project_closed.png
node_project_clone.png
nodejs_installed.png
npm_install.png
node_running.png
node_browser.png
```

Each required screenshot has equal weight. Exam-practice questions are not included in lab marks. Automated grading requires a configured Lab 5 grader.

---

## Getting Started

Create the screenshot directory in your existing `DevOps` repository:

```bash
cd ~/DevOps
mkdir -p Labs/Lab05/screenshots
```

Save the images there and include them in `Lab5_Solution.pdf` in task order.

---

## Task 1: Clone and Launch IntelliJ IDEA

1. In your **Ubuntu host terminal**, clone the supplied repository:

   ```bash
   cd ~
   git clone https://github.com/WaqasSaleem97/Intellij-Idea-docker.git
   cd Intellij-Idea-docker
   ```

   - Screenshot as `intellij_repo_clone.png`, showing the clone command and successful download.

2. Build and launch the environment:

   ```bash
   bash setup.sh
   docker compose ps
   ```

   Enter and confirm the browser password when prompted. The script builds the image and starts it with Docker Compose. The first build downloads the desktop and IDE.

   - Screenshot as `intellij_running.png`, showing the running `intellij` service and desktop port 3001. Keep passwords outside the screenshot.

3. Open **https://localhost:3001** in your host computer's browser. Acknowledge the self-signed certificate notice for this local desktop address and sign in with browser username **`admin`** and the password you chose during setup.

4. Launch **IntelliJ IDEA** from the browser desktop's applications menu. If needed, open the terminal in that browser desktop and run:

   ```bash
   idea
   ```

   Complete the first-run screens and reach the IntelliJ Welcome screen.

   - Screenshot as `intellij_desktop.png`, showing IntelliJ in the browser desktop and your Ubuntu host terminal beside it.

Create all three projects under **`/config/IdeaProjects`**. This directory is stored in the container's persistent `/config` volume.


**Screenshot Required:**

- `intellij_repo_clone.png`
- `intellij_running.png`
- `intellij_desktop.png`

---

## Task 2: Build and Run the Maven Project

1. On IntelliJ's Welcome screen, select **Clone Repository** or **Get from Version Control**. Enter:

   - **URL:** `https://gitlab.com/nanuchi/java-maven-app.git`
   - **Directory:** `/config/IdeaProjects/java-maven-app`

   - Screenshot as `maven_project_clone.png`, showing the repository URL and destination in the clone dialog.

   Click **Clone**, open/trust the course sample and wait for it to load. Use the Maven project configuration when asked. The project contains `pom.xml`.

2. Open **View → Tool Windows → Terminal** inside IntelliJ. Work from the Maven project's root directory. Run:

   ```bash
   sudo apt update
   sudo apt install -y maven
   mvn --version
   ```

   - Screenshot as `maven_installed.png`, showing the installed Maven version in IntelliJ's terminal.

3. Package the application:

   ```bash
   mvn package
   ```

   Wait for **`BUILD SUCCESS`**.

   - Screenshot as `maven_package.png`, showing the command and successful build.

4. In IntelliJ's Project pane, refresh the project if needed and expand **`target`**. Locate:

   ```text
   target/java-maven-app-1.1.0-SNAPSHOT.jar
   ```

   - Screenshot as `maven_jar.png`, showing the `target` directory and the exact JAR filename.

5. In the same IntelliJ terminal, run the JAR from the project root:

   ```bash
   java -jar target/java-maven-app-1.1.0-SNAPSHOT.jar
   ```

   Wait for the Spring Boot/Tomcat startup output. The sample runs as a web application on port 8080.

   - Screenshot as `maven_running.png`, showing the JAR command and successful application startup.

6. Press **Ctrl+C** in that terminal to stop the application. Wait for the shell prompt to return.

   - Screenshot as `maven_stopped.png`, showing `^C`/shutdown output and the returned prompt.

7. Select **File → Close Project** and return to IntelliJ's Welcome screen.

   - Screenshot as `maven_project_closed.png`. Use your Ubuntu host terminal beside the browser for identity evidence.


**Screenshot Required:**

- `maven_project_clone.png`
- `maven_installed.png`
- `maven_package.png`
- `maven_jar.png`
- `maven_running.png`
- `maven_stopped.png`
- `maven_project_closed.png`

**Summary:** Maven used `pom.xml` to build an executable JAR in `target`, and Java ran that JAR.

---

## Task 3: Build and Run the Gradle Project

1. From IntelliJ's Welcome screen, use **Clone Repository / Get from Version Control** again:

   - **URL:** `https://gitlab.com/nanuchi/java-app.git`
   - **Directory:** `/config/IdeaProjects/java-app`

   - Screenshot as `gradle_project_clone.png`, showing the URL and destination.

   Click **Clone**, open/trust the sample and load it as a Gradle project. It contains `build.gradle`, `settings.gradle` and `gradlew`.

2. Select the project's **Gradle Wrapper** when IntelliJ asks which Gradle installation to use.

3. Open **View → Tool Windows → Terminal**. From the Gradle project root, run:

   ```bash
   ./gradlew build
   ```

   The wrapper downloads the project's Gradle version on its first run. Wait for **`BUILD SUCCESSFUL`**.

   - Screenshot as `gradle_build.png`, showing the command and successful build.

   If the wrapper reports `Permission denied`, run `chmod +x gradlew` and retry `./gradlew build`.

4. In IntelliJ's Project pane, expand **`build` → `libs`**. Locate:

   ```text
   build/libs/java-app-1.0-SNAPSHOT.jar
   ```

   - Screenshot as `gradle_jar.png`, showing the output directory and exact JAR filename.

5. Run the generated JAR from the project root:

   ```bash
   java -jar build/libs/java-app-1.0-SNAPSHOT.jar
   ```

   Wait for successful application startup.

   - Screenshot as `gradle_running.png`, showing the command and startup output.

6. Press **Ctrl+C** and wait for the prompt to return. This also releases port 8080 before another application uses it.

   - Screenshot as `gradle_stopped.png`, showing the shutdown and returned prompt.

7. Select **File → Close Project**.

   - Screenshot as `gradle_project_closed.png`, showing the Welcome screen beside your Ubuntu host terminal.


**Screenshot Required:**

- `gradle_project_clone.png`
- `gradle_build.png`
- `gradle_jar.png`
- `gradle_running.png`
- `gradle_stopped.png`
- `gradle_project_closed.png`

**Summary:** The Gradle Wrapper built the executable JAR in `build/libs`, and Java ran it.

---

## Task 4: Install and Run the Node.js Project

1. Use IntelliJ's **Clone Repository / Get from Version Control** action:

   - **URL:** `https://gitlab.com/devopsbootcamp3/node-project.git`
   - **Directory:** `/config/IdeaProjects/node-project`

   - Screenshot as `node_project_clone.png`, showing the URL and destination.

   Clone and open the project. Its application files are in the `app` subdirectory.

2. Open the terminal **inside IntelliJ** and install Node.js together with npm:

   ```bash
   sudo apt update
   sudo apt install -y nodejs npm
   node --version
   npm --version
   ```

   - Screenshot as `nodejs_installed.png`, showing both installed versions.

3. From the project root, enter `app` and install its dependencies:

   ```bash
   cd app
   npm install
   ```

   The `app/package.json` file defines the dependencies and the start script.

   - Screenshot as `npm_install.png`, showing the command and completed installation.

4. Start the application:

   ```bash
   npm start
   ```

   The supplied `server.js` initially listens on port 3000. Webtop also uses that port inside this container, so the initial start may fail with **`EADDRINUSE`**.

5. If port 3000 fails, press **Ctrl+C** if the process is still active. Open **`app/server.js`** in IntelliJ and change the listener and its message to:

   ```javascript
   app.listen(4000, function () {
     console.log("app listening on port 4000!");
   });
   ```

   Save the file and run `npm start` again from the `app` directory. Use port **4000** for the final browser check inside the container desktop. Keep Webtop running while changing the application port.

   - Screenshot as `node_running.png`, showing `npm start` and `app listening on port 4000!`.

6. Keep the application running. Open a browser **inside the container's desktop** from its applications menu and visit **http://localhost:4000**. Verify that the Node.js project page and its profile images are visible.

   - Screenshot as `node_browser.png`, showing the page, its URL and your IntelliJ terminal in a split-screen view inside the container desktop.

7. After saving the evidence, stop the Node.js application with **Ctrl+C** in IntelliJ's terminal.


**Screenshot Required:**

- `node_project_clone.png`
- `nodejs_installed.png`
- `npm_install.png`
- `node_running.png`
- `node_browser.png`

**Summary:** npm installed the Node.js dependencies and used the start script to launch the application on the working port.

---

## Exam Evaluation Questions

These questions are **practice only and are not graded**. Put your answers in the solution PDF; no extra screenshots are required.

1. Maven built `java-maven-app-1.1.0-SNAPSHOT.jar`. Which values in `pom.xml` determine this name, and what does the `package` phase do?
2. Why can `./gradlew build` work without a separate system-wide Gradle installation? Which JDK settings would you check if this sample reports an unsupported Java version?
3. Why does `EADDRINUSE` occur on port 3000 in this environment, and how does changing the application listener to port 4000 let the container's browser open it?

## Summary

- You cloned and launched the supplied IntelliJ Docker environment.
- You used IntelliJ to clone three existing projects.
- You installed Maven and created an executable JAR under `target`.
- You used the Gradle Wrapper to create an executable JAR under `build/libs`.
- You ran both JARs with `java -jar`, stopped them with Ctrl+C and closed each project.
- You installed Node.js/npm, downloaded dependencies and started the Node.js application.
- You resolved the application-port conflict and viewed the Node.js page in the container's browser.

## Final Submission Checklist

- [ ] I used IntelliJ's integrated terminal for the build-tool and application commands.
- [ ] Maven reported `BUILD SUCCESS`, and Gradle reported `BUILD SUCCESSFUL`.
- [ ] I showed both exact JAR filenames in their generated output directories.
- [ ] I ran and stopped both JARs, then closed each Java project.
- [ ] I installed Node.js and npm and completed `npm install` in `node-project/app`.
- [ ] The Node.js application ran on port 4000 and its page opened in the container's browser.
- [ ] All 21 required screenshots use the exact filenames and show the required identity.
- [ ] No browser password, private SSH key or access token appears in my evidence.
- [ ] `Lab5_Solution.pdf` contains my name, registration number, course/section, GitHub username, task screenshots, captions and practice answers.
- [ ] My complete `DevOps/Labs/Lab05` submission is visible on GitHub.

## Submission

Upload **`Lab5_Solution.pdf`** and the original screenshots to your own `DevOps/Labs/Lab05` directory. Submit that directory's GitHub URL. Include task headings and a short caption explaining each result in the PDF.

After finishing the lab, you can stop the desktop from the **Ubuntu host terminal**:

```bash
cd ~/Intellij-Idea-docker
docker compose stop
```

Use `docker compose start` to resume that same container. Projects under `/config` persist; packages installed manually in the container need reinstalling if it is recreated.

## Official References

- [IntelliJ Docker repository: setup, access and persistence](https://github.com/WaqasSaleem97/Intellij-Idea-docker)
- [Maven sample project](https://gitlab.com/nanuchi/java-maven-app)
- [Gradle sample project](https://gitlab.com/nanuchi/java-app)
- [Node.js sample project](https://gitlab.com/devopsbootcamp3/node-project)
- [JetBrains: Clone a repository](https://www.jetbrains.com/help/idea/set-up-a-git-repository.html)
- [JetBrains: IntelliJ terminal](https://www.jetbrains.com/help/idea/terminal-emulator.html)
- [JetBrains: Configure SDKs and download a JDK](https://www.jetbrains.com/help/idea/sdk.html)
- [Maven: Build lifecycle and package phase](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)
- [Gradle: Java compatibility](https://docs.gradle.org/current/userguide/compatibility.html)
- [Gradle: The Wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html)
- [npm: Install dependencies](https://docs.npmjs.com/cli/v11/commands/npm-install/)
- [npm: Start scripts](https://docs.npmjs.com/cli/v11/commands/npm-start/)
- [LinuxServer Webtop: Desktop ports](https://docs.linuxserver.io/images/docker-webtop/)
