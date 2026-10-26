# Practical Lab: Setting Up Burp Suite for Manual SQL Injection Testing

## 1. Introduction

Burp Suite is a web application security testing platform used by ethical hackers and penetration testers to inspect, intercept, modify, and resend HTTP requests between a browser and a web application.

In this practical, we will use Burp Suite Community Edition to test an intentionally vulnerable web application called **OWASP Mutillidae II**.

Instead of simply entering values into a web form, we will use Burp Suite to observe the requests sent by the browser, modify selected parameters, and examine how the application responds.

### Learning Objectives

By the end of this practical, students should be able to:

1. Explain what Burp Suite does.
2. Set up Burp Suite for web application testing.
3. Connect a browser to Burp Suite's proxy.
4. Intercept and inspect HTTP requests.
5. Modify request parameters in Burp Suite Repeater.
6. Perform controlled manual SQL injection tests.
7. Identify signs of potentially unsafe SQL query construction.
8. Document findings and recommend security improvements.

## 2. Lab Environment and Requirements

The practical requires:

- Kali Linux or another supported operating system.
- Burp Suite Community Edition.
- OWASP Mutillidae II.
- OWASP Broken Web Applications virtual machine, or another authorized Mutillidae lab installation.
- A browser, preferably Burp Suite's built-in browser.
- A virtual lab network connecting the testing machine to Mutillidae.

### Example Lab Configuration

| Component | Example |
|---|---|
| Attacker/testing machine | Kali Linux |
| Interception tool | Burp Suite Community Edition |
| Target application | OWASP Mutillidae II |
| Example target address | `http://192.168.56.107/mutillidae/` |
| Test type | Manual SQL injection testing |
| Authorization | Isolated training lab only |

**Important:** The IP address above is an example from this lab. Your target may use a different IP address. Use the address that actually opens Mutillidae in your environment.

Keep the vulnerable application on a private lab network. Do not expose it to the public internet or test systems without explicit authorization.

---

## 3. Understanding How Burp Suite Works

When you browse directly to a web application, your browser sends HTTP requests to the server and receives responses.

Burp Suite can act as an intermediary between the browser and the application.

The process looks like this:

1. You enter a URL in the browser.
2. The browser sends a request through Burp Suite.
3. Burp Suite displays the request for inspection.
4. You can forward the request unchanged or modify it in the lab.
5. The application processes the request and returns a response.
6. Burp Suite allows you to inspect that response.

### Example

Suppose a login form contains these fields:

- Username: `Abba`
- Password: `abba`

The browser may send the values in an HTTP request. Burp Suite lets us inspect the actual parameter names and values rather than guessing what the application sends.

A request could contain a body similar to:

`username=Abba&password=abba`

This is only an illustration. Your actual request may use different parameter names, a different HTTP method, or a different format.

### Important Burp Suite Features

| Feature | Purpose |
|---|---|
| Proxy | Intercepts and inspects browser traffic |
| HTTP history | Keeps a record of requests and responses |
| Repeater | Allows requests to be modified and resent manually |
| Decoder | Helps encode or decode data |
| Intruder | Automates repeated requests with different inputs; use only within authorized testing |
| Target/Site map | Helps organize the application's discovered paths and endpoints |

For this practical, the main tools are **Proxy, HTTP history, and Repeater**.

---

## 4. Step One: Start OWASP Mutillidae

First, start the virtual machine hosting Mutillidae.

1. Open VirtualBox.
2. Start the OWASP Broken Web Applications virtual machine.
3. Allow the system to finish booting.
4. Identify the target machine's IP address using the VM's network information or the method provided by your lab instructor.
5. On Kali Linux, open Firefox or another browser.
6. Enter the Mutillidae URL.

Example:

`http://192.168.56.107/mutillidae/`

If the page loads, confirm that you can see the Mutillidae interface.

If it does not load, check that the target VM is running, that both VMs can communicate over their configured network, and that the IP address is correct.

**Do not continue until Mutillidae is accessible.**

---

## 5. Step Two: Start Burp Suite

On Kali Linux:

1. Open the application menu.
2. Search for **Burp Suite**.
3. Launch Burp Suite Community Edition.
4. Select **Temporary project** if prompted.
5. Select **Use Burp defaults**.
6. Click **Start Burp**.

The wording may vary slightly depending on the installed version.

Wait for the Burp Suite dashboard to appear.

### Why Use a Temporary Project?

A temporary project is sufficient for this introductory exercise. It allows you to work with Burp Suite without creating a persistent project file.

---

## 6. Step Three: Configure the Browser

The browser must send its traffic through Burp Suite before Burp can intercept it.

### Recommended Method: Use Burp's Built-in Browser

This is usually the easiest method for beginners.

1. In Burp Suite, open the **Proxy** tab.
2. Select **Intercept**.
3. Look for the button labelled **Open browser** or its equivalent in your version.
4. Click it.
5. A browser window should open that is configured to use Burp Suite's proxy.
6. In that browser, navigate to the Mutillidae URL.

Example:

`http://192.168.56.107/mutillidae/`

If Burp is configured correctly, browser traffic should appear in Burp Suite.

### Alternative Method: Configure Firefox Manually

Use this method if the built-in browser is unavailable or you need to use your existing Firefox profile.

1. Open Firefox.
2. Go to **Settings**.
3. Search for **Network Settings**.
4. Open **Settings** in the Network Settings section.
5. Select **Manual proxy configuration**.
6. Enter the following example settings:

| Setting | Value |
|---|---|
| HTTP Proxy | `127.0.0.1` |
| Port | `8080` |
| HTTPS Proxy | `127.0.0.1` |
| Port | `8080` |

7. If the option to use the same proxy for all protocols is available, you may select it.
8. Save the settings.

These values assume Burp's proxy listener is running on `127.0.0.1:8080`. Verify this in Burp under **Proxy settings** or the proxy listener configuration. If your listener uses a different address or port, use the actual values shown there.

For this exercise, the target uses HTTP, so you do not need to install a Burp CA certificate merely to inspect that HTTP traffic. HTTPS interception may require additional certificate configuration.

### Important Note

If Firefox cannot reach the target after you enable the proxy, check that Burp Suite is running and that the proxy listener is active. Also verify that Firefox is pointing to the correct listener.

---

## 7. Step Four: Intercept a Normal Request

We will first observe normal traffic before attempting any security test.

1. Return to Burp Suite.
2. Open **Proxy → Intercept**.
3. Ensure interception is enabled. Depending on the version, the control may say **Intercept is on**.
4. Switch to the browser configured to use Burp.
5. Load the Mutillidae page.
6. Observe the request that appears in Burp Suite.

Burp may pause the request while interception is enabled.

You may see request details such as:

- HTTP method, such as `GET` or `POST`.
- URL path.
- Host header.
- Cookies.
- Query-string parameters or form fields.
- Headers describing the browser and request content.

### Example Request

The following is an illustrative request, not a guaranteed copy of Mutillidae's actual request:

```http
POST /mutillidae/index.php?page=user-info.php HTTP/1.1
Host: 192.168.56.107
Content-Type: application/x-www-form-urlencoded

username=Abba&password=abba
```

Your real request may differ. Always inspect the captured request rather than copying this example blindly.

### Understanding the Request

- `POST` is the HTTP method.
- `/mutillidae/index.php?page=user-info.php` is the requested path and query string.
- `Host` identifies the target host.
- `Content-Type` describes the request body format.
- `username` and `password` are example form parameters.

### Forward the Request

If the request is legitimate and you want the application to receive it:

1. Click **Forward**.
2. If additional requests appear, inspect them and forward them as appropriate.
3. Observe the result in the browser.

If the page loads normally, you have confirmed that Burp can intercept and forward the traffic.

After this initial check, you can turn interception off temporarily if it is interrupting every browser request. You can still inspect traffic in **Proxy → HTTP history**.

---

## 8. Step Five: Find the Actual Login or User Lookup Request

Do not assume that the form submits the parameters shown in the example.

1. Open the relevant Mutillidae page.
2. Enter ordinary test values into the form.
3. Submit the form.
4. Return to Burp Suite.
5. Open **Proxy → HTTP history**.
6. Locate the request associated with the form submission.
7. Select it and inspect the request details.

Look for:

- The HTTP method.
- The URL and page parameter.
- The request body, if present.
- The actual names of the submitted parameters.
- The application's response.

For the Mutillidae User Lookup page used in this lab, the page has previously been observed at:

`index.php?page=user-info.php`

However, the exact path and parameters can vary between versions and configurations.

### What If You Cannot Find the Parameters?

Check whether the request uses:

- A query string in the URL.
- A form-encoded POST body.
- JSON or another request format.
- A different page or endpoint than expected.

Use the actual captured request for the rest of the practical.

---

## 9. Step Six: Send the Request to Repeater

Repeater lets you modify a captured request and send it again without repeatedly submitting the form in the browser.

1. In **Proxy → HTTP history**, select the relevant Mutillidae request.
2. Right-click the request.
3. Choose **Send to Repeater**.
4. Open the **Repeater** tab.
5. Inspect the request displayed on the left.
6. Inspect the response displayed on the right after sending the request.

The layout may vary slightly by Burp version.

### Why Use Repeater?

Repeater is useful because it allows you to:

- Change one parameter at a time.
- Resend the request under controlled conditions.
- Compare the application's responses.
- Determine whether an input affects application behaviour.
- Record reproducible evidence.

Before modifying anything, send the request unchanged and record the baseline response.

---

## 10. Step Seven: Perform a Controlled Manual SQL Injection Test

**Only perform this test against your authorized Mutillidae lab.**

The purpose is to determine whether a user-controlled input may be inserted unsafely into a database query.

### Test A: Establish a Baseline

1. In Repeater, leave the request unchanged.
2. Click **Send**.
3. Inspect the response.
4. Record the status code, response length if shown, and relevant response message.

For example, the application might return an authentication error for invalid credentials.

This baseline provides a comparison point for later tests.

### Test B: Submit an Apostrophe

1. Identify a form parameter that corresponds to the username or another input being tested.
2. Change only that parameter's value to a single apostrophe:

   `'`

3. Leave other request fields unchanged.
4. Click **Send**.
5. Inspect the response carefully.
6. Compare it with the baseline.

If you are testing the password field, modify the password parameter instead. Do not change several parameters simultaneously, because that makes it harder to determine which input caused the result.

### Example

Suppose the captured request body is:

```text
username=Abba&password=abba
```

For a controlled username test, the body might become:

```text
username=%27&password=abba
```

Here, `%27` is the URL-encoded representation of an apostrophe. Burp may also display the character directly, depending on how the request is edited and encoded.

The exact parameter names and encoding must match the captured request.

### What Should You Observe?

Possible outcomes include:

- The same response as the baseline.
- A different application error.
- A database-related error.
- A change in response content or length.
- A server-side failure.

A database error may indicate that user input is affecting SQL query construction. However, **an error alone does not prove that an attacker can bypass authentication or retrieve database records**.

### Previous Lab Observation

In an earlier run of this Mutillidae environment, entering an apostrophe in the password field produced a MySQL syntax error. The displayed query included:

```sql
SELECT * FROM accounts WHERE username='Abba' AND password='''
```

This is evidence of unsafe query construction and excessive database error disclosure in that test. It is not, by itself, proof of successful authentication bypass or data extraction.

Your environment may produce a different response. Record what you actually observe.

---

## 11. Step Eight: Compare Requests and Responses

A disciplined tester changes one variable at a time.

Compare:

1. A normal request with ordinary input.
2. A request containing an apostrophe in the username parameter.
3. A request containing an apostrophe in the password parameter.

For each request, record:

- Parameter changed.
- Test value used.
- HTTP status code.
- Application response.
- Whether a database error appeared.
- Any other observable differences.

### Example Evidence Table

| Test | Input | Observation | Interpretation |
|---|---|---|---|
| Baseline | Ordinary values | Authentication error | Baseline behaviour |
| Username test | Apostrophe in username | Record the actual result | Check for input-handling differences |
| Password test | Apostrophe in password | MySQL syntax error observed in a previous run | Possible unsafe SQL construction and error disclosure |

Replace the observations with the results from your own current run.

### Why Status Codes Are Not Enough

A response with HTTP status `200 OK` does not necessarily mean that a test succeeded. An application may return an error message inside a normal HTTP response.

Likewise, a `500 Internal Server Error` does not automatically prove SQL injection. Look at the response body and compare it with the baseline.

---

## 12. Step Nine: Try a Second Input Carefully

After completing the apostrophe test, select another ordinary input parameter from the captured request.

1. Return to Repeater.
2. Restore the original request.
3. Change only the selected parameter.
4. Send the request.
5. Compare the result against the baseline.
6. Record any observable differences.

Do not assume that every parameter is vulnerable just because one parameter produces a database error.

If your instructor has approved further SQL injection exercises, continue with the separate manual SQL injection lesson and use the database type, query structure, and application behaviour observed in your lab. Avoid applying syntax intended for a different database system.

**Technical note:** This Mutillidae instance uses MySQL in the observed lab. SQL Server-specific expressions are not interchangeable with MySQL syntax.

---

## 13. Step Ten: Save Evidence for Your Report

Professional security testing requires reproducible evidence.

Capture screenshots of:

1. Burp Suite running.
2. The proxy listener configuration, if needed to demonstrate setup.
3. A normal request in HTTP history.
4. The captured request in Repeater.
5. The baseline response.
6. The modified test request.
7. The resulting response.
8. Your completed evidence table.

### Screenshot Guidelines

- Ensure the relevant request and response are readable.
- Keep the test value visible where it is necessary to explain the result.
- Remove or redact session cookies, authorization headers, tokens, and other secrets.
- Avoid including unrelated personal or sensitive data.
- Do not claim successful exploitation unless the evidence demonstrates it.

### Example Finding

**Title:** Potential SQL Injection Indicator in User Lookup Input

**Observation:** A single apostrophe submitted through a tested input caused a MySQL syntax error in the observed lab run.

**Security implication:** The application may be constructing SQL queries unsafely. Detailed database errors may also disclose implementation information.

**Limitations:** This test alone does not establish authentication bypass, successful data extraction, or the vulnerability of every input parameter.

