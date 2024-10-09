# CSN. Lab 6. Static Code Analysis

## Done by Fedorov Alexey

---

```
In this lab your task is simple, you have a list of vulnerable projects and you are
required to an automated static code analysis (SAST). Below you will a list of
vulnerable applications/projects as well as a list of suggested SAST solutions. pick at
least 2 applications/projects and scan them with one of the suggested SAST
solution.
```

I decided to use snyk analyser for this lab.

## DVWA

I forked DVWA repo. 

![image](https://github.com/user-attachments/assets/7c13d7be-eaf2-4594-be39-cdbbb6ce8bb2)

Started scanning on https://app.snyk.io/ and got a report.

![image](https://github.com/user-attachments/assets/810a525c-b048-4058-a36c-9ef6c6c091dd)

Let's discuss some of vulnerabilities.

### SQL Injection

There are a lot of vulnerabilities related to SQL injection in project.

![image](https://github.com/user-attachments/assets/a6524fcc-6850-4c4c-bf81-32d3039c3074)

Considering that this is PHP, it's easy to create a vulnerability like an SQL injection. As seen in the image, the query `$query = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";` is passed to the function `$sqlite_db_connection->query`, and the value of `$id` is neither validated nor sanitized beforehand. This means that an SQL injection can be easily performed, and in this case, it would allow retrieving the first and last names of all users.

In another case, it could expose all user information, including sensitive data.

![image](https://github.com/user-attachments/assets/d7039c5b-e2c7-4da6-b1c8-3dbc109944eb)

### Command injection

Project has many command injection vulnerabilities.

![image](https://github.com/user-attachments/assets/1cfa183f-4709-4104-bf7b-8b82799a839a)

The vulnerability is present in the lines `shell_exec('ping ' . $target);` and `shell_exec('ping -c 4 ' . $target);`. This means that an attacker can insert any shell code by placing a semicolon `;` after the ping command. Such a vulnerability allows compromising not only data but also the integrity of the infrastructure.

### Cross-site Scripting (XSS)

I also found an XSS vulnerability in this project.

![image](https://github.com/user-attachments/assets/b07d36ad-5cdf-471e-9dc4-adbcb4a16032)

The vulnerability exists here because some HTML elements on the page were not sanitized before being rendered. As a result, an attacker can inject malicious code into the page, which will be executed on the user's computer.

## DVNA

I forked DVNA repo.

![image](https://github.com/user-attachments/assets/6d3ba15a-3a2d-4d13-b6bc-9c7cb7c87eeb)

Started scanning on https://app.snyk.io/ and got a report.

![image](https://github.com/user-attachments/assets/c84dfa35-17c1-4e7d-a3bc-6055f2ffe71d)

### SQL Injection

I also found an SQL injection here.

![image](https://github.com/user-attachments/assets/a00f4e20-4ae0-4b88-a2a3-3292753361b7)

Attacker can inject code into the login form and retrieve any user data using the UNION statement.

### Hardcoded secret

Here is hardcoded secret vulnerability.

![image](https://github.com/user-attachments/assets/b52541ec-a318-47df-b178-f771e84eacb7)

If an attacker gains access to the code, they will learn the `secret_key`, which appears to be used for session initialization or JWT authentication.

### Deserialization of Untrusted Data

One more vulnerability related to deserialization of untrusted Data

![image](https://github.com/user-attachments/assets/eebc6ccd-6893-4d15-94ad-f5bf628302d8)

Attacker can easily insert a line of code that executes shell commands, gaining access to the system.

## References

1. https://docs.snyk.io/scan-with-snyk
2. https://portswigger.net/web-security/sql-injection
3. https://owasp.org/www-community/attacks/Command_Injection
4. https://owasp.org/www-community/vulnerabilities/Deserialization_of_untrusted_data
