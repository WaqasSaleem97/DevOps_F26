# Lab 4 — Student observations and exam answers

- Complete actual name:
- Registration number:
- Course code and section:
- GitHub username:
- Databases fork URL:
- Databases fork submission commit URL:
- DevOps evidence repository URL:

## Connection observations

Record the endpoint, database and actual server-side marker for MySQL, hardcoded MongoDB, environment-mode dev/test/prod, and file-mode dev/test/prod. Do not record passwords.

| Stage / environment | Endpoint | Database | Marker read from database | Result |
| --- | --- | --- | --- | --- |
| MySQL hardcoded | | | | |
| MongoDB hardcoded | | | | |
| Environment dev | | | | |
| Environment test | | | | |
| Environment prod | | | | |
| File dev | | | | |
| File test | | | | |
| File prod | | | | |

## Configuration comparison

1. What values are hardcoded in stages 1 and 2, and what changes when deployment settings change?
2. Why does host Python use 127.0.0.1 with a different published port for each database?
3. How does the host Python process read variables exported in the Ubuntu shell?
4. Why does changing only APP_ENV not select a different set of credentials?
5. What is the role of auth_source in this lab?
6. How did you prove that the database operation stayed unchanged across profiles?
7. What happened for the wrong password, missing password and wrong environment marker?
8. Are plaintext configuration files encrypted? Does .gitignore secure an already committed credential?
9. Which configuration approach would you select for this lab and for a production deployment, and why?

## Exam Question 1 — Configuration source selection

Prediction, commands used, observed endpoints/markers, and explanation:

## Exam Question 2 — Special characters in file credentials

Test result and explanation of why interpolation is disabled:

## Exam Question 3 — Cross-environment credentials

Prediction, observed error, restoration steps, and explanation:
