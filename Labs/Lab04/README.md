# Lab 4: Simple Python Database Connections

**Estimated duration:** 3 hours  
**Starter repository:** [Databases](https://github.com/WaqasSaleem97/Databases) — fork this repository first  
**Submission repository:** `DevOps`  
**Submission directory:** `Labs/Lab04`  
**Screenshot directory:** `Labs/Lab04/screenshots`

Write and run Python directly on your assigned Ubuntu server. Docker runs the databases. Work in your own `Databases` fork and **edit the same file, `db_connect.py`, through all four stages**:

1. MySQL with the username, password and endpoint in the code.
2. MongoDB with the username, password and endpoint in the code.
3. MongoDB with values read using `os.getenv()`.
4. MongoDB with values read from a credentials file.

Each example only connects, sends a request and prints which database answered. In the MongoDB stages, the connection block stays the same; the source of the username, password, endpoint and database name changes.

```text
DevOps/
└── Labs/
    └── Lab04/
        ├── README.md
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

The instructor supplies this README and the starter repository. Create your own `screenshots/` directory in your `DevOps/Labs/Lab04` submission and include your written answers in `Lab4_Solution.pdf`. The `prod` environment is a disposable local simulation.

## Lab Objective

By the end of this lab, you will be able to:

- Explain the username, password, endpoint and database name used for a connection.
- Connect to MySQL and MongoDB using a short Python program.
- Explain the settings embedded in a MongoDB URI.
- Use `os.getenv()` to run unchanged Python code against dev, test and simulated prod.
- Use `configparser` to read those settings from an INI credentials file.
- Read the actual server's environment value to confirm the target.
- Capture readable evidence of the database connections.

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

These are the 15 required Lab 4 screenshot filenames:

```text
databases_fork.png
databases_clone.png
docker_installation.png
database_services.png
database_ports.png
mysql_hardcoded_connection.png
mongo_hardcoded_connection.png
mongo_env_dev.png
mongo_env_test.png
mongo_env_prod.png
credential_files_created.png
credential_file_permissions.png
mongo_file_dev.png
mongo_file_test.png
mongo_file_prod.png
```

Each check has equal weight. The three `Q*.png` exam-practice screenshots are reviewed separately. Automated grading requires a configured Lab 4 grader.

---

## Getting Started

1. Create the evidence directory in your existing `DevOps` repository:

   ```bash
   cd ~/DevOps
   mkdir -p Labs/Lab04/screenshots
   ```

2. Do the programming work in your `Databases` fork. Save screenshots in `DevOps/Labs/Lab04/screenshots/` and include your observations and answers in `Lab4_Solution.pdf`.

---

## Task 1: Fork and Clone the Databases Repository

1. Open [Databases](https://github.com/WaqasSaleem97/Databases), select **Fork**, and keep the name `Databases`.
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

   The starter contains this short MySQL connection:

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

   `user` and `password` supply the database credentials. `host` identifies the server, `port` identifies its MySQL service, and `database` selects the database. `mysql.connector.connect()` opens the connection.

2. Run the file:

   ```bash
   python db_connect.py
   ```

   - Screenshot as `mysql_hardcoded_connection.png`.
     Show `Connected to MySQL: school_mysql`, server `127.0.0.1 3307`, and `Environment: dev`.

The small SQL query reads the environment value stored in MySQL. `cursor.close()` and `connection.close()` release the resources. Record the result in your solution PDF.


**Screenshot Required:**

- `mysql_hardcoded_connection.png`

**Summary:** The connection works, and its settings are written directly in Python.

---

## Task 5: MongoDB with Values in the Code

1. Replace the contents of **the same `db_connect.py`** with:

<!-- stage: mongo_hardcoded -->
```python
from pymongo import MongoClient

username = "dev_user"
password = "DevDemoOnly123!"
endpoint = "127.0.0.1:27017"
database_name = "school_dev"

uri = "mongodb://" + username + ":" + password + "@" + endpoint + "/" + database_name

client = MongoClient(uri, serverSelectionTimeoutMS=5000)
database = client[database_name]
database.command("ping")
print("Connected to MongoDB:", database.name)
print("Server:", client.address)
print("Environment:", database.environment_info.find_one()["name"])
client.close()
```

   `MongoClient(uri)` creates the MongoDB client, and `client[database_name]` selects the database.

   `username`, `password`, `endpoint` and `database_name` hold the settings separately. The `+` operator joins them into `uri`. The URI's database is also the authentication database in this lab.

2. Run the same command:

   ```bash
   python db_connect.py
   ```

   - Screenshot as `mongo_hardcoded_connection.png`.
     Show database `school_dev`, port `27017`, and environment `dev`.

`database.command("ping")` sends a real request; creating a client or selecting a database alone does not verify connectivity. The final query reads the environment from the selected database. `serverSelectionTimeoutMS=5000` limits server selection to five seconds.


**Screenshot Required:**

- `mongo_hardcoded_connection.png`

**Summary:** You changed the database driver while keeping the same run command.

---

## Task 6: MongoDB with os.getenv()

1. Replace the same file with the environment-variable version:

<!-- stage: mongo_env -->
```python
import os
from pymongo import MongoClient

username = os.getenv("MONGO_USERNAME")
password = os.getenv("MONGO_PASSWORD")
endpoint = os.getenv("MONGO_ENDPOINT")
database_name = os.getenv("MONGO_DATABASE")

if not username or not password or not endpoint or not database_name:
    raise SystemExit("Set all four MongoDB environment variables first.")

uri = "mongodb://" + username + ":" + password + "@" + endpoint + "/" + database_name

client = MongoClient(uri, serverSelectionTimeoutMS=5000)
database = client[database_name]
database.command("ping")
print("Connected to MongoDB:", database.name)
print("Server:", client.address)
print("Environment:", database.environment_info.find_one()["name"])
client.close()
```

   Add `import os` and read the username, password, endpoint and database name separately using `os.getenv()`. Keep the URI concatenation, MongoDB connection and output block unchanged. The short `if` statement tells you if a required variable is missing.

2. Set the dev values in the same Ubuntu shell and run:

   ```bash
   export MONGO_USERNAME=dev_user
   export MONGO_PASSWORD='DevDemoOnly123!'
   export MONGO_ENDPOINT=127.0.0.1:27017
   export MONGO_DATABASE=school_dev
   clear
   python db_connect.py
   ```

   - Screenshot as `mongo_env_dev.png`, showing `school_dev`, port `27017`, and environment `dev`.

3. Set the test values and run:

   ```bash
   export MONGO_USERNAME=test_user
   export MONGO_PASSWORD='TestDemoOnly123!'
   export MONGO_ENDPOINT=127.0.0.1:27018
   export MONGO_DATABASE=school_test
   clear
   python db_connect.py
   ```

   - Screenshot as `mongo_env_test.png`, showing `school_test`, port `27018`, and environment `test`.

4. Set the simulated prod values and run:

   ```bash
   export MONGO_USERNAME=prod_user
   export MONGO_PASSWORD='ProdDemoOnly123!'
   export MONGO_ENDPOINT=127.0.0.1:27019
   export MONGO_DATABASE=school_prod
   clear
   python db_connect.py
   ```

   - Screenshot as `mongo_env_prod.png`, showing `school_prod`, port `27019`, and environment `prod`.

These export commands contain the supplied public dummy credentials. `clear` removes them from the visible terminal before you capture the result. Do not print `MONGO_PASSWORD`, the assembled URI or capture the exports in a screenshot.

Python inherits exported variables from the shell. `MONGO_USERNAME`, `MONGO_PASSWORD`, `MONGO_ENDPOINT` and `MONGO_DATABASE` supply separate values. Python joins them into the connection URI. Export all four matching values for each environment. This version does not read `APP_ENV`.

Complete **Exam Question 1** while this version is still in your file.


**Screenshot Required:**

- `mongo_env_dev.png`
- `mongo_env_test.png`
- `mongo_env_prod.png`

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
username = dev_user
password = DevDemoOnly123!
endpoint = 127.0.0.1:27017
database = school_dev

[test]
username = test_user
password = TestDemoOnly123!
endpoint = 127.0.0.1:27018
database = school_test

[prod]
username = prod_user
password = ProdDemoOnly123!
endpoint = 127.0.0.1:27019
database = school_prod
```

2. Restrict the file to your account and check its mode:

   ```bash
   chmod 600 credentials.ini
   stat -c '%a %n' credentials.ini
   ```

   - Screenshot as `credential_file_permissions.png`, showing `600 credentials.ini`.

3. Replace the contents of the same `db_connect.py` with:

<!-- stage: mongo_file -->
```python
import os
import configparser
from pymongo import MongoClient

config = configparser.ConfigParser(interpolation=None)
config.read("credentials.ini")
environment = os.getenv("APP_ENV", "dev")
username = config[environment]["username"]
password = config[environment]["password"]
endpoint = config[environment]["endpoint"]
database_name = config[environment]["database"]

uri = "mongodb://" + username + ":" + password + "@" + endpoint + "/" + database_name

client = MongoClient(uri, serverSelectionTimeoutMS=5000)
database = client[database_name]
database.command("ping")
print("Connected to MongoDB:", database.name)
print("Server:", client.address)
print("Environment:", database.environment_info.find_one()["name"])
client.close()
```

   The `configparser` module is included with Python. `config.read()` reads the file, and `config[environment]` chooses a section. `os.getenv("APP_ENV", "dev")` uses the selected section name, with `dev` as its default.

   The `username`, `password`, `endpoint` and `database_name` assignments read separate values from that section. Python builds `uri` with the same concatenation line used before. Keep the MongoDB connection and output block unchanged.

4. Choose dev and run:

   ```bash
   export APP_ENV=dev
   python db_connect.py
   ```

   - Screenshot as `mongo_file_dev.png`, showing the dev endpoint, database and environment.

5. Choose test and run:

   ```bash
   export APP_ENV=test
   python db_connect.py
   ```

   - Screenshot as `mongo_file_test.png`, showing the test endpoint, database and environment.

6. Choose simulated prod and run:

   ```bash
   export APP_ENV=prod
   python db_connect.py
   ```

   - Screenshot as `mongo_file_prod.png`, showing the prod endpoint, database and environment.

The file version reads `APP_ENV` to choose a section and loads the four connection settings from that section. An INI file stores plaintext. `interpolation=None` makes the file reader treat values literally. The supplied dummy credentials are joined directly into the URI.

Complete **Exam Questions 2 and 3** after this task. Keep the file version as your final `db_connect.py`.


**Screenshot Required:**

- `credential_files_created.png`
- `credential_file_permissions.png`
- `mongo_file_dev.png`
- `mongo_file_test.png`
- `mongo_file_prod.png`

**Summary:** The same connection code now reads its settings from an external file.

---

## Exam Evaluation Questions

Write your predictions, results and explanations in `Lab4_Solution.pdf`. The three exam-practice screenshots are not included in the 15 mandatory checks.

### 1. A Missing Environment Variable

Complete this during Task 6, before replacing the environment-variable code.

1. With the valid prod profile exported, remove only the database-name variable:

   ```bash
   unset MONGO_DATABASE
   python db_connect.py
   ```

   - Screenshot as `Q1_missing_environment.png`, showing `Set all four MongoDB environment variables first.`.
2. Explain which `os.getenv()` call returned no value and why the `if` statement stopped the program.
3. Restore `export MONGO_DATABASE=school_prod` and confirm that the program connects again.

### 2. Which Source Does the File Version Read?

Complete this after Task 7.

1. Leave Task 6's prod username, password, endpoint and database variables exported. Remove only the section selector:

   ```bash
   unset APP_ENV
   python db_connect.py
   ```

   - Screenshot as `Q2_file_source_selection.png`, showing the dev connection.
2. Explain why this version connects to dev even though the shell still contains the prod connection settings.
3. Select `APP_ENV=test` and verify the test connection.

### 3. A Wrong Password in the File

1. Open the local `credentials.ini`. In the `[test]` section only, temporarily set `password = wrong`. Keep the username, endpoint and database name unchanged.
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

## Final Submission Checklist

- [ ] My required Ubuntu terminal identity is readable in every screenshot.
- [ ] My `Databases` repository is my own fork.
- [ ] Python ran directly on Ubuntu; Docker ran only the databases.
- [ ] I edited the same `db_connect.py` through all four stages.
- [ ] I captured MySQL, hardcoded MongoDB, and dev/test/prod results for environment and file versions.
- [ ] The MongoDB connection block stayed unchanged when I changed configuration sources.
- [ ] My final `db_connect.py` uses the credentials-file version.
- [ ] All 15 required screenshots use the exact filenames in `screenshots/`.
- [ ] I completed the three exam questions and restored the valid dummy settings.
- [ ] No passwords, credential-bearing URIs, private keys or tokens appear in screenshots or answers.
- [ ] `Lab4_Solution.pdf` contains my identity, connection results, fork URL, captions, evidence and exam answers.
- [ ] My screenshots and solution PDF are submitted in my `DevOps` repository.
- [ ] My complete `DevOps/Labs/Lab04` directory is visible on GitHub.

## Submission

Submit the URL of your own `DevOps/Labs/Lab04` directory. Include your `Databases` fork URL in `Lab4_Solution.pdf`.

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
