# Lab 8: Build Docker Images with Dockerfiles

**Execution environment:** Your assigned Ubuntu server  
**Submission repository:** `DevOps`  
**Submission directory:** `Labs/Lab08`  
**Screenshot directory:** `Labs/Lab08/screenshots`

Write two Dockerfiles using Ubuntu. First, build an image that prints a text file. Then build an image that serves a small HTML page on port `3000`.

```text
DevOps/
└── Labs/
    └── Lab08/
        ├── README.md
        ├── task1/
        │   ├── Dockerfile
        │   └── myfile
        ├── task2/
        │   ├── Dockerfile
        │   └── index.html
        └── screenshots/
            └── (your eight required screenshots)
```

The instructor supplies this README. Create the task files and screenshots yourself.

## Lab Objective

By the end of this lab, you will be able to:

- Use `FROM`, `LABEL`, `COPY`, `RUN`, `ADD`, `WORKDIR` and `CMD` in a Dockerfile.
- Build and run an image, then read a container's logs.
- Use `ENV` and `EXPOSE` and publish a container port with `-p`.
- Keep a container running by starting a web server as its main application.

## Prerequisites

- Complete Lab 7 and use Docker on your assigned Ubuntu server.
- Use your registered GitHub username as your Ubuntu username and `ubuntu` as your hostname.
- Have internet access for the Ubuntu image, package installation and the image downloaded by `ADD`.
- Have a text editor, browser and access to your existing `DevOps` repository.
- Port `3000` must be free on your Ubuntu server and reachable from your browser.

## Task List

