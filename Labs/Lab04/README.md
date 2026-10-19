# Lab 4: Simple Python Database Connections

**Estimated duration:** 2–3 hours  
**Starter repository:** [WaqasSaleem97/Databases](https://github.com/WaqasSaleem97/Databases) — fork this repository first  
**Submission repository:** `DevOps`  
**Submission directory:** `Labs/Lab04`  
**Screenshot directory:** `Labs/Lab04/screenshots`

Write and run Python directly on your assigned Ubuntu server. Docker runs the databases. Work in your own `Databases` fork and **edit the same file, `db_connect.py`, through all four stages**:

1. MySQL with the username, password and endpoint in the code.
2. MongoDB with the username, password and endpoint in the code.
3. MongoDB with values read using `os.getenv()`.
4. MongoDB with values read from a credentials file.

Each example only connects, sends a request and prints which database answered. In the MongoDB stages, the connection block stays the same; the source of two settings changes.

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

Databases/                     # Your separate fork
├── db_connect.py              # The only Python file you edit
├── credentials.ini.example   # Public dummy values supplied by the instructor
├── credentials.ini           # Local copy; ignored by Git
├── requirements.txt
├── compose.yaml
└── db-init/                   # Supplied database initialization
```

The instructor supplies the instructions and starter. Create your own screenshots, written answers and solution PDF. The `prod` environment is a disposable local simulation.

## Lab Objective

By the end of this lab, you will be able to:

- Recognize the Python equivalents of Java's MySQL and MongoDB connection code.
- Connect to MySQL and MongoDB using a short Python program.
- Explain the settings embedded in a MongoDB URI.
- Use `os.getenv()` to run unchanged Python code against dev, test and simulated prod.
- Use `configparser` to read those settings from an INI credentials file.
- Read the actual server's environment value to confirm the target.
- Keep local credentials out of Git and submit the required evidence.

## Prerequisites

- Use the Ubuntu account and Git/SSH access prepared in Labs 1 and 2.
- Your Ubuntu username matches your Student Marks Portal `user_name`, and the hostname is `ubuntu`.
- Your GitHub `DevOps` repository and your `Databases` fork are accessible through SSH.
- A browser, screenshot tool and document editor are available on your computer.
- Internet access is available for package and database image downloads.

Keep your Ubuntu terminal visible when capturing evidence from Windows over SSH.

**Navigation:** Click a task below to jump to its instructions.

## Task List

- [Getting Started](#getting-started)
- [Task 1: Fork and Clone the Databases Repository](#task-1-fork-and-clone-the-databases-repository)
- [Task 2: Install Docker, Docker Compose, and Python on Ubuntu](#task-2-install-docker-docker-compose-and-python-on-ubuntu)
- [Task 3: Start the Database Services](#task-3-start-the-database-services)
- [Task 4: MySQL with Values in the Code](#task-4-mysql-with-values-in-the-code)
- [Task 5: MongoDB with Values in the Code](#task-5-mongodb-with-values-in-the-code)
- [Task 6: MongoDB with os.getenv](#task-6-mongodb-with-osgetenv)
- [Task 7: MongoDB with a Credentials File](#task-7-mongodb-with-a-credentials-file)
- [Task 8: Compare the Approaches and Check Git Exclusions](#task-8-compare-the-approaches-and-check-git-exclusions)
- [Task 9: Prepare the Solution PDF and Push Both Submissions](#task-9-prepare-the-solution-pdf-and-push-both-submissions)
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

3. For GitHub web-interface tasks, use a split-screen view so the GitHub page and a terminal showing `YourGitHubUsername@ubuntu` are visible in the same screenshot.

4. Save every screenshot inside:

   ```text
   Labs/Lab04/screenshots/
   ```

5. Use the exact filename and capitalization given in this document. Do not rename files, add numbers, or use alternative spellings.

6. Screenshots must be readable by OCR. A missing, unclear, cropped, or unreadable screenshot receives zero for that check.

7. The required command and its relevant output must be visible. A screenshot containing only the command or only unrelated output may receive zero.

8. Exact and visually near-duplicate screenshots are compared across students for the same task. If two students submit the same or slightly modified screenshot, both receive zero for that task.

9. Do not expose passwords, private SSH keys, access tokens, recovery codes, or other secrets. It is acceptable to show a public-key entry or fingerprint without revealing private key contents.

10. Commit and push the complete `Labs/Lab04` directory to `DevOps` before the deadline. Files remaining only on your computer cannot be graded.

### Required Automated-Grading Filenames

These are the 29 required Lab 4 screenshot filenames:

```text
databases_fork.png
databases_clone.png
docker_installation.png
database_services.png
database_ports.png
mysql_hardcoded_settings.png
mysql_hardcoded_connection.png
mongo_hardcoded_settings.png
mongo_hardcoded_connection.png
mongo_env_settings.png
mongo_env_dev.png
mongo_env_test.png
mongo_env_prod.png
mongo_env_unchanged.png
credential_files_created.png
credential_file_settings.png
credential_file_permissions.png
mongo_file_settings.png
mongo_file_dev.png
mongo_file_test.png
mongo_file_prod.png
mongo_file_unchanged.png
credentials_gitignore.png
approach_comparison.png
lab04_code_push.png
solution_title_page.png
lab4_solution_pdf.png
lab04_git_push.png
lab04_github_submission.png
```

Each check has equal weight. The three `Q*.png` exam-practice screenshots are reviewed separately. The same filenames are recorded in `evidence.json`; automated grading requires a configured Lab 4 grader.

---

## Getting Started

1. Create the evidence directory in your existing `DevOps` repository:

   ```bash
   cd ~/DevOps
   mkdir -p Labs/Lab04/screenshots
   ```

2. Do the programming work in your `Databases` fork. Save evidence and answers in `DevOps/Labs/Lab04`.

---

## Task 1: Fork and Clone the Databases Repository

1. Open [WaqasSaleem97/Databases](https://github.com/WaqasSaleem97/Databases), select **Fork**, and keep the name `Databases`.
2. Confirm that the page shows `<your-username>/Databases` and the original repository as its source.
   - Take a split-screen screenshot as `databases_fork.png`.
3. Clone your own fork:

   ```bash
   cd ~
   git clone git@github.com:<your-username>/Databases.git
   cd Databases
   ```

   Replace `<your-username>` with your GitHub username. Reuse an existing clone if you already have one.
   - Screenshot as `databases_clone.png`.

**Screenshot Required:**

- `databases_fork.png`
- `databases_clone.png`

**Summary:** Your fork contains the code you will edit. Your separate `DevOps` repository holds the evidence.

---

## Task 2: Install Docker, Docker Compose, and Python on Ubuntu

1. Connect to your assigned Ubuntu server.

2. Update the Ubuntu package index and install Docker Engine and `curl`. On Ubuntu, the required package name is `docker.io`:

   ```bash
   sudo apt update -y
   sudo apt install -y docker.io curl
   ```

3. Create the system-wide Docker CLI plug-in directory and download the Docker Compose plug-in for an `x86_64` Ubuntu server:

   ```bash
   sudo mkdir -p /usr/local/lib/docker/cli-plugins
   sudo curl -SL https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64 -o /usr/local/lib/docker/cli-plugins/docker-compose
   sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
   ```

4. Start the Docker service and add your current Ubuntu account to the `docker` group:

   ```bash
   sudo systemctl start docker
   sudo usermod -aG docker $USER
   exit
   ```

   Completely sign out of the Ubuntu server and reconnect so that the new group membership takes effect. Opening only a new terminal tab is not sufficient.

5. Reconnect and verify the Git, Docker, and Docker Compose installations:

   ```bash
   ssh <github-username>@<ubuntu-server-ip>
   git --version
   docker --version
   docker compose version
   ```

   Do not continue until all three version commands display their installed versions.

   - Save a screenshot showing the three commands and their installed versions as `docker_installation.png`.

### Install Python and Prepare the Host Environment

```bash
sudo apt install -y python3 python3-venv python3-pip
cd ~/Databases
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

Edit `db_connect.py` directly in `~/Databases` using your editor. Run every `python` command below from this directory with the virtual environment active. If you open a new terminal, return to `~/Databases` and activate `.venv` again.

**Screenshot Required:**

- `docker_installation.png`

---

## Task 3: Start the Database Services

1. Start the four supplied databases:

   ```bash
   docker compose up -d --wait mysql mongo-dev mongo-test mongo-prod
   docker compose ps
   ```

   - Screenshot as `database_services.png`, showing all four services healthy.

2. Inspect the ports used by Python on the Ubuntu host:

   ```bash
   docker compose port mysql 3306
   docker compose port mongo-dev 27017
   docker compose port mongo-test 27017
   docker compose port mongo-prod 27017
   ```

   - Screenshot as `database_ports.png`.

| Database | Host endpoint | Database name | Dummy username | Dummy password |
| --- | --- | --- | --- | --- |
| MySQL | `127.0.0.1:3307` | `school_mysql` | `mysql_student` | `MysqlDemoOnly123!` |
| MongoDB dev | `127.0.0.1:27017` | `school_dev` | `dev_user` | `DevDemoOnly123!` |
| MongoDB test | `127.0.0.1:27018` | `school_test` | `test_user` | `TestDemoOnly123!` |
| MongoDB simulated prod | `127.0.0.1:27019` | `school_prod` | `prod_user` | `ProdDemoOnly123!` |

These public dummy values are supplied for the local lab. Keep this table outside screenshots. The initialization scripts create a small `environment_info` table/collection containing the actual environment name.


**Screenshot Required:**

- `database_services.png`
- `database_ports.png`

---

## Task 4: MySQL with Values in the Code

1. Open `db_connect.py` in your editor:

   ```bash
   nano db_connect.py
   ```

   The starter contains this short Python equivalent of the Java MySQL connection:

<!-- stage: mysql -->
```python
import mysql.connector

connection = mysql.connector.connect(
    host="127.0.0.1",
    port=3307,
    user="mysql_student",
    password="MysqlDemoOnly123!",
    database="school_mysql",
)

print("Connected to MySQL:", connection.database)
print("Server:", connection.server_host, connection.server_port)
cursor = connection.cursor()
cursor.execute("SELECT environment FROM environment_info")
print("Environment:", cursor.fetchone()[0])
cursor.close()
connection.close()
```

   `user` and `password` correspond to Java's `setUser()` and `setPassword()`. `host`, `port` and `database` correspond to the parts of the JDBC URL. `mysql.connector.connect()` opens the connection.

2. Display the code with the password hidden:

   ```bash
   sed -E 's/(password=).*/\1"<redacted>",/' db_connect.py
   ```

   - Screenshot as `mysql_hardcoded_settings.png`.

3. Run the file:

   ```bash
   python db_connect.py
   ```

   - Screenshot as `mysql_hardcoded_connection.png`.
     Show `Connected to MySQL: school_mysql`, server `127.0.0.1 3307`, and `Environment: dev`.

The small SQL query reads the environment value stored in MySQL. `cursor.close()` and `connection.close()` release the resources. Record the result in `answers.md`.


**Screenshot Required:**

- `mysql_hardcoded_settings.png`
- `mysql_hardcoded_connection.png`

**Summary:** The connection works, and its settings are written directly in Python.

---

## Task 5: MongoDB with Values in the Code

1. Replace the contents of **the same `db_connect.py`** with:

<!-- stage: mongo_hardcoded -->
```python
from pymongo import MongoClient

uri = "mongodb://dev_user:DevDemoOnly123!@127.0.0.1:27017/school_dev"
database_name = "school_dev"

client = MongoClient(uri, serverSelectionTimeoutMS=5000)
database = client[database_name]
database.command("ping")
print("Connected to MongoDB:", database.name)
print("Server:", client.address)
print("Environment:", database.environment_info.find_one()["name"])
client.close()
```

   `MongoClient(uri)` is the Python equivalent of creating the Java client from a MongoDB URI. `client[database_name]` selects the database, like `getDB(...)` in the Java example.

   In the URI, `dev_user` is the username, the value after `:` is the password, `127.0.0.1:27017` is the endpoint, and `school_dev` is the authentication database. This lab selects that same database for the request.

2. Show the code with the URI credentials hidden:

   ```bash
   sed -E 's#mongodb://[^@]*@#mongodb://<redacted>@#g' db_connect.py
   ```

   - Screenshot as `mongo_hardcoded_settings.png`.

3. Run the same command:

   ```bash
   python db_connect.py
   ```

   - Screenshot as `mongo_hardcoded_connection.png`.
     Show database `school_dev`, port `27017`, and environment `dev`.

`database.command("ping")` sends a real request; creating a client or selecting a database alone does not verify connectivity. The final query reads the environment from the selected database. `serverSelectionTimeoutMS=5000` limits server selection to five seconds.


**Screenshot Required:**

- `mongo_hardcoded_settings.png`
- `mongo_hardcoded_connection.png`

**Summary:** You changed the database driver while keeping the same run command.

---

## Task 6: MongoDB with os.getenv()

1. Replace the same file with the environment-variable version:

<!-- stage: mongo_env -->
```python
import os
from pymongo import MongoClient

uri = os.getenv("MONGO_URI")
database_name = os.getenv("MONGO_DATABASE")

if not uri or not database_name:
    raise SystemExit("Set MONGO_URI and MONGO_DATABASE first.")

client = MongoClient(uri, serverSelectionTimeoutMS=5000)
database = client[database_name]
database.command("ping")
print("Connected to MongoDB:", database.name)
print("Server:", client.address)
print("Environment:", database.environment_info.find_one()["name"])
client.close()
```

   Add `import os` and read the two settings using `os.getenv()`. Keep the MongoDB connection and output block unchanged. The short `if` statement tells you if a required variable is missing.

2. Display the code and record its checksum:

   ```bash
   cat db_connect.py
   sha256sum db_connect.py > /tmp/lab04-env-before.sha256
   ```

   - Screenshot as `mongo_env_settings.png`, showing `os.getenv()`.

3. Set the dev values in the same Ubuntu shell and run:

   ```bash
   export MONGO_URI='mongodb://dev_user:DevDemoOnly123!@127.0.0.1:27017/school_dev'
   export MONGO_DATABASE=school_dev
   clear
   python db_connect.py
   ```

   - Screenshot as `mongo_env_dev.png`, showing `school_dev`, port `27017`, and environment `dev`.

4. Set the test values and run:

   ```bash
   export MONGO_URI='mongodb://test_user:TestDemoOnly123!@127.0.0.1:27018/school_test'
   export MONGO_DATABASE=school_test
   clear
   python db_connect.py
   ```

   - Screenshot as `mongo_env_test.png`, showing `school_test`, port `27018`, and environment `test`.

5. Set the simulated prod values and run:

   ```bash
   export MONGO_URI='mongodb://prod_user:ProdDemoOnly123!@127.0.0.1:27019/school_prod'
   export MONGO_DATABASE=school_prod
   clear
   python db_connect.py
   ```

   - Screenshot as `mongo_env_prod.png`, showing `school_prod`, port `27019`, and environment `prod`.

6. Confirm that changing the environment variables did not change the code:

   ```bash
   sha256sum -c /tmp/lab04-env-before.sha256
   ```

   - Screenshot as `mongo_env_unchanged.png`, showing `db_connect.py: OK`.

These export commands contain the supplied public dummy credentials. `clear` removes them from the visible terminal before you capture the result. Do not print `MONGO_URI` or capture the exports in a screenshot.

Python inherits exported variables from the shell. The URI supplies the username, password and endpoint together; `MONGO_DATABASE` selects the database. Export both values for each environment. This version does not read `APP_ENV`.

Complete **Exam Question 1** while this version is still in your file.


**Screenshot Required:**

- `mongo_env_settings.png`
- `mongo_env_dev.png`
- `mongo_env_test.png`
- `mongo_env_prod.png`
- `mongo_env_unchanged.png`

**Summary:** One unchanged program connects to three databases using values from its process environment.

---

## Task 7: MongoDB with a Credentials File

1. Create one local file containing the three environment sections:

   ```bash
   cp -n credentials.ini.example credentials.ini
   ls credentials.ini
   ```

   - Screenshot as `credential_files_created.png`.

   The file contains:

```ini
# Public dummy values for the disposable classroom databases only.
[dev]
uri = mongodb://dev_user:DevDemoOnly123!@127.0.0.1:27017/school_dev
database = school_dev

[test]
uri = mongodb://test_user:TestDemoOnly123!@127.0.0.1:27018/school_test
database = school_test

[prod]
uri = mongodb://prod_user:ProdDemoOnly123!@127.0.0.1:27019/school_prod
database = school_prod
```

2. Show its settings with the URI credentials hidden:

   ```bash
   sed -E 's#mongodb://[^@]*@#mongodb://<redacted>@#g' credentials.ini
   ```

   - Screenshot as `credential_file_settings.png`, showing all three sections and their endpoints.

3. Restrict the file to your account and check its mode:

   ```bash
   chmod 600 credentials.ini
   stat -c '%a %n' credentials.ini
   ```

   - Screenshot as `credential_file_permissions.png`, showing `600 credentials.ini`.

4. Replace the contents of the same `db_connect.py` with:

<!-- stage: mongo_file -->
```python
import os
import configparser
from pymongo import MongoClient

config = configparser.ConfigParser(interpolation=None)
config.read("credentials.ini")
environment = os.getenv("APP_ENV", "dev")
uri = config[environment]["uri"]
database_name = config[environment]["database"]

client = MongoClient(uri, serverSelectionTimeoutMS=5000)
database = client[database_name]
database.command("ping")
print("Connected to MongoDB:", database.name)
print("Server:", client.address)
print("Environment:", database.environment_info.find_one()["name"])
client.close()
```

   The `configparser` module is included with Python. `config.read()` reads the file, and `config[environment]` chooses a section. `os.getenv("APP_ENV", "dev")` uses the selected section name, with `dev` as its default.

   The `uri` and `database_name` assignments read from that section. Keep the MongoDB connection and output block unchanged.

5. Display the Python code and record its checksum:

   ```bash
   cat db_connect.py
   sha256sum db_connect.py > /tmp/lab04-file-before.sha256
   ```

   - Screenshot as `mongo_file_settings.png`, showing the file-reading code.

6. Choose dev and run:

   ```bash
   export APP_ENV=dev
   python db_connect.py
   ```

   - Screenshot as `mongo_file_dev.png`, showing the dev endpoint, database and environment.

7. Choose test and run:

   ```bash
   export APP_ENV=test
   python db_connect.py
   ```

   - Screenshot as `mongo_file_test.png`, showing the test endpoint, database and environment.

8. Choose simulated prod and run:

   ```bash
   export APP_ENV=prod
   python db_connect.py
   ```

   - Screenshot as `mongo_file_prod.png`, showing the prod endpoint, database and environment.

9. Confirm that switching sections did not change the Python code:

   ```bash
   sha256sum -c /tmp/lab04-file-before.sha256
   ```

   - Screenshot as `mongo_file_unchanged.png`, showing `db_connect.py: OK`.

The file version reads `APP_ENV` to choose a section; it does not read `MONGO_URI` or `MONGO_DATABASE`. An INI file stores plaintext. `interpolation=None` preserves literal `%` characters in file values; credentials within a URI must still use valid URI encoding.

Complete **Exam Questions 2 and 3** after this task. Keep the file version as your final `db_connect.py`.


**Screenshot Required:**

- `credential_files_created.png`
- `credential_file_settings.png`
- `credential_file_permissions.png`
- `mongo_file_settings.png`
- `mongo_file_dev.png`
- `mongo_file_test.png`
- `mongo_file_prod.png`
- `mongo_file_unchanged.png`

**Summary:** The same connection code now reads its settings from an external file.

---

## Task 8: Compare the Approaches and Check Git Exclusions

1. Confirm that the local credentials file is ignored and untracked:

   ```bash
   git check-ignore -v credentials.ini
   git ls-files credentials.ini
   git status --short
   ```

   `git ls-files credentials.ini` must print nothing.
   - Screenshot as `credentials_gitignore.png`.

2. Complete `~/DevOps/Labs/Lab04/answers.md` with your identity, fork URL, eight connection observations and the comparison questions. Do not record passwords or credential-bearing URIs.
3. Display your short comparison with the required terminal identity visible.
   - Screenshot as `approach_comparison.png`.

| Stage | Where values come from | What changes for another environment |
| --- | --- | --- |
| MySQL hardcoded | Python connection arguments | Edit Python |
| MongoDB hardcoded | URI and database name in Python | Edit Python |
| MongoDB environment | `os.getenv()` | Export a different URI and database name |
| MongoDB file | An INI section | Select another section with `APP_ENV` |

Do not force-add `credentials.ini`. A Git ignore rule does not remove files that were already tracked. Plaintext files and environment variables do not encrypt credentials.


**Screenshot Required:**

- `credentials_gitignore.png`
- `approach_comparison.png`

---

## Task 9: Prepare the Solution PDF and Push Both Submissions

1. Review and push the final file version to your own fork:

   ```bash
   cd ~/Databases
   git status
   git add db_connect.py
   git commit -m "Complete Lab 4 Python database connection stages"
   git push origin main
   git log -1 --oneline
   ```

   - Screenshot as `lab04_code_push.png`, showing the successful push.
   - Add the fork's final commit URL to `answers.md`. Keep `credentials.ini` untracked.

2. Create a document title page with the lab number/title, complete actual name, registration number, course code, section and GitHub username.
   - Take a split-screen screenshot as `solution_title_page.png`.
3. Add task headings, your own screenshots in task order, short captions, the observation table, comparisons and exam answers. Include evidence of each version of `db_connect.py`.
4. Export exactly `Lab4_Solution.pdf` to `~/DevOps/Labs/Lab04/`. Open the PDF and verify its pages are readable.
   - Take a split-screen screenshot as `lab4_solution_pdf.png`.
5. Keep the original images in `screenshots/`, then push the evidence repository:

   ```bash
   cd ~/DevOps
   git status
   git add Labs/Lab04
   git commit -m "Submit Lab 04 Python database connections"
   git push origin main
   ```

   - Screenshot as `lab04_git_push.png`.
6. Open `<your-username>/DevOps/tree/main/Labs/Lab04` on GitHub. Confirm that the instructions, answers, PDF and screenshots are present.
   - Take a split-screen screenshot as `lab04_github_submission.png`.
7. Add the final push/GitHub screenshots and their PDF captions, then synchronize them:

   ```bash
   cd ~/DevOps
   git add Labs/Lab04
   git commit -m "Add Lab 04 final submission evidence"
   git push origin main
   git status
   ```

   Confirm the complete submission is visible on GitHub. No additional screenshot is required for this last synchronization.


**Screenshot Required:**

- `lab04_code_push.png`
- `solution_title_page.png`
- `lab4_solution_pdf.png`
- `lab04_git_push.png`
- `lab04_github_submission.png`

---

## Exam Evaluation Questions

Write your predictions, results and explanations in `answers.md` and the PDF. The three exam screenshots are reviewed separately from the 29 mandatory checks.

### 1. A Missing Environment Variable

Complete this during Task 6, before replacing the environment-variable code.

1. With the valid prod profile exported, remove only the database-name variable:

   ```bash
   unset MONGO_DATABASE
   python db_connect.py
   ```

   - Screenshot as `Q1_missing_environment.png`, showing `Set MONGO_URI and MONGO_DATABASE first.`.
2. Explain which `os.getenv()` call returned no value and why the `if` statement stopped the program.
3. Restore `export MONGO_DATABASE=school_prod` and confirm that the program connects again.

### 2. Which Source Does the File Version Read?

Complete this after Task 7.

1. Leave Task 6's prod values in `MONGO_URI` and `MONGO_DATABASE`. Remove only the section selector:

   ```bash
   unset APP_ENV
   python db_connect.py
   ```

   - Screenshot as `Q2_file_source_selection.png`, showing the dev connection.
2. Explain why this version connects to dev even though the shell still contains the prod MongoDB URI.
3. Select `APP_ENV=test` and verify the test connection.

### 3. A Wrong Password in the File

1. Open the local `credentials.ini`. In the `[test]` URI only, temporarily replace its dummy password with `wrong`. Keep the endpoint and database name unchanged.
2. Run:

   ```bash
   export APP_ENV=test
   python db_connect.py
   ```

   - Screenshot as `Q3_wrong_file_password.png`, showing the authentication error without displaying the file.
3. Explain why selecting the right endpoint does not make an incorrect password valid.
4. Restore the supplied test dummy password, run again successfully, then restore `APP_ENV=prod`. Do not commit the credential file.

---

## Summary

- You edited one Python file through four short connection examples.
- You connected to MySQL and MongoDB with values written in code.
- You used `os.getenv()` to connect to three MongoDB deployments without changing the program.
- You selected three sections in a credentials file using the same connection block.
- You verified each target using the database name, endpoint and stored environment value.
- You kept local credentials out of Git and pushed your code and evidence to the correct repositories.

## Final Submission Checklist

- [ ] My required Ubuntu terminal identity is readable in every screenshot.
- [ ] My `Databases` repository is my own fork.
- [ ] Python ran directly on Ubuntu; Docker ran only the databases.
- [ ] I edited the same `db_connect.py` through all four stages.
- [ ] I captured MySQL, hardcoded MongoDB, and dev/test/prod results for environment and file versions.
- [ ] The MongoDB connection block stayed unchanged when I changed configuration sources.
- [ ] My checksum screenshots show unchanged code while switching environments.
- [ ] My final `db_connect.py` uses the credentials-file version and is pushed to my fork.
- [ ] The local `credentials.ini` is ignored and untracked.
- [ ] All 29 required screenshots use the exact filenames in `screenshots/`.
- [ ] I completed the three exam questions and restored the valid dummy settings.
- [ ] No passwords, credential-bearing URIs, private keys or tokens appear in screenshots or answers.
- [ ] `answers.md` contains my identity, observations, comparisons and fork commit URL.
- [ ] `Lab4_Solution.pdf` contains my title page, captions, evidence and answers.
- [ ] My final screenshots and updated PDF are committed and pushed.
- [ ] My complete `DevOps/Labs/Lab04` directory is visible on GitHub.

## Submission

Submit the URL of your own `DevOps/Labs/Lab04` directory. Include your `Databases` fork URL and final code commit URL in the answers and PDF.

After capturing evidence, stop the databases:

```bash
cd ~/Databases
docker compose down
```

Initialization scripts run on empty database volumes. `docker compose down -v` intentionally deletes this project's disposable data; use it only for a fresh reset.

## Official References

- [MySQL Connector/Python: Connecting to MySQL](https://dev.mysql.com/doc/connector-python/en/connector-python-example-connecting.html)
- [MongoDB: Connect to MongoDB with PyMongo](https://www.mongodb.com/docs/languages/python/pymongo-driver/current/connect/)
- [Python: os.getenv](https://docs.python.org/3/library/os.html#os.getenv)
- [Python: configparser](https://docs.python.org/3/library/configparser.html)
- [Python: virtual environments](https://docs.python.org/3/library/venv.html)
- [Ubuntu: docker.io package](https://packages.ubuntu.com/noble/docker.io)
- [Docker: Install the Compose plugin manually](https://docs.docker.com/compose/install/linux/#install-the-plugin-manually)
- [Docker: Linux post-installation steps](https://docs.docker.com/engine/install/linux-postinstall/)
- [Git: gitignore](https://git-scm.com/docs/gitignore)
