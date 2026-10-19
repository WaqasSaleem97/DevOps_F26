# Lab 4 — Student Observations and Exam Answers

- Complete actual name:
- Registration number:
- Course code and section:
- GitHub username:
- Databases fork URL:
- Databases final code commit URL:
- DevOps evidence repository URL:

## Connection Observations

Record what the program actually printed. Do not include passwords or credential-bearing URIs.

| Stage / environment | Host endpoint | Database name | Environment read from server | Result |
| --- | --- | --- | --- | --- |
| MySQL hardcoded | | | | |
| MongoDB hardcoded | | | | |
| Environment dev | | | | |
| Environment test | | | | |
| Environment simulated prod | | | | |
| File dev | | | | |
| File test | | | | |
| File simulated prod | | | | |

## Compare the Four Stages

1. How do the MySQL host, port, username, password and database arguments correspond to the Java example?
2. What do `MongoClient(uri)`, `client[database_name]` and `database.command("ping")` do?
3. Which two assignments changed when moving from hardcoded MongoDB values to `os.getenv()`?
4. Why must `MONGO_URI` and `MONGO_DATABASE` describe the same lab environment?
5. What does `os.getenv("APP_ENV", "dev")` select in the file version?
6. Which MongoDB connection lines stayed the same across stages 2–4?
7. What does the environment value read from the database prove?
8. Which approach would you choose for this lab, and why?
9. Do an INI file, mode `600` and `.gitignore` encrypt credentials? What would you need to consider for real production secrets?

## Exam Question 1 — A Missing Environment Variable

Prediction, command, observed result, explanation and restoration:

## Exam Question 2 — Which Source Does the File Version Read?

Prediction, observed database/endpoint/environment, explanation of the default section, and test selection:

## Exam Question 3 — A Wrong Password in the File

Prediction, authentication error, explanation and successful restoration:
