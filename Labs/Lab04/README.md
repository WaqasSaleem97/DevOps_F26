# Lab 4: Database Connections, Environment Variables, and Credential Files with Docker

**Estimated duration:** 3 hours  
**Starter repository:** [WaqasSaleem97/Databases](https://github.com/WaqasSaleem97/Databases) — fork this repository first  
**Submission repository:** `DevOps`  
**Submission directory:** `Labs/Lab04`  
**Screenshot directory:** `Labs/Lab04/screenshots`

Start by forking `Databases` into your own GitHub account. Run the application and databases from that fork on your Ubuntu server. Submit the screenshots, written answers and solution PDF to your separate GitHub repository named exactly `DevOps`.

```text
DevOps/
└── Labs/
    └── Lab04/
        ├── README.md
        ├── .gitignore
        ├── answers.md
        ├── evidence.json
        ├── Lab4_Solution.pdf
        └── screenshots/
            ├── lab04_folder_structure.png
            ├── ubuntu_identity.png
            ├── databases_fork.png
            └── ...

Databases/                       # Your separate fork and working directory
├── Dockerfile
├── compose.yaml
├── 01_mysql_hardcoded.py
├── 02_mongo_hardcoded.py
├── 03_mongo_env.py
├── 04_mongo_file.py
├── config-examples/
├── config/                      # Created locally; credential files stay ignored
├── db-init/
├── tests/
└── LAB_NOTES.md                  # Your own short practice record
```

The instructor supplies this README and the starter code. Create your own evidence and `Lab4_Solution.pdf`; the screenshots and completed solution are not supplied.

---

## Lab Objective

By the end of this lab, you will be able to:

- Connect a Python application to MySQL using hardcoded endpoint and credentials.
- Connect the application to MongoDB using hardcoded endpoint and credentials.
- Run unchanged MongoDB operations against dev, test and simulated prod using process environment variables.
- Run the same operations using external credential files selected for each environment.
- Verify the actual database target using a server-side environment marker.
- Distinguish configuration externalization from secure secret storage.
- Keep credential files out of Git while committing your code and evidence correctly.

The `prod` database is a disposable local simulation. Use only the supplied dummy accounts. Python runs inside the `app` container, so no Python packages need to be installed on the Ubuntu host.

## Prerequisites

- Complete the Ubuntu identity setup from Lab 1 and the Git/SSH setup from Lab 2.
- Your Ubuntu username matches the `user_name` registered in the Student Marks Portal, and the hostname is `ubuntu`.
- Git, Docker Engine and the Docker Compose plugin work on the Ubuntu server.
- Your own GitHub repositories `DevOps` and a fork of `Databases` are accessible through SSH.
- A browser, screenshot tool and document editor are available on your computer.
- Internet access is available for the first Docker image/package downloads.

If Docker is missing, follow the [official Docker Engine installation instructions for Ubuntu](https://docs.docker.com/engine/install/ubuntu/). Use your normal Ubuntu account with authorized Docker access. Keep the Ubuntu terminal visible when capturing evidence from Windows over SSH.

**Navigation:** Click any task below to jump to its instructions.

## Task List

- [Getting Started](#getting-started)
- [Task 1: Fork and Clone the Databases Repository](#task-1-fork-and-clone-the-databases-repository)
- [Task 2: Verify Docker and Build the Python Application](#task-2-verify-docker-and-build-the-python-application)
- [Task 3: Start and Inspect the Database Services](#task-3-start-and-inspect-the-database-services)
- [Task 4: Connect to MySQL with Hardcoded Settings](#task-4-connect-to-mysql-with-hardcoded-settings)
- [Task 5: Connect to MongoDB with Hardcoded Settings](#task-5-connect-to-mongodb-with-hardcoded-settings)
- [Task 6: Select Dev, Test, and Prod with Environment Variables](#task-6-select-dev-test-and-prod-with-environment-variables)
- [Task 7: Prove Database Isolation](#task-7-prove-database-isolation)
- [Task 8: Diagnose Authentication and Configuration Failures](#task-8-diagnose-authentication-and-configuration-failures)
- [Task 9: Select Dev, Test, and Prod with Credential Files](#task-9-select-dev-test-and-prod-with-credential-files)
- [Task 10: Run Configuration and Live Database Checks](#task-10-run-configuration-and-live-database-checks)
- [Task 11: Verify Git Exclusions and Compare the Approaches](#task-11-verify-git-exclusions-and-compare-the-approaches)
- [Task 12: Prepare the Solution PDF and Push Both Submissions](#task-12-prepare-the-solution-pdf-and-push-both-submissions)
- [Exam Evaluation Questions](#exam-evaluation-questions)
- [Summary](#summary)
- [Final Submission Checklist](#final-submission-checklist)
- [Submission](#submission)
- [Official References](#official-references)

---

## Mandatory Screenshot and Autograding Rules

Follow the same evidence rules used in Lab 2. Each of the 37 mandatory screenshots represents one equally weighted evidence check. `evidence.json` records these filenames for consistent manual or automated evaluation; a Lab 4 grader must be configured separately before automatic marking is available.

1. Use an Ubuntu account whose terminal prompt shows your registered GitHub username before `@` and `ubuntu` as the hostname:

   ```text
   YourGitHubUsername@ubuntu:~$
   ```

   Spelling must match your Student Marks Portal `user_name`; identity comparison is case-insensitive. A virtual-environment prefix or a current directory after the hostname is acceptable.

2. Every screenshot must clearly show your `YourGitHubUsername@ubuntu` terminal identity, the required command or operation, and the relevant result. Keep the Ubuntu host prompt visible around Docker commands; the Python container does not need to use your GitHub username internally.
3. For GitHub browser tasks, use split-screen so the GitHub page and the Ubuntu terminal identity appear in the same screenshot. Apply the same rule to the document title page and exported PDF.
4. Save original screenshots only under `DevOps/Labs/Lab04/screenshots/`, even when you execute commands inside your separate `Databases` fork.
5. Use the exact filenames and capitalization below. Do not append numbers, change extensions, or submit only images embedded in a PDF.
6. Screenshots must be clear and readable by OCR. Missing, unclear, cropped, misplaced or unreadable evidence receives zero for the affected check.
7. Show both the command and relevant output. A command alone or unrelated output is insufficient.
8. Exact and visually near-duplicate screenshots may be compared across students for the same task. Copied or slightly modified shared evidence receives zero for the affected task for both students.
9. Do not display passwords, private SSH keys, access tokens, recovery codes or secrets. Enter passwords using the hidden prompts. Use the supplied redaction commands before capturing source/configuration; do not edit screenshots afterwards to conceal or replace evidence. Public-key entries or fingerprints are acceptable when no private key is exposed.
10. Commit and push your code/practice notes to your `Databases` fork and the complete `Labs/Lab04` evidence directory to `DevOps` before the instructor's deadline. Local-only files cannot be graded.

### Required Automated-Grading Filenames

These are the 37 mandatory Lab 4 filenames. They define Lab 4 evidence; the Lab 2 Git screenshot filenames are not reused.

```text
lab04_folder_structure.png
ubuntu_identity.png
databases_fork.png
databases_clone.png
lab04_prerequisites.png
python_container.png
database_services.png
database_ports.png
mysql_hardcoded_settings.png
mysql_hardcoded_connection.png
mongo_hardcoded_settings.png
mongo_hardcoded_connection.png
mongo_env_dev.png
mongo_env_test.png
mongo_env_prod.png
mongo_env_unchanged.png
mongo_dev_only_record.png
mongo_test_isolation.png
mongo_wrong_password.png
mongo_missing_password.png
mongo_environment_mismatch.png
credential_files_created.png
credential_file_settings.png
credential_file_permissions.png
mongo_file_dev.png
mongo_file_test.png
mongo_file_prod.png
mongo_file_source_selection.png
lab04_tests.png
lab04_live_checks.png
credentials_gitignore.png
approach_comparison.png
lab04_code_push.png
solution_title_page.png
lab4_solution_pdf.png
lab04_git_push.png
lab04_github_submission.png
```

Exam-practice screenshots `Q1_source_selection.png`, `Q2_literal_password.png` and `Q3_cross_environment_auth.png` are additional evidence reviewed separately. They are not included in the 37 equally weighted checks.

---

## Getting Started

1. In your Ubuntu terminal, verify your existing identity:

   ```bash
   whoami
   hostname
   ```

   The username must match your registered GitHub username and the hostname must be `ubuntu`. Do not merely change the prompt text to imitate the required identity. Save `ubuntu_identity.png`.

2. Open your existing `DevOps` clone and create the submission directory:

   ```bash
   cd ~/DevOps
   mkdir -p Labs/Lab04/screenshots
   find Labs/Lab04 -maxdepth 2 -type d
   ```

   Save `lab04_folder_structure.png`. If `DevOps` has not been cloned on this server, first clone your own existing repository using the SSH procedure from Lab 2.

3. Copy the instructor-provided `README.md`, `.gitignore`, `answers.md` and `evidence.json` from this lab into `~/DevOps/Labs/Lab04/`. You can download the missing files with the following commands from `~/DevOps`; existing files are preserved:

   ```bash
   for lab_file in README.md .gitignore answers.md evidence.json
   do
     if [ ! -e "Labs/Lab04/${lab_file}" ]
     then
       curl --fail --location --silent --show-error "https://raw.githubusercontent.com/WaqasSaleem97/DevOps_F26/main/Labs/Lab04/${lab_file}" --output "Labs/Lab04/${lab_file}"
     fi
   done
   ```

   The lab's `.gitignore` allows `Lab4_Solution.pdf` even if your repository normally ignores PDF files. Keep the current README as the task instructions and write your answers in `answers.md`.

---

## Task 1: Fork and Clone the Databases Repository

### Steps

1. Open [WaqasSaleem97/Databases](https://github.com/WaqasSaleem97/Databases) and select **Fork**. Use your own GitHub account and keep the repository name `Databases`.
2. Confirm that the fork page shows `<your-username>/Databases` and identifies `WaqasSaleem97/Databases` as its source. Save split-screen evidence as `databases_fork.png`.
3. Clone your own fork, then configure the instructor's repository as `upstream`:

   ```bash
   cd ~
   git clone git@github.com:<your-username>/Databases.git
   cd Databases
   git remote add upstream https://github.com/WaqasSaleem97/Databases.git
   git remote -v
   ```

   Replace `<your-username>` with your actual GitHub username. If you already cloned your fork or configured `upstream`, reuse it instead of recreating it. `origin` must point to your fork. Save `databases_clone.png`.

📸 **Screenshot Required:** `databases_fork.png`, `databases_clone.png`

**Summary:** You have an independent code fork and a separate `DevOps` evidence repository.

---

## Task 2: Verify Docker and Build the Python Application

### Steps

1. Run from your `Databases` clone:

   ```bash
   cd ~/Databases
   git --version
   docker --version
   docker compose version
   docker info --format '{{.ServerVersion}}'
   ```

   Save `lab04_prerequisites.png` showing successful version checks and your terminal identity.

2. Match container writes to your Ubuntu user and build the Python image:

   ```bash
   export LAB_UID="$(id -u)"
   export LAB_GID="$(id -g)"
   docker compose config --quiet
   docker compose build app
   docker compose run --rm app python --version
   docker compose run --rm app python -m pip show pymongo mysql-connector-python
   ```

   Save `python_container.png` showing the container's Python version and the two installed drivers. Use the same Ubuntu shell for subsequent tasks so the UID/GID and profile variables remain available.

📸 **Screenshot Required:** `lab04_prerequisites.png`, `python_container.png`

**Summary:** The application runs inside Docker. Host Python package installation is unnecessary.

---

## Task 3: Start and Inspect the Database Services

### Steps

1. Start all four database containers and wait for healthy services:

   ```bash
   docker compose up -d --wait mysql mongo-dev mongo-test mongo-prod
   docker compose ps
   ```

   Save `database_services.png` showing all four databases healthy. The application is an on-demand tools service, so it does not need to appear as a background service.

2. Inspect the loopback-only published ports:

   ```bash
   docker compose port mysql 3306
   docker compose port mongo-dev 27017
   docker compose port mongo-test 27017
   docker compose port mongo-prod 27017
   ```

   Save `database_ports.png`.

| Environment | Endpoint from the app container | Host endpoint | Database / MongoDB auth source | Username | Disposable dummy password |
| --- | --- | --- | --- | --- | --- |
| MySQL dev | `mysql:3306` | `127.0.0.1:3307` | `school_mysql` | `mysql_student` | `MysqlDemoOnly123!` |
| MongoDB dev | `mongo-dev:27017` | `127.0.0.1:27017` | `school_dev` | `dev_user` | `DevDemoOnly123!` |
| MongoDB test | `mongo-test:27017` | `127.0.0.1:27018` | `school_test` | `test_user` | `TestDemoOnly123!` |
| MongoDB simulated prod | `mongo-prod:27017` | `127.0.0.1:27019` | `school_prod` | `prod_user` | `ProdDemoOnly123!` |

Use Docker service names inside the application container. The application's own `localhost` is not the MySQL or MongoDB container. Keep this password reference outside the captured evidence area.

If `--wait` is unavailable, update the Compose plugin or start with `docker compose up -d` and check `docker compose ps` until all services are healthy. Read `docker compose logs <service>` when troubleshooting. Initialization scripts run when database data volumes are first created.

📸 **Screenshot Required:** `database_services.png`, `database_ports.png`

---

## Task 4: Connect to MySQL with Hardcoded Settings

### Steps

1. Display the MySQL connection block with the password removed from the output before taking a screenshot:

   ```bash
   sed -E 's/(password[[:space:]]*=[[:space:]]*).*/\1"<redacted>",/' 01_mysql_hardcoded.py | sed -n '/mysql.connector.connect(/,/connection_timeout/p'
   ```

   Save `mysql_hardcoded_settings.png`. The file itself must keep the supplied dummy password so the example works.

2. Run the program:

   ```bash
   docker compose run --rm app python 01_mysql_hardcoded.py
   ```

   Save `mysql_hardcoded_connection.png` showing `mysql:3306`, `school_mysql`, the dev marker, saved/read student records and the PASS message.

3. In `answers.md`, identify the hardcoded settings and explain what must change if the endpoint or credentials change.

📸 **Screenshot Required:** `mysql_hardcoded_settings.png`, `mysql_hardcoded_connection.png`

**Summary:** The connection works, but deployment settings are embedded in application code.

---

## Task 5: Connect to MongoDB with Hardcoded Settings

### Steps

1. Display the MongoDB settings without revealing the password:

   ```bash
   sed -E 's/(password[[:space:]]*=[[:space:]]*).*/\1"<redacted>",/' 02_mongo_hardcoded.py | sed -n '/settings = MongoSettings(/,/^    )/p'
   ```

   Save `mongo_hardcoded_settings.png`.

2. Run the program:

   ```bash
   docker compose run --rm app python 02_mongo_hardcoded.py
   ```

   Save `mongo_hardcoded_connection.png` showing `mongo-dev:27017`, `school_dev`, marker `dev`, student records and PASS.

3. Open `mongo_demo.py` and `settings.py` in your editor. Notice that stages 2–4 all call the same `run_demo()` function. Explain why switching to MongoDB did not remove the hardcoded-configuration problem.

📸 **Screenshot Required:** `mongo_hardcoded_settings.png`, `mongo_hardcoded_connection.png`

---

## Task 6: Select Dev, Test, and Prod with Environment Variables

### Steps

1. Record the application file's checksum before changing any deployment variables:

   ```bash
   sha256sum 03_mongo_env.py > /tmp/lab04-env-before.sha256
   ```

2. Export the complete dev profile. At the hidden prompt enter the dev dummy password from Task 3:

   ```bash
   export APP_ENV=dev
   export MONGO_HOST=mongo-dev
   export MONGO_PORT=27017
   export MONGO_DATABASE=school_dev
   export MONGO_USERNAME=dev_user
   export MONGO_AUTH_SOURCE=school_dev
   read -r -s -p "Dev database password: " MONGO_PASSWORD
   echo
   export MONGO_PASSWORD
   docker compose run --rm app python 03_mongo_env.py
   ```

   Save `mongo_env_dev.png` showing the relevant command, `mongo-dev:27017`, `school_dev`, marker `dev`, and PASS.

3. Change the deployment variables to test and enter its password:

   ```bash
   export APP_ENV=test
   export MONGO_HOST=mongo-test
   export MONGO_PORT=27017
   export MONGO_DATABASE=school_test
   export MONGO_USERNAME=test_user
   export MONGO_AUTH_SOURCE=school_test
   read -r -s -p "Test database password: " MONGO_PASSWORD
   echo
   export MONGO_PASSWORD
   docker compose run --rm app python 03_mongo_env.py
   ```

   Save `mongo_env_test.png` showing the test endpoint, database, marker and PASS.

4. Change the complete profile to simulated prod:

   ```bash
   export APP_ENV=prod
   export MONGO_HOST=mongo-prod
   export MONGO_PORT=27017
   export MONGO_DATABASE=school_prod
   export MONGO_USERNAME=prod_user
   export MONGO_AUTH_SOURCE=school_prod
   read -r -s -p "Simulated prod database password: " MONGO_PASSWORD
   echo
   export MONGO_PASSWORD
   docker compose run --rm app python 03_mongo_env.py
   ```

   Save `mongo_env_prod.png` showing the simulated prod endpoint, database, marker and PASS.

5. Verify that the program stayed unchanged:

   ```bash
   sha256sum -c /tmp/lab04-env-before.sha256
   ```

   Save `mongo_env_unchanged.png` showing `03_mongo_env.py: OK`.

`APP_ENV` is a label and consistency check. It does not contain an automatic credentials map. Compose passes the named terminal variables through its `app.environment` configuration, and the Python process reads those values. Supply the complete matching profile. Do not print `env`, `printenv`, a credential-bearing URI or the password itself for evidence.

📸 **Screenshot Required:** `mongo_env_dev.png`, `mongo_env_test.png`, `mongo_env_prod.png`, `mongo_env_unchanged.png`

**Summary:** Deployment variables change the target database without edits to the application.

---

## Task 7: Prove Database Isolation

### Steps

1. From the host, open an interactive shell in the dev MongoDB service. Enter its dummy password at the hidden prompt:

   ```bash
   docker compose exec mongo-dev mongosh school_dev --username dev_user --authenticationDatabase school_dev
   ```

2. Run these commands in `mongosh`:

   ```javascript
   db.students.updateOne(
     { roll_no: "DEV-ONLY" },
     { $set: { name: "Dev Only Student", environment: "dev" } },
     { upsert: true }
   )
   db.students.find({ roll_no: "DEV-ONLY" }, { _id: 0 }).toArray()
   exit
   ```

   Return to the host and run:

   ```bash
   docker compose run --rm app python 02_mongo_hardcoded.py
   ```

   Save `mongo_dev_only_record.png` showing the host identity and `DEV-ONLY` in the dev program's output.

3. Repeat Task 6's complete **test** export block and run `03_mongo_env.py`. Save `mongo_test_isolation.png` showing the test marker, the seeded test student and absence of `DEV-ONLY` from the complete short student list.
4. Restore Task 6's complete **prod** profile before Task 8.

📸 **Screenshot Required:** `mongo_dev_only_record.png`, `mongo_test_isolation.png`

**Summary:** A record created in dev is absent from the separate test database.

---

## Task 8: Diagnose Authentication and Configuration Failures

Begin with the complete valid prod profile from Task 6.

### Steps

1. At the hidden prompt below enter an intentionally incorrect password, then run the same program:

   ```bash
   read -r -s -p "Enter an intentionally wrong password: " MONGO_PASSWORD
   echo
   export MONGO_PASSWORD
   docker compose run --rm app python 03_mongo_env.py
   ```

   Save `mongo_wrong_password.png` showing `Authentication failed`. The endpoint still points to the same database; the credentials are rejected.

2. Remove the password variable:

   ```bash
   unset MONGO_PASSWORD
   docker compose run --rm app python 03_mongo_env.py
   ```

   Save `mongo_missing_password.png` showing `Missing MongoDB settings: password`. The application must not use a fallback password or switch to dev.

3. Restore the valid prod password and deliberately change only the environment label:

   ```bash
   read -r -s -p "Restore the simulated prod password: " MONGO_PASSWORD
   echo
   export MONGO_PASSWORD
   export APP_ENV=dev
   docker compose run --rm app python 03_mongo_env.py
   ```

   Save `mongo_environment_mismatch.png` showing the database-marker error and `no student was written`. The endpoint and valid credentials still identify prod; the dev label does not reroute the connection.

4. Restore the label and confirm that the prod run succeeds:

   ```bash
   export APP_ENV=prod
   docker compose run --rm app python 03_mongo_env.py
   ```

📸 **Screenshot Required:** `mongo_wrong_password.png`, `mongo_missing_password.png`, `mongo_environment_mismatch.png`

---

## Task 9: Select Dev, Test, and Prod with Credential Files

### Steps

1. Create the editable local credential files:

   ```bash
   docker compose run --rm app python prepare_config.py
   ls config
   ```

   Save `credential_files_created.png` showing `dev.ini`, `test.ini` and `prod.ini`. Existing files are kept rather than overwritten.

2. Display each file's settings with its password removed from the terminal output:

   ```bash
   for profile in dev test prod
   do
     echo "--- ${profile}.ini ---"
     sed -E 's/^(password[[:space:]]*=).*/\1 <redacted>/' "config/${profile}.ini"
   done
   ```

   Save `credential_file_settings.png` showing the three selected profiles, endpoints, database names, usernames and redacted password fields. Make the terminal tall enough to keep the output and identity readable.

3. Check permissions:

   ```bash
   stat -c '%a %n' config/dev.ini config/test.ini config/prod.ini
   ```

   Save `credential_file_permissions.png`; newly prepared files should show Unix mode `600`.

4. Run file mode against dev:

   ```bash
   docker compose run --rm app python 04_mongo_file.py --env dev
   ```

   Save `mongo_file_dev.png` showing the external-file source and actual dev marker.

5. Run file mode against test:

   ```bash
   docker compose run --rm app python 04_mongo_file.py --env test
   ```

   Save `mongo_file_test.png` showing the test file and actual test marker.

6. Run file mode against simulated prod:

   ```bash
   docker compose run --rm app python 04_mongo_file.py --env prod
   ```

   Save `mongo_file_prod.png` showing the prod file and actual prod marker.

7. The shell should still hold the prod profile from Task 8. Display only non-secret identifiers and select the dev file:

   ```bash
   printf 'Shell profile: %s | %s | %s\n' "$APP_ENV" "$MONGO_HOST" "$MONGO_DATABASE"
   docker compose run --rm app python 04_mongo_file.py --env dev
   ```

   Save `mongo_file_source_selection.png` showing that the selected file connects to dev even though the shell describes prod. File mode deliberately ignores those shell credentials.

Python uses INI files through its standard-library `configparser`; Java commonly uses a properties file for an equivalent lesson. These INI files are plaintext. Disabling interpolation preserves literal `%` characters in passwords. Moving a secret to a file does not encrypt it. Do not commit the generated credential files.

📸 **Screenshot Required:** `credential_files_created.png`, `credential_file_settings.png`, `credential_file_permissions.png`, `mongo_file_dev.png`, `mongo_file_test.png`, `mongo_file_prod.png`, `mongo_file_source_selection.png`

---

## Task 10: Run Configuration and Live Database Checks

### Steps

1. Run the offline tests inside the Python image:

   ```bash
   docker compose run --rm app python -m unittest discover -s tests -v
   ```

   Save `lab04_tests.png` showing all 15 tests passing. These tests check configuration selection and mocked database boundaries; they do not replace live connections.

2. With all four databases healthy and credential files prepared, run the live smoke test:

   ```bash
   docker compose run --rm app python smoke_test.py
   ```

   Save `lab04_live_checks.png` showing all 11 live scenarios passing. It exercises the eight successful stage/environment combinations and three expected failures.

If a check fails, correct the actual configuration and rerun it. Keep the original dummy credentials in place for the supplied smoke test.

📸 **Screenshot Required:** `lab04_tests.png`, `lab04_live_checks.png`

---

## Task 11: Verify Git Exclusions and Compare the Approaches

### Steps

1. From the `Databases` root, check that credential copies are ignored and untracked:

   ```bash
   git check-ignore -v config/dev.ini config/test.ini config/prod.ini .env
   git ls-files config
   git status --short
   ```

   `git ls-files config` must print no credential-file paths. Save `credentials_gitignore.png`. Do not use `git add -f` for credential files. A `.gitignore` rule does not remove previously tracked files; if you accidentally tracked a local credential file, untrack it with `git rm --cached <path>` and handle any real exposed secret through rotation.

2. Complete `~/DevOps/Labs/Lab04/answers.md` with your identity, fork link, observed endpoints/markers, and the configuration comparison. Explain hardcoded values, environment injection, external files and production secret management. Include the results of the failure experiments.
3. Display your short comparison with the Ubuntu terminal identity visible. Save `approach_comparison.png`. Keep answers concise enough to read, and never include passwords.
4. In the `Databases` fork, create your own `LAB_NOTES.md` containing your name, GitHub username, a short description of the four stages, and the observed environment markers. Record no passwords or credential-bearing connection strings.

📸 **Screenshot Required:** `credentials_gitignore.png`, `approach_comparison.png`

**Summary:** External configuration makes deployments easier to change. Real secret handling also needs controlled access and rotation; neither variables nor plaintext files automatically provide that.

---

## Task 12: Prepare the Solution PDF and Push Both Submissions

### Steps

1. Commit your practice record and any intentional code changes to your own `Databases` fork:

   ```bash
   cd ~/Databases
   git status
   git add LAB_NOTES.md
   git commit -m "Document Lab 4 database configuration practice"
   git push origin main
   git log -1 --oneline
   ```

   If you changed starter code, review and stage the intended non-secret files as well. Save `lab04_code_push.png` showing a successful push to your fork. Add the fork's commit URL to `answers.md` in `DevOps`.

2. Create a document title page containing the lab number/title, your complete actual name, registration number, course code, section and GitHub username. Use a split-screen view with your Ubuntu identity visible and save `solution_title_page.png`.
3. Add headings for Tasks 1–11, then insert your own evidence screenshots in task order with a short caption for each. Include the Getting Started evidence, the observation table, your written comparisons and the exam-practice answers.
4. Export the document as exactly `Lab4_Solution.pdf` and place it at `~/DevOps/Labs/Lab04/Lab4_Solution.pdf`. Open the exported PDF, verify that all pages are readable, and save split-screen evidence as `lab4_solution_pdf.png`. Keep all original images in `screenshots/`. An editable Word copy may also be included; it does not replace the required PDF and `answers.md`.
5. Push the main evidence submission:

   ```bash
   cd ~/DevOps
   git status
   git add Labs/Lab04
   git commit -m "Submit Lab 04 database configuration evidence"
   git push origin main
   ```

   Save `lab04_git_push.png` showing the successful push to your own `DevOps` repository.
6. Open `<your-username>/DevOps/tree/main/Labs/Lab04` on GitHub. Check that `README.md`, `answers.md`, `Lab4_Solution.pdf` and the `screenshots` directory are present. Save split-screen evidence as `lab04_github_submission.png`.
7. Copy the final two screenshot files into `screenshots/`, then commit and push them too:

   ```bash
   cd ~/DevOps
   git add Labs/Lab04/screenshots
   git commit -m "Add Lab 04 final push and GitHub verification evidence"
   git push origin main
   git status
   ```

   Add Task 12 evidence/captions to the PDF and push the updated PDF after capturing these steps. Finally, confirm the complete submission is visible on GitHub. No additional screenshot is required for that last synchronization; it prevents the final evidence files or PDF from remaining local only.

📸 **Screenshot Required:** `lab04_code_push.png`, `solution_title_page.png`, `lab4_solution_pdf.png`, `lab04_git_push.png`, `lab04_github_submission.png`

---

## Exam Evaluation Questions

Complete these practical questions and write explanations in `answers.md` and the solution PDF. The three `Q*.png` images are reviewed separately from the 37 mandatory evidence checks.

### 1. Configuration Source Selection

1. Restore the complete valid test environment-variable profile from Task 6.
2. Predict the endpoints and markers for `03_mongo_env.py` and `04_mongo_file.py --env prod`.
3. Run both commands without changing the shell variables between them.
4. Save `Q1_source_selection.png` showing both configuration sources and observed targets.
5. Explain which source each entry point reads and why the results differ.

### 2. Special Characters in File Credentials

1. Run the file-reader tests without changing any database account:

   ```bash
   docker compose run --rm app python -m unittest discover -s tests -p test_config.py -v
   ```

2. Locate the passing `test_percent_password_is_read_literally_from_file` test and save `Q2_literal_password.png`.
3. Explain the effect of `ConfigParser(interpolation=None)` and why URI construction would require separate attention to credential encoding. Do not expose the test password in the screenshot.

### 3. Cross-Environment Credentials

1. Use the complete valid **test** profile from Task 6, then change only `MONGO_HOST` to `mongo-dev`.
2. Run `03_mongo_env.py` and save the authentication failure as `Q3_cross_environment_auth.png`.
3. Explain why the test account does not authenticate against the separate dev deployment.
4. Restore `MONGO_HOST=mongo-test`, confirm the test run succeeds, and record your restoration steps.

---

## Summary

- You forked and ran a Docker-based Python database application.
- You compared hardcoded MySQL and MongoDB connection settings.
- You used one unchanged MongoDB operation with three environment-variable profiles.
- You used separate credential files for the same three simulated environments.
- You verified the actual database using server-side markers and data isolation.
- You distinguished authentication, missing configuration and target mismatch errors.
- You excluded local credentials from Git and submitted your code fork and evidence repository correctly.

## Final Submission Checklist

- [ ] My GitHub username matches my registered portal identity and Ubuntu username.
- [ ] The hostname is `ubuntu`, and my required prompt is readable in every screenshot.
- [ ] My `Databases` repository is a fork of `WaqasSaleem97/Databases`.
- [ ] The application and databases were run through Docker Compose.
- [ ] MySQL and hardcoded MongoDB saved and read the demonstration student.
- [ ] Environment mode reached dev, test and simulated prod without editing the program.
- [ ] File mode reached dev, test and simulated prod using the selected file.
- [ ] I captured actual database markers and the isolation/failure experiments.
- [ ] All 15 offline tests and all 11 live scenarios passed on my machine.
- [ ] Generated credential files remain local and are excluded from Git.
- [ ] `LAB_NOTES.md` and intentional code changes are pushed to my `Databases` fork.
- [ ] All 37 mandatory screenshots use their exact filenames under `Labs/Lab04/screenshots/`.
- [ ] GitHub and document/PDF screenshots use split-screen identity evidence.
- [ ] No passwords, private keys or access tokens appear in my screenshots or answers.
- [ ] `answers.md` contains my observations, comparison, fork commit link and exam answers.
- [ ] I created `Lab4_Solution.pdf` with my title page, captions and evidence in task order.
- [ ] The final push/GitHub screenshots and updated PDF were committed and pushed too.
- [ ] The complete `Labs/Lab04` directory is visible on my own `DevOps` repository.

## Submission

Submit the URL of your own `DevOps/Labs/Lab04` directory as instructed by the instructor. Include your `Databases` fork URL and its final code/practice commit URL in `answers.md` and the PDF. The instructor repositories `DevOps_F26` and `WaqasSaleem97/Databases` are reference sources, not your personal submission destinations.

After grading evidence is saved, stop the database services from `~/Databases`:

```bash
docker compose down
```

For a fresh local lab only, `docker compose down -v` deletes this project's disposable database volumes. Do not use it if you intend to keep the lab data. Changing initialization passwords in Compose does not rotate existing database users; initialization runs on an empty data directory.

## Official References

- [MySQL Connector/Python: Connecting to MySQL](https://dev.mysql.com/doc/connector-python/en/connector-python-example-connecting.html)
- [MongoDB PyMongo: Create a MongoClient](https://www.mongodb.com/docs/languages/python/pymongo-driver/current/connect/mongoclient/)
- [MongoDB: SCRAM authentication and authSource](https://www.mongodb.com/docs/languages/python/pymongo-driver/current/security/authentication/scram/)
- [Docker: Set container environment variables](https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/)
- [Docker Compose profiles](https://docs.docker.com/reference/compose-file/profiles/)
- [Docker Official Image: MongoDB initialization](https://github.com/docker-library/docs/blob/master/mongo/README.md)
- [Docker Official Image: MySQL initialization](https://github.com/docker-library/docs/blob/master/mysql/README.md)
- [Python: configparser](https://docs.python.org/3/library/configparser.html)
- [OWASP: Secrets Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html)
- [Git: gitignore](https://git-scm.com/docs/gitignore)