**Recommended remediation:** Use parameterized queries, validate inputs appropriately, and return generic error messages to users while keeping detailed diagnostics in protected server logs.

---

## 14. Troubleshooting

### Problem 1: Burp Does Not Show Any Requests

Possible causes:

- The browser is not using Burp's proxy.
- The proxy listener is not running.
- The listener address or port is incorrect.
- The request was made in a different browser profile.

**Solution:**

1. Verify the proxy listener in Burp.
2. Use Burp's built-in browser as the first troubleshooting step.
3. Load the Mutillidae page again.
4. Check **Proxy → HTTP history**.

### Problem 2: The Browser Cannot Open Mutillidae

Possible causes:

- The target VM is powered off.
- The target IP address changed.
- The two VMs cannot communicate.
- The URL path is incorrect.

**Solution:**

1. Confirm that the target VM is running.
2. Verify the target IP address.
3. Confirm network connectivity between the VMs.
4. Open the Mutillidae URL directly in the configured browser.

### Problem 3: Every Request Stops in Burp

This is expected when interception is enabled.

**Solution:** Click **Forward** to allow the request to continue, or disable interception temporarily while retaining HTTP history for review.

### Problem 4: The Request Has No Visible Username or Password

Possible causes:

- You captured a page-loading request instead of a form submission.
- The form submits to another endpoint.
- The parameters are in another part of the request.
- The form uses a different data format.

**Solution:** Submit the form again and inspect the newest relevant entry in HTTP history. Look at both the URL and request body.

### Problem 5: The Apostrophe Produces No Database Error

This does not automatically mean that the application is secure.

Possible explanations include:

- The input is handled safely.
- The tested parameter does not reach the SQL query.
- The application suppresses database errors.
- The request was not modified as intended.
- The application uses a different code path.

Verify the request and response before drawing conclusions.

### Problem 6: The Application Shows a Different Error Than Expected

Record the actual result. Application versions, configurations, and request formats can differ. Do not change your report to match an expected result that you did not observe.

---

## 15. Security Recommendations

Developers should use several layers of defence to reduce SQL injection risk.

### 1. Use Parameterized Queries

Do not build SQL statements by concatenating untrusted user input into query strings.

Parameterized queries separate the SQL statement from the data supplied by the user.

### 2. Validate Input

Apply appropriate validation based on the expected format and business requirements of each field. Validation should complement parameterized queries, not replace them.

### 3. Use Least-Privilege Database Accounts

The application's database account should have only the permissions required for its intended operations.

### 4. Handle Errors Securely

Do not expose raw SQL statements, database credentials, table details, or database stack traces to end users.

Log detailed diagnostic information securely for authorized administrators.

### 5. Protect Authentication

Use established authentication controls and secure password storage. Applications should not rely on unsafe string-built SQL queries to verify credentials.

### 6. Retest After Fixing

Repeat the same baseline and modified-input tests after remediation. Confirm that unexpected input no longer produces database errors or unintended behaviour.

---

## 16. Lab Exercise for Students

Complete the following tasks in the authorized Mutillidae environment.

- [ ] Start the Mutillidae virtual machine.
- [ ] Launch Burp Suite Community Edition.
- [ ] Configure the browser to use Burp Suite.
- [ ] Open Mutillidae through the configured browser.
- [ ] Capture a normal form-submission request.
- [ ] Find the request in HTTP history.
- [ ] Send the request to Repeater.
- [ ] Send the unmodified request and record the baseline.
- [ ] Test one input with a single apostrophe.
- [ ] Test a second input separately.
- [ ] Compare the responses.
- [ ] Capture screenshots of the relevant evidence.
- [ ] Complete the evidence table.
- [ ] Explain what the observations do and do not prove.
- [ ] Recommend suitable remediation.

### Questions

1. What is Burp Suite, and what role does its proxy play?
2. What is the difference between HTTP history and Repeater?
3. Why should you record a baseline response before modifying a request?
4. What does a database syntax error after entering an apostrophe suggest?
5. Why does a database error not automatically prove successful SQL injection exploitation?
6. Why must the actual request parameters be inspected instead of guessed?
7. What is the purpose of parameterized SQL queries?
8. Why should detailed database errors not be shown to ordinary users?

---

## 17. Conclusion

Burp Suite provides ethical hackers with a controlled way to inspect and manipulate HTTP requests during authorized web application testing.

In this practical, students learn to configure the proxy, capture real requests, use Repeater to modify parameters, compare application responses, and document potential SQL injection indicators.

The key lesson is that effective security testing depends on evidence. A database error is a useful observation, but a responsible tester must distinguish between a potential vulnerability indicator and a confirmed security impact.

Testing should always remain within the scope of explicit authorization, and findings should be reported accurately with practical remediation recommendations.