- [Screenshot Rules](#screenshot-rules)
- [Required Screenshot Filenames](#required-screenshot-filenames)
- [Getting Started](#getting-started)
- [Task 1: Build an Ubuntu Image that Prints a File](#task-1-build-an-ubuntu-image-that-prints-a-file)
- [Task 2: Serve an HTML Page on Port 3000](#task-2-serve-an-html-page-on-port-3000)
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
3. Follow the screenshot instruction directly below each step. When a step asks for multiple commands in one screenshot, keep those commands and their relevant outputs together.
4. Save your screenshots inside `Labs/Lab08/screenshots/`.
5. Use the exact filenames and capitalization given below.
6. Keep the terminal text clear and readable.
7. Keep your screenshots unedited, with the required commands and their relevant output visible.
8. Do not expose real passwords, access tokens, private keys or other secrets.
9. Capture your own work. Do not copy or reuse another student's screenshots.
10. Commit and push your complete `Labs/Lab08` directory to your `DevOps` repository before the deadline.

### Required Screenshot Filenames

Lab 8 requires these 8 guided-task screenshots. Save them inside `Labs/Lab08/screenshots/` using the exact filenames below:

```text
task1_dockerfile.png
task1_image_build.png
task1_image_run.png
task1_container_logs.png
task2_web_files.png
task2_image_build.png
task2_container_running.png
task2_webpage.png
```

---

## Getting Started

In your existing `DevOps` repository, create the two task directories and the screenshot directory:

```bash
cd ~/DevOps
mkdir -p Labs/Lab08/task1 Labs/Lab08/task2 Labs/Lab08/screenshots
```

---

## Task 1: Build an Ubuntu Image that Prints a File

1. Prepare the build files:

   ```bash
   cd ~/DevOps/Labs/Lab08/task1
   echo "Hello from myfile" > myfile
   ```

   In the same directory, create a file named exactly `Dockerfile` with the following code. Replace the maintainer email with your own email address and use normal straight quotation marks:

   ```dockerfile
   FROM ubuntu
   LABEL maintainer="you@gmail.com"
   COPY myfile /app/
   RUN echo "my text" >> /app/myfile
   ADD https://www.python.org/static/community_logos/python-logo.png /app/
   WORKDIR /app
   CMD ["cat", "myfile"]
   ```

   `COPY` puts your local `myfile` into `/app/`. `RUN` appends a line while building the image. `ADD` downloads `python-logo.png` from Python's public website. This is a direct static image URL with no expiry token. `CMD` prints `myfile` when the container starts.

   - Screenshot as `task1_dockerfile.png`, showing the complete Dockerfile and your Ubuntu terminal prompt.

2. Build the image from this directory:

   ```bash
   docker build -t my-ubuntu:1.0 .
   ```

   - Screenshot as `task1_image_build.png`, showing the build command and its successful result.

3. List the images and run the new image. Give the container a name so it can be used in the next step:

   ```bash
   docker images
   docker run --name my-ubuntu-demo my-ubuntu:1.0
   ```

   The container prints:

   ```text
   Hello from myfile
   my text
   ```

   It then exits because `cat` has finished.

   - Screenshot as `task1_image_run.png`. Show both commands in **one screenshot**, including the `my-ubuntu` image with tag `1.0` and the printed file contents.

4. Read the output saved for that container:

   ```bash
   docker logs my-ubuntu-demo
   ```

   Use the container name `my-ubuntu-demo` here. `my-ubuntu:1.0` is the image tag. The logs show the same two lines, even though this container has exited.

   - Screenshot as `task1_container_logs.png`, showing the logs command and both lines of output.

**Screenshot Required:**

- `task1_dockerfile.png`
- `task1_image_build.png`
- `task1_image_run.png`
- `task1_container_logs.png`

---

## Task 2: Serve an HTML Page on Port 3000

1. Create the web page and its Dockerfile in a separate directory:

   ```bash
   cd ~/DevOps/Labs/Lab08/task2
   ```

   Create `index.html` containing just this one line:

   ```html
   <h1>Hello from Lab 8</h1>
   ```

   Create `Dockerfile` in the same directory:

   ```dockerfile
   FROM ubuntu
   LABEL maintainer="you@gmail.com"
   RUN apt-get update && apt-get install -y python3
   COPY index.html /app/
   WORKDIR /app
   ENV PORT=3000
   EXPOSE $PORT
   CMD ["sh", "-c", "exec python3 -m http.server \"$PORT\" --bind 0.0.0.0"]
   ```

   `ENV PORT=3000` sets the port variable and `EXPOSE $PORT` records the container's intended port. The square-bracket `CMD` uses `sh -c` to expand `$PORT`. `exec` starts Python's web server as the container's main process, serving the current `/app` directory. The server keeps running, so the container stays running.

   - Screenshot as `task2_web_files.png`, showing the one-line HTML file, the complete Dockerfile and your Ubuntu terminal prompt. Place the two files side by side in your editor if needed.

2. Build the second image:

   ```bash
   docker build -t my-ubuntu:1.1 .
   ```

   - Screenshot as `task2_image_build.png`, showing the build command and its successful result.

3. Start the web container in the background and check that it is running:

   ```bash
   docker run -d --name my-ubuntu-web -p 3000:3000 my-ubuntu:1.1
   docker ps
   ```

   Keep the image name `my-ubuntu:1.1` consistent between the build and run commands. `-d` returns control to your terminal while the web server runs. `-p 3000:3000` maps port `3000` on the Ubuntu server to port `3000` in the container. `EXPOSE` alone does not publish the port.

   - Screenshot as `task2_container_running.png`, showing both commands and the `my-ubuntu-web` container with an `Up` status and the `3000` port mapping.

4. In your browser, open `http://<server-ip>:3000`. Replace `<server-ip>` with the IP address of your Ubuntu server. Confirm that the page shows **Hello from Lab 8**.

   - Screenshot as `task2_webpage.png`, showing the page, the address bar with port `3000`, and your Ubuntu terminal prompt beside the browser.

**Screenshot Required:**

- `task2_web_files.png`
- `task2_image_build.png`
- `task2_container_running.png`
- `task2_webpage.png`

---

## Summary

- You built `my-ubuntu:1.0` and ran its default command to print a file.
- You downloaded a public image during the build and viewed the container logs by container name.
- You built `my-ubuntu:1.1` with a one-line HTML page and a web server.
- You published port `3000` and viewed the running container's webpage in a browser.

## Final Submission Checklist

- [ ] Task 1 contains my `Dockerfile` and `myfile`.
- [ ] Task 2 contains my `Dockerfile` and one-line `index.html`.
- [ ] Both Docker images built successfully with the required tags.
- [ ] The first container printed both lines and its logs show the same output.
- [ ] `my-ubuntu-web` is running and the webpage is visible on port `3000`.
- [ ] All eight screenshots use the exact filenames in `Labs/Lab08/screenshots/`.
- [ ] My complete `DevOps/Labs/Lab08` directory is visible on GitHub.

## Submission

Submit the URL of your own `DevOps/Labs/Lab08` directory after completing both tasks and pushing your task files and required screenshots.

## Official References

- [Docker: Dockerfile reference](https://docs.docker.com/reference/dockerfile/)
- [Docker: Container logs](https://docs.docker.com/reference/cli/docker/container/logs/)
- [Docker: Port publishing and mapping](https://docs.docker.com/engine/network/port-publishing/)
- [Python: Simple HTTP server](https://docs.python.org/3/library/http.server.html)
- [Python: Public logo image used by ADD](https://www.python.org/static/community_logos/python-logo.png)
