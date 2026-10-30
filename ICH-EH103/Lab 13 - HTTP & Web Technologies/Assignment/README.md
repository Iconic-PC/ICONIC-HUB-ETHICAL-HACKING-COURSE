# ICH-EH103 — HTTP AND WEB TECHNOLOGIES EXERCISE

## Project: Build, Host and Test Iconic Hub Cyber Lab

**Course:** Iconic Hub Ethical Hacking Course  
**Module:** ICH-EH103  
**Exercise:** HTTP and Web Technologies Exercise  
**Duration:** 3–4 hours  
**Operating System:** Kali Linux / Ubuntu Linux  
**Code Editor:** Nano or Visual Studio Code  
**Frontend:** HTML, CSS  
**Backend:** Python Flask  
**Database:** SQLite  
**Network Analysis:** Wireshark  
**Web Testing:** Burp Suite Community Edition

---

# 1. Introduction

Web applications are software applications that users access through web browsers. They allow users to interact with web servers by submitting information, requesting resources and receiving responses.

Examples of web applications include:

- Online banking platforms.
- E-commerce websites.
- Student portals.
- Social media platforms.
- Hospital management systems.
- Online examination platforms.
- Content management systems.

When a user opens a website, the browser communicates with a web server using protocols such as HTTP or HTTPS.

For example, when a student logs into a student portal, the browser sends the login information to the server. The server checks the supplied credentials and determines whether access should be granted.

Understanding this communication is important in cybersecurity because many web application vulnerabilities occur when applications process user input, manage sessions or enforce access controls incorrectly.

In this exercise, students will build and host a small web application called **Iconic Hub Cyber Lab**.

The application will provide a login page, user dashboard, fictional student records and a feedback form.

After building the application, students will use Wireshark and Burp Suite to inspect its network traffic and investigate selected security weaknesses in an isolated training environment.

# 2. Learning objectives

At the end of this exercise, students should be able to:

1. Explain the relationship between web browsers and web servers.
2. Explain how HTTP requests and responses work.
3. Differentiate between HTTP GET and POST methods.
4. Identify HTTP headers, cookies and sessions.
5. Understand the basic structure of a URL.
6. Build a simple website using HTML and CSS.
7. Develop a basic backend using Python Flask.
8. Create a login system.
9. Connect a web application to an SQLite database.
10. Host a web application on a Linux machine.
11. Capture and inspect HTTP traffic using Wireshark.
12. Intercept and examine requests using Burp Suite.
13. Explain authentication and authorisation.
14. Identify weaknesses in authentication and access-control mechanisms.
15. Explain SQL injection and insecure direct object references.
16. Recommend appropriate security controls for web applications.

# 3. Required tools and technologies

| Tool / Technology | Purpose |
|---|---|
| Kali Linux or Ubuntu | Development and hosting environment |
| Nano | Terminal-based code editor |
| Visual Studio Code | Graphical code editor |
| Python 3 | Backend programming |
| Flask | Python web framework |
| SQLite | Database management |
| HTML | Webpage structure |
| CSS | Website styling |
| Firefox | Accessing the application |
| Wireshark | Network traffic capture and analysis |
| Burp Suite Community | HTTP request interception and inspection |

# 4. Understanding the application

Students will build a small student portal with the following features.

| Feature | Description |
|---|---|
| Login page | Allows users to authenticate |
| Dashboard | Displays information about the logged-in user |
| Student records | Displays fictional student information |
| Feedback page | Allows users to submit feedback |
| Logout | Ends the current session |
| Vulnerable login | Demonstrates weak authentication |
| Vulnerable search | Demonstrates SQL injection |
| Vulnerable record access | Demonstrates insecure direct object references |

## 4.1 Secure and vulnerable environments

The application has two sets of functionality.

**Secure application**

This demonstrates security practices such as:

- Password hashing.
- Parameterized SQL queries.
- Authentication checks.
- Session management.
- Input validation.
- Output escaping provided by Jinja templates.

**Vulnerable training environment**

This contains deliberately weak controls for controlled security demonstrations:

- SQL injection in the training login.
- SQL injection in the training search function.
- Missing record-level authorisation checks.
- Plain-text fictional training credentials.

The vulnerable environment must only be enabled on an isolated classroom network. Do not deploy it to a public hosting platform or expose it to the internet.

All accounts, passwords and records in this exercise are fictional.

# 5. Understanding the application architecture

The application follows a basic client-server architecture.

```text
                  STUDENT
                     |
                     |
              Web Browser
             (Firefox)
                     |
                     |
              HTTP Request
                     |
                     v
          ----------------------
          |   FLASK WEB SERVER |
          |                    |
          | Login Processing   |
          | Dashboard          |
          | Feedback           |
          | Student Records    |
          ----------------------
                     |
                     |
                     v
              SQLITE DATABASE
                     |
          -----------------------
          |                     |
       User Accounts       Student Records
          |                     |
          -----------------------
```

Wireshark will be used to observe network traffic, while Burp Suite will be used to inspect and modify HTTP requests.

# 6. Preparing the Linux environment

Open the Kali Linux or Ubuntu terminal.

Update the package list:

```bash
sudo apt update
```

Install the required packages:

```bash
sudo apt install python3 python3-venv python3-pip nano wireshark tree -y
```

Check the Python version:

```bash
python3 --version
```

Check pip:

```bash
python3 -m pip --version
```

# 7. Creating the project directory

Create the project directory:

```bash
mkdir -p ~/iconic-hub-cyber-lab
```

Enter the directory:

```bash
cd ~/iconic-hub-cyber-lab
```

Create the required folders:

```bash
mkdir templates
mkdir static
```

Create the Python files:

```bash
touch app.py
touch database.py
touch requirements.txt
```

Create the HTML files:

```bash
touch templates/base.html
touch templates/login.html
touch templates/dashboard.html
touch templates/records.html
touch templates/feedback.html
touch templates/message.html
```

Create the CSS file:

```bash
touch static/style.css
```

Display the project structure:

```bash
tree
```

The project structure should look like this:

```text
iconic-hub-cyber-lab/
│
├── app.py
├── database.py
├── requirements.txt
│
├── templates/
│   ├── base.html
│   ├── login.html
│   ├── dashboard.html
│   ├── records.html
│   ├── feedback.html
│   └── message.html
│
└── static/
    └── style.css
```

# 8. Creating a Python virtual environment

A Python virtual environment keeps the project's dependencies separate from the operating system's Python packages.

Create the environment:

```bash
python3 -m venv venv
```

Activate it:

```bash
source venv/bin/activate
```

When activated, the terminal should display `(venv)`.

Create the `requirements.txt` file:

```bash
nano requirements.txt
```

Enter:

```text
Flask
Werkzeug
```

Save the file.

Install the dependencies:

```bash
pip install -r requirements.txt
```

# 9. Creating the database

The application will use SQLite to store user accounts, student records and feedback.

The database will contain three main tables:

| Table | Purpose |
|---|---|
| users | Secure application accounts |
| student_records | Fictional student information |
| feedback | Student feedback submissions |
| lab_users | Fictional accounts for vulnerable training exercises |

Open the database file:

```bash
nano database.py
```

Paste the following complete code:

```python
import sqlite3
from pathlib import Path
from werkzeug.security import generate_password_hash

BASE_DIR = Path(__file__).resolve().parent
DATABASE = BASE_DIR / "cyberlab.db"


def get_db():
    connection = sqlite3.connect(DATABASE)
    connection.row_factory = sqlite3.Row
    return connection


def initialise_database():

    db = get_db()

    db.executescript("""
    CREATE TABLE IF NOT EXISTS users (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        username TEXT UNIQUE NOT NULL,
        password_hash TEXT NOT NULL,
        full_name TEXT NOT NULL,
        email TEXT NOT NULL,
        role TEXT NOT NULL DEFAULT 'student'
    );

    CREATE TABLE IF NOT EXISTS student_records (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        full_name TEXT NOT NULL,
        email TEXT NOT NULL,
        department TEXT NOT NULL
    );

    CREATE TABLE IF NOT EXISTS feedback (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        user_id INTEGER NOT NULL,
        subject TEXT NOT NULL,
        message TEXT NOT NULL,
        FOREIGN KEY (user_id) REFERENCES users(id)
    );

    CREATE TABLE IF NOT EXISTS lab_users (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        username TEXT NOT NULL,
        password TEXT NOT NULL,
        role TEXT NOT NULL
    );
    """)

    user_count = db.execute(
        "SELECT COUNT(*) FROM users"
    ).fetchone()[0]

    if user_count == 0:

        accounts = [
            (
                "student01",
                "Student123!",
                "Alex Morgan",
                "alex@example.test",
                "student"
            ),
            (
                "student02",
                "Student123!",
                "Jamie Smith",
                "jamie@example.test",
                "student"
            ),
            (
                "student03",
                "Student123!",
                "Taylor Brown",
                "taylor@example.test",
                "student"
            ),
            (
                "admin",
                "Admin123!",
                "Training Administrator",
                "admin@example.test",
                "admin"
            )
        ]

        for username, password, name, email, role in accounts:

            db.execute("""
                INSERT INTO users
                (username, password_hash, full_name, email, role)
                VALUES (?, ?, ?, ?, ?)
            """, (
                username,
                generate_password_hash(password),
                name,
                email,
                role
            ))

    record_count = db.execute(
        "SELECT COUNT(*) FROM student_records"
    ).fetchone()[0]

    if record_count == 0:

        records = [
            (
                "Alex Morgan",
                "alex@example.test",
                "Cybersecurity"
            ),
            (
                "Jamie Smith",
                "jamie@example.test",
                "Networking"
            ),
            (
                "Taylor Brown",
                "taylor@example.test",
                "Software Engineering"
            ),
            (
                "Jordan White",
                "jordan@example.test",
                "Information Technology"
            ),
            (
                "Casey Green",
                "casey@example.test",
                "Computer Science"
            )
        ]

        db.executemany("""
            INSERT INTO student_records
            (full_name, email, department)
            VALUES (?, ?, ?)
        """, records)

    lab_count = db.execute(
        "SELECT COUNT(*) FROM lab_users"
    ).fetchone()[0]

    if lab_count == 0:

        lab_accounts = [
            ("labstudent01", "LabPass123!", "student"),
            ("labstudent02", "LabPass123!", "student"),
            ("labstudent03", "LabPass123!", "student"),
            ("labadmin", "LabAdmin123!", "admin")
        ]

        db.executemany("""
            INSERT INTO lab_users
            (username, password, role)
            VALUES (?, ?, ?)
        """, lab_accounts)

    db.commit()
    db.close()


if __name__ == "__main__":

    initialise_database()

    print("Cyber Lab database initialised successfully.")
```

Save the file.

Run the database initialisation command:

```bash
python3 database.py
```

Expected output:

```text
Cyber Lab database initialised successfully.
```

The database file `cyberlab.db` should now exist inside the project directory.

**Security note:** The `users` table stores password hashes. The separate `lab_users` table intentionally stores fictional training passwords in plain text to demonstrate insecure credential storage. Never use real passwords in the vulnerable training database.

# 10. Creating the Flask application

Flask will handle requests from the browser, process submitted information and communicate with the SQLite database.

Open:

```bash
nano app.py
```

Paste the complete code:

```python
import os
import sqlite3

from pathlib import Path

from flask import (
    Flask,
    render_template,
    request,
    redirect,
    url_for,
    session,
    flash,
    abort
)

from werkzeug.security import check_password_hash

from database import get_db, initialise_database


BASE_DIR = Path(__file__).resolve().parent

app = Flask(__name__)

app.secret_key = os.environ.get(
    "SECRET_KEY",
    "development-only-secret-change-before-use"
)

app.config.update(
    SESSION_COOKIE_HTTPONLY=True,
    SESSION_COOKIE_SAMESITE="Lax"
)

LAB_MODE = os.environ.get("LAB_MODE", "0") == "1"


# Initialise the database when the application starts.
initialise_database()


# --------------------------------------------------
# HOME
# --------------------------------------------------

@app.route("/")
def home():

    if "user_id" in session:
        return redirect(url_for("dashboard"))

    return redirect(url_for("login"))


# --------------------------------------------------
# SECURE LOGIN
# --------------------------------------------------

@app.route("/login", methods=["GET", "POST"])
def login():

    if request.method == "POST":

        username = request.form.get(
            "username", ""
        ).strip()

        password = request.form.get(
            "password", ""
        )

        db = get_db()

        user = db.execute(
            """
            SELECT id, username, password_hash,
                   full_name, email, role
            FROM users
            WHERE username = ?
            """,
            (username,)
        ).fetchone()

        db.close()

        if user and check_password_hash(
            user["password_hash"], password
        ):

            session.clear()

            session["user_id"] = user["id"]

            flash(
                "Login successful.",
                "success"
            )

            return redirect(url_for("dashboard"))

        flash(
            "Invalid username or password.",
            "error"
        )

    return render_template("login.html")


# --------------------------------------------------
# SECURE DASHBOARD
# --------------------------------------------------

@app.route("/dashboard")
def dashboard():

    if "user_id" not in session:

        return redirect(url_for("login"))

    db = get_db()

    user = db.execute(
        """
        SELECT id, username, full_name, email, role
        FROM users
        WHERE id = ?
        """,
        (session["user_id"],)
    ).fetchone()

    db.close()

    if not user:

        session.clear()

        return redirect(url_for("login"))

    return render_template(
        "dashboard.html",
        user=user
    )


# --------------------------------------------------
# STUDENT RECORDS
# --------------------------------------------------

@app.route("/records")
def records():

    if "user_id" not in session:

        return redirect(url_for("login"))

    db = get_db()

    records = db.execute(
        """
        SELECT id, full_name, email, department
        FROM student_records
        ORDER BY id
        """
    ).fetchall()

    db.close()

    return render_template(
        "records.html",
        records=records
    )


# --------------------------------------------------
# FEEDBACK
# --------------------------------------------------

@app.route("/feedback", methods=["GET", "POST"])
def feedback():

    if "user_id" not in session:

        return redirect(url_for("login"))

    if request.method == "POST":

        subject = request.form.get(
            "subject", ""
        ).strip()

        message = request.form.get(
            "message", ""
        ).strip()

        if not subject or not message:

            flash(
                "Please complete all fields.",
                "error"
            )

            return redirect(url_for("feedback"))

        if len(subject) > 100 or len(message) > 2000:

            flash(
                "Your input is too long.",
                "error"
            )

            return redirect(url_for("feedback"))

        db = get_db()

        db.execute(
            """
            INSERT INTO feedback
            (user_id, subject, message)
            VALUES (?, ?, ?)
            """,
            (
                session["user_id"],
                subject,
                message
            )
        )

        db.commit()
        db.close()

        flash(
            "Feedback submitted successfully.",
            "success"
        )

        return redirect(url_for("feedback"))

    return render_template("feedback.html")


# --------------------------------------------------
# LOGOUT
# --------------------------------------------------

@app.route("/logout")
def logout():

    session.clear()

    flash(
        "You have been logged out.",
        "success"
    )

    return redirect(url_for("login"))


# ==================================================
# VULNERABLE TRAINING ENVIRONMENT
# ==================================================

def require_lab_mode():

    if not LAB_MODE:

        abort(404)


# --------------------------------------------------
# VULNERABLE TRAINING LOGIN
# --------------------------------------------------

@app.route(
    "/lab/vuln/login",
    methods=["GET", "POST"]
)
def vulnerable_login():

    require_lab_mode()

    if request.method == "POST":

        username = request.form.get(
            "username", ""
        )

        password = request.form.get(
            "password", ""
        )

        db = get_db()

        # INTENTIONALLY VULNERABLE:
        # User input is concatenated directly
        # into the SQL query.

        query = (
            "SELECT id, username, role "
            "FROM lab_users "
            f"WHERE username = '{username}' "
            f"AND password = '{password}'"
        )

        try:

            user = db.execute(
                query
            ).fetchone()

        except sqlite3.Error:

            user = None

        db.close()

        if user:

            session["lab_user_id"] = user["id"]

            session["lab_username"] = user["username"]

            session["lab_role"] = user["role"]

            return redirect(
                url_for("vulnerable_dashboard")
            )

        flash(
            "Training login failed.",
            "error"
        )

    return render_template(
        "login.html",
        lab_login=True
    )


# --------------------------------------------------
# VULNERABLE TRAINING DASHBOARD
# --------------------------------------------------

@app.route("/lab/vuln/dashboard")
def vulnerable_dashboard():

    require_lab_mode()

    if "lab_user_id" not in session:

        return redirect(
            url_for("vulnerable_login")
        )

    return render_template(
        "message.html",
        title="Vulnerable Training Dashboard",
        message=(
            "You are viewing the isolated "
            "vulnerable training environment."
        )
    )


# --------------------------------------------------
# VULNERABLE RECORD ACCESS
# --------------------------------------------------

@app.route(
    "/lab/vuln/records/<int:record_id>"
)
def vulnerable_record(record_id):

    require_lab_mode()

    if "lab_user_id" not in session:

        return redirect(
            url_for("vulnerable_login")
        )

    # INTENTIONALLY VULNERABLE:
    # The application checks whether the user is logged in,
    # but does not verify whether the user is authorised
    # to access the requested record.

    db = get_db()

    record = db.execute(
        """
        SELECT id, full_name, email, department
        FROM student_records
        WHERE id = ?
        """,
        (record_id,)
    ).fetchone()

    db.close()

    if not record:

        abort(404)

    return render_template(
        "message.html",
        title="Training Record",
        message=(
            f"Record ID: {record['id']} | "
            f"Name: {record['full_name']} | "
            f"Email: {record['email']} | "
            f"Department: {record['department']}"
        )
    )


# --------------------------------------------------
# VULNERABLE SEARCH
# --------------------------------------------------

@app.route("/lab/vuln/search")
def vulnerable_search():

    require_lab_mode()

    if "lab_user_id" not in session:

        return redirect(
            url_for("vulnerable_login")
        )

    search_term = request.args.get(
        "q", ""
    )

    db = get_db()

    # INTENTIONALLY VULNERABLE:
    # The search value is inserted directly
    # into the SQL statement.

    query = (
        "SELECT id, full_name, email, department "
        "FROM student_records "
        f"WHERE full_name LIKE '%{search_term}%'"
    )

    try:

        results = db.execute(
            query
        ).fetchall()

    except sqlite3.Error:

        results = []

    db.close()

    return render_template(
        "records.html",
        records=results,
        lab_search=True
    )


# --------------------------------------------------
# ERROR HANDLING
# --------------------------------------------------

@app.errorhandler(404)
def not_found(error):

    return render_template(
        "message.html",
        title="Page Not Found",
        message="The requested page could not be found."
    ), 404


# --------------------------------------------------
# START APPLICATION
# --------------------------------------------------

if __name__ == "__main__":

    app.run(
        host="0.0.0.0",
        port=5000,
        debug=False
    )
```

Save the file.

**Important:** The vulnerable routes are disabled by default. They only become available when `LAB_MODE=1` is set. Do not enable that mode on a public-facing server.

# 11. Creating the HTML templates

HTML defines the structure and content of the webpages.

We will create six HTML files.

## 11.1 Base template

The base template provides the common layout used by the other pages.

Open:

```bash
nano templates/base.html
```

Paste:

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">

    <meta name="viewport"
          content="width=device-width, initial-scale=1.0">

    <title>
        Iconic Hub Cyber Lab
    </title>

    <link rel="stylesheet"
          href="{{ url_for('static', filename='style.css') }}">
</head>

<body>

    <header class="navbar">

        <div class="logo">
            ICONIC HUB CYBER LAB
        </div>

        <nav>

            {% if session.get('user_id') %}

                <a href="{{ url_for('dashboard') }}">
                    Dashboard
                </a>

                <a href="{{ url_for('records') }}">
                    Records
                </a>

                <a href="{{ url_for('feedback') }}">
                    Feedback
                </a>

                <a href="{{ url_for('logout') }}">
                    Logout
                </a>

            {% elif session.get('lab_user_id') %}

                <a href="{{ url_for('vulnerable_dashboard') }}">
                    Lab Dashboard
                </a>

                <a href="{{ url_for('logout') }}">
                    Logout
                </a>

            {% else %}

                <a href="{{ url_for('login') }}">
                    Login
                </a>

            {% endif %}

        </nav>

    </header>

    <main class="container">

        {% with messages = get_flashed_messages(with_categories=true) %}

            {% if messages %}

                {% for category, message in messages %}

                    <div class="alert {{ category }}">
                        {{ message }}
                    </div>

                {% endfor %}

            {% endif %}

        {% endwith %}

        {% block content %}

        {% endblock %}

    </main>

    <footer>

        <p>
            Iconic Hub — Secure. Build. Innovate.
        </p>

        <p>
            Cybersecurity Training Environment
        </p>

    </footer>

</body>
</html>
```

## 11.2 Login page

This page allows students to enter their usernames and passwords.

Open:

```bash
nano templates/login.html
```

Paste:

```html
{% extends "base.html" %}

{% block content %}

<div class="login-card">

    <h1>
        Student Login
    </h1>

    <p>
        Welcome to Iconic Hub Cyber Lab.
    </p>

    {% if lab_login %}

        <div class="warning">
            Isolated Vulnerable Training Environment
        </div>

        <form method="POST"
              action="{{ url_for('vulnerable_login') }}">

    {% else %}

        <form method="POST"
              action="{{ url_for('login') }}">

    {% endif %}

        <label for="username">
            Username
        </label>

        <input
            type="text"
            id="username"
            name="username"
            placeholder="Enter your username"
            required
        >

        <label for="password">
            Password
        </label>

        <input
            type="password"
            id="password"
            name="password"
            placeholder="Enter your password"
            required
        >

        <button type="submit">
            Login
        </button>

    </form>

    {% if not lab_login %}

        <p class="small-text">
            Use your assigned training account.
        </p>

    {% endif %}

</div>

{% endblock %}
```

## 11.3 Dashboard page

The dashboard displays the logged-in student's information.

Open:

```bash
nano templates/dashboard.html
```

Paste:

```html
{% extends "base.html" %}

{% block content %}

<section class="welcome">

    <h1>
        Welcome, {{ user.full_name }}
    </h1>

    <p>
        You have successfully accessed your student dashboard.
    </p>

</section>

<div class="dashboard-grid">

    <div class="dashboard-card">

        <h3>
            Username
        </h3>

        <p>
            {{ user.username }}
        </p>

    </div>

    <div class="dashboard-card">

        <h3>
            Email
        </h3>

        <p>
            {{ user.email }}
        </p>

    </div>

    <div class="dashboard-card">

        <h3>
            Account Role
        </h3>

        <p>
            {{ user.role }}
        </p>

    </div>

</div>

<section class="content-card">

    <h2>
        Student Portal
    </h2>

    <p>
        Use the navigation links to access student records
        and submit feedback.
    </p>

</section>

{% endblock %}
```

## 11.4 Student records page

Open:

```bash
nano templates/records.html
```

Paste:

```html
{% extends "base.html" %}

{% block content %}

<h1>
    Student Records
</h1>

<p>
    Fictional records used for cybersecurity training.
</p>

{% if lab_search %}

    <div class="warning">
        Vulnerable SQL Search Demonstration
    </div>

    <form method="GET"
          action="{{ url_for('vulnerable_search') }}">

        <label for="q">
            Search Student
        </label>

        <input
            type="text"
            id="q"
            name="q"
            placeholder="Enter a student name"
        >

        <button type="submit">
            Search
        </button>

    </form>

{% endif %}

<div class="table-container">

    <table>

        <thead>

            <tr>

                <th>ID</th>
                <th>Full Name</th>
                <th>Email</th>
                <th>Department</th>

            </tr>

        </thead>

        <tbody>

            {% for record in records %}

                <tr>

                    <td>
                        {{ record.id }}
                    </td>

                    <td>
                        {{ record.full_name }}
                    </td>

                    <td>
                        {{ record.email }}
                    </td>

                    <td>
                        {{ record.department }}
                    </td>

                </tr>

            {% else %}

                <tr>

                    <td colspan="4">
                        No records found.
                    </td>

                </tr>

            {% endfor %}

        </tbody>

    </table>

</div>

{% endblock %}
```

## 11.5 Feedback page

Open:

```bash
nano templates/feedback.html
```

Paste:

```html
{% extends "base.html" %}

{% block content %}

<div class="content-card">

    <h1>
        Student Feedback
    </h1>

    <p>
        Submit your questions, observations or suggestions.
    </p>

    <form method="POST"
          action="{{ url_for('feedback') }}">

        <label for="subject">
            Subject
        </label>

        <input
            type="text"
            id="subject"
            name="subject"
            maxlength="100"
            required
        >

        <label for="message">
            Message
        </label>

        <textarea
            id="message"
            name="message"
            rows="6"
            maxlength="2000"
            required
        ></textarea>

        <button type="submit">
            Submit Feedback
        </button>

    </form>

</div>

{% endblock %}
```

## 11.6 General message page

Open:

```bash
nano templates/message.html
```

Paste:

```html
{% extends "base.html" %}

{% block content %}

<div class="content-card">

    <h1>
        {{ title }}
    </h1>

    <p>
        {{ message }}
    </p>

    <a href="{{ url_for('login') }}"
       class="button-link">
        Return to Login
    </a>

</div>

{% endblock %}
```

# 12. Creating the website styling

CSS controls the appearance of the website.

Open:

```bash
nano static/style.css
```

Paste:

```css
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    padding: 0;
    font-family: Arial, sans-serif;
    background: #f3f5f9;
    color: #202b3c;
}

.navbar {
    background: #101c32;
    color: white;
    padding: 20px 7%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 15px;
}

.logo {
    font-size: 19px;
    font-weight: bold;
    letter-spacing: 1px;
}

nav {
    display: flex;
    gap: 20px;
    flex-wrap: wrap;
}

nav a {
    color: white;
    text-decoration: none;
    font-size: 14px;
}

nav a:hover {
    color: #65b7ff;
}

.container {
    width: 90%;
    max-width: 1100px;
    margin: 40px auto;
    min-height: 65vh;
}

.login-card {
    background: white;
    width: 100%;
    max-width: 420px;
    margin: 50px auto;
    padding: 35px;
    border-radius: 12px;
    box-shadow: 0 5px 25px rgba(0,0,0,0.08);
}

.login-card h1 {
    margin-top: 0;
    color: #101c32;
}

form {
    display: flex;
    flex-direction: column;
    gap: 12px;
}

label {
    font-weight: bold;
    margin-top: 10px;
}

input,
textarea {
    width: 100%;
    padding: 13px;
    border: 1px solid #ccd3df;
    border-radius: 6px;
    font-size: 15px;
}

input:focus,
textarea:focus {
    outline: 2px solid #65b7ff;
    border-color: #65b7ff;
}

button,
.button-link {
    background: #145da0;
    color: white;
    border: none;
    padding: 13px 18px;
    border-radius: 6px;
    cursor: pointer;
    font-size: 15px;
    text-decoration: none;
    text-align: center;
}

button:hover,
.button-link:hover {
    background: #0d467b;
}

.welcome {
    background: #101c32;
    color: white;
    padding: 35px;
    border-radius: 12px;
    margin-bottom: 25px;
}

.dashboard-grid {
    display: grid;
    grid-template-columns: repeat(
        auto-fit,
        minmax(200px, 1fr)
    );
    gap: 20px;
    margin-bottom: 25px;
}

.dashboard-card,
.content-card {
    background: white;
    padding: 25px;
    border-radius: 10px;
    box-shadow: 0 3px 15px rgba(0,0,0,0.05);
    margin-bottom: 20px;
}

.dashboard-card h3 {
    color: #145da0;
}

.table-container {
    overflow-x: auto;
    background: white;
    padding: 15px;
    border-radius: 10px;
}

table {
    width: 100%;
    border-collapse: collapse;
}

th,
td {
    text-align: left;
    padding: 14px;
    border-bottom: 1px solid #e3e7ed;
}

th {
    background: #101c32;
    color: white;
}

.alert {
    padding: 13px;
    margin-bottom: 15px;
    border-radius: 6px;
}

.success {
    background: #d9f5e5;
    color: #17643a;
}

.error {
    background: #ffe0e0;
    color: #8c2020;
}

.warning {
    background: #fff0cc;
    color: #765000;
    padding: 13px;
    border-radius: 6px;
    margin-bottom: 20px;
}

.small-text {
    font-size: 13px;
    color: #657084;
}

footer {
    background: #101c32;
    color: white;
    text-align: center;
    padding: 25px;
    margin-top: 40px;
}

@media (max-width: 600px) {

    .navbar {
        flex-direction: column;
        align-items: flex-start;
    }

    nav {
        gap: 12px;
    }

    .container {
        width: 94%;
    }

    .login-card {
        padding: 22px;
    }
}
```

# 13. Initialise and test the application

Make sure the virtual environment is activated:

```bash
source venv/bin/activate
```

Initialise the database:

```bash
python3 database.py
```

Start the application in secure mode:

```bash
python3 app.py
```

The terminal should display a local server address similar to:

```text
Running on http://127.0.0.1:5000
Running on http://192.168.56.10:5000
```

Open Firefox and visit:

```text
http://127.0.0.1:5000
```

You should see the Iconic Hub Cyber Lab login page.

# 14. Training login accounts

The application automatically creates the following fictional accounts.

## Secure application accounts

| Username | Password |
|---|---|
| student01 | Student123! |
| student02 | Student123! |
| student03 | Student123! |
| admin | Admin123! |

These are demonstration credentials only.

Students should log in with their assigned account.

After login, they should be able to:

1. View their dashboard.
2. View fictional student records.
3. Submit feedback.
4. Log out.

## Vulnerable training accounts

| Username | Password |
|---|---|
| labstudent01 | LabPass123! |
| labstudent02 | LabPass123! |
| labstudent03 | LabPass123! |
| labadmin | LabAdmin123! |

These accounts are stored separately in the intentionally vulnerable training table.

# 15. Hosting the application on a Linux server

The application can run on one Linux machine and be accessed from other machines on the same classroom network.

For this exercise, use a VirtualBox Host-Only Network or another isolated private network.

Do not expose the vulnerable application to the public internet.

## Step 1: Find the server IP address

On the Linux server, run:

```bash
ip addr
```

Identify the IP address assigned to the private classroom network interface.

For example:

```text
192.168.56.10
```

This is only an example. Use the actual IP address shown on your machine.

## Step 2: Start the application

From the project directory, activate the virtual environment:

```bash
source venv/bin/activate
```

Start the application:

```bash
python3 app.py
```

The Flask application listens on port 5000.

## Step 3: Access the website from another machine

Open Firefox on another authorised machine connected to the same private network.

Enter:

```text
http://SERVER_IP:5000
```

Replace `SERVER_IP` with the actual server IP address.

For example:

```text
http://192.168.56.10:5000
```

The login page should appear.

## Step 4: Confirm connectivity

From the client machine, run:

```bash
ping SERVER_IP
```

Then test the HTTP service:

```bash
curl -I http://SERVER_IP:5000
```

A successful response should include an HTTP status such as:

```text
HTTP/1.1 200 OK
```

If the client cannot connect, check:

- Both machines are connected to the same private network.
- The server is running.
- The IP address is correct.
- Port 5000 is permitted by the host firewall.

# 16. Understanding HTTP requests and responses

HTTP stands for Hypertext Transfer Protocol.

It is an application-layer protocol used for communication between clients and servers.

A client sends an HTTP request. The server processes it and returns an HTTP response.

## Example HTTP GET request

```http
GET /login HTTP/1.1
Host: 192.168.56.10:5000
User-Agent: Mozilla/5.0
Accept: text/html
```

This request asks the server to return the login page.

## Example HTTP POST request

When a student submits the login form, the browser sends a POST request.

An illustrative request body might look like:

```http
POST /login HTTP/1.1
Host: 192.168.56.10:5000
Content-Type: application/x-www-form-urlencoded

username=student01&password=Student123%21
```

This demonstrates how form information is transmitted.

**Important:** This classroom application uses HTTP on a private network. Never use real passwords or sensitive information. HTTP does not encrypt the request body. A properly configured production application should use HTTPS.

## Common HTTP methods

| Method | Purpose |
|---|---|
| GET | Retrieve information |
| POST | Submit information |
| PUT | Replace a resource |
| PATCH | Modify part of a resource |
| DELETE | Remove a resource |
| HEAD | Retrieve response headers without the response body |
| OPTIONS | Discover supported communication methods |

## Common HTTP status codes

| Code | Meaning |
|---|---|
| 200 | OK |
| 201 | Created |
| 301 | Moved Permanently |
| 302 | Found / Redirect |
| 400 | Bad Request |
| 401 | Unauthorised |
| 403 | Forbidden |
| 404 | Not Found |
| 405 | Method Not Allowed |
| 500 | Internal Server Error |

# 17. Practical exercise: Inspect HTTP traffic with Wireshark

Wireshark is a network protocol analyser used to capture and inspect network packets.

It allows security analysts to understand how devices communicate across networks.

## Exercise 1: Start Wireshark

Open Wireshark.

Identify the network interface connected to the private classroom network.

Start capturing packets.

## Exercise 2: Generate HTTP traffic

Open Firefox and visit:

```text
http://SERVER_IP:5000
```

Log in using a fictional training account.

Visit the dashboard and submit feedback.

## Exercise 3: Apply an HTTP display filter

In Wireshark, enter:

```text
http
```

This filters the captured packets to show HTTP traffic.

## Exercise 4: Inspect an HTTP request

Select a packet containing an HTTP request.

Expand:

```text
Hypertext Transfer Protocol
```

Identify:

- Request method.
- Request URI.
- Host header.
- User-Agent header.
- Content-Type header, where present.
- Request body, where visible.

## Exercise 5: Inspect the HTTP response

Select the corresponding server response.

Identify:

- HTTP response status.
- Server response headers.
- Content-Type.
- Set-Cookie, if present.
- Response body, where visible.

## Exercise 6: Understand session cookies

After a successful login, inspect the server response for the `Set-Cookie` header.

Then inspect subsequent requests for the `Cookie` header.

Students should explain how the browser sends a session cookie to the server so that the server can associate requests with a logged-in session.

Do not share, copy or publish session cookies. They can provide access to an active account.

## Exercise 7: Understand the limitations of HTTP capture

Discuss:

1. Why can HTTP form data be visible in a packet capture?
2. What happens when the same communication uses HTTPS?
3. Why should login credentials not be sent over unencrypted HTTP?
4. What does encryption protect?
5. Can HTTPS prevent SQL injection or broken access control?

**Expected learning:** HTTPS protects data in transit, but it does not automatically fix vulnerabilities in application logic or database queries.

# 18. Practical exercise: Inspect HTTP requests with Burp Suite

Burp Suite is a web application security testing platform.

It can intercept and inspect HTTP requests and responses between a browser and a web application.

## Exercise 1: Open Burp Suite

On Kali Linux, open Burp Suite Community Edition.

Select:

```text
Temporary project
```

Then select:

```text
Use Burp defaults
```

Click Start Burp.

## Exercise 2: Configure Firefox

In Firefox:

1. Open Settings.
2. Search for Network Settings.
3. Select Manual proxy configuration.
4. Set HTTP Proxy to `127.0.0.1`.
5. Set the port to `8080`.
6. Apply the configuration.

The browser should now send traffic through Burp Suite when Burp's proxy listener is active.

If the application is on another machine, the browser still connects to the application using the server's private IP address.

## Exercise 3: Intercept a login request

In Burp Suite:

1. Open Proxy.
2. Open Intercept.
3. Turn Intercept on.
4. Return to Firefox.
5. Open the login page.
6. Submit a fictional training account.

Burp should intercept the HTTP request.

An example request may look like:

```http
POST /login HTTP/1.1
Host: 192.168.56.10:5000
Content-Type: application/x-www-form-urlencoded

username=student01&password=Student123%21
```

Identify the username and password parameters.

Click Forward to allow the request to continue.

## Exercise 4: Inspect the response

Examine the server response.

Identify:

- HTTP status code.
- Response headers.
- Session cookie.
- Redirect response, if present.

Record your observations.

# 19. Practical exercise: Enable the vulnerable training environment

The vulnerable environment is deliberately separate from the secure application's normal routes.

Only enable it on the isolated classroom network.

Stop the currently running Flask application with:

```text
CTRL + C
```

From the project directory, activate the virtual environment:

```bash
source venv/bin/activate
```

Enable the vulnerable training routes:

```bash
export LAB_MODE=1
```

Set a temporary classroom-only session secret:

```bash
export SECRET_KEY="classroom-training-secret-change-this"
```

Start the application:

```bash
python3 app.py
```

The vulnerable training login is available at:

```text
http://SERVER_IP:5000/lab/vuln/login
```

The vulnerable routes are not available when `LAB_MODE` is disabled.

When the exercise is complete, stop the server and return to secure mode by starting it without `LAB_MODE=1`.

# 20. Practical exercise: Authentication and SQL injection

Authentication is the process of verifying a user's identity.

A login system normally compares the supplied username and password with the account information stored in a database.

The vulnerable training login demonstrates what can happen when an application inserts user input directly into an SQL statement.

## Exercise 1: Normal login

Open:

```text
http://SERVER_IP:5000/lab/vuln/login
```

Use:

```text
Username: labstudent01
Password: LabPass123!
```

Record the result.

## Exercise 2: Understand the vulnerable query

The application constructs its login query using string concatenation:

```python
query = (
    "SELECT id, username, role "
    "FROM lab_users "
    f"WHERE username = '{username}' "
    f"AND password = '{password}'"
)
```

This is insecure because the application treats user input as part of the SQL command rather than as data.

A secure application should use parameterized queries.

## Exercise 3: Controlled authentication-bypass demonstration

In the vulnerable training login only, submit the following fictional test input:

```text
Username: labstudent01' -- 
Password: anything
```

The SQL comment causes the remaining password condition to be ignored in this intentionally vulnerable query.

If the demonstration succeeds, the application will redirect to the vulnerable training dashboard.

Record:

- The input submitted.
- The HTTP response.
- Whether access was granted.
- Why the query behaved differently.
- Which programming mistake caused the vulnerability.

This exercise is limited to the supplied training application and its fictional account database.

## Exercise 4: Inspect the request using Burp Suite

Repeat the controlled login demonstration with Burp Intercept enabled.

Inspect the request body.

Identify:

- The username parameter.
- The password parameter.
- The HTTP method.
- The request path.
- The server response.

Explain how changing a parameter changes the SQL statement constructed by the vulnerable application.

# 21. Practical exercise: SQL injection and synthetic data exposure

SQL injection occurs when untrusted input is incorporated into an SQL query without appropriate separation between commands and data.

The vulnerable search route demonstrates this problem.

## Exercise 1: Normal search

After logging into the vulnerable training environment, open:

```text
http://SERVER_IP:5000/lab/vuln/search?q=Alex
```

Observe the matching record.

## Exercise 2: Controlled SQL injection test

Use the search parameter with the following test input:

```text
'
```

Observe the result.

Then test:

```text
' OR '1'='1' -- 
```

The application may return multiple fictional student records because the condition is manipulated.

## Exercise 3: Explain the result

Answer these questions:

1. Why did the search return more records?
2. What role did the quotation mark play?
3. What did the OR condition change?
4. Why is string concatenation dangerous?
5. How can parameterized queries prevent this vulnerability?

Students must not use this exercise against any other database or website.

## Secure alternative

The secure application uses parameterized queries:

```python
records = db.execute(
    """
    SELECT id, full_name, email, department
    FROM student_records
    WHERE full_name LIKE ?
    """,
    (f"%{search_term}%",)
).fetchall()
```

The database treats the search term as data rather than executable SQL syntax.

# 22. Practical exercise: Insecure Direct Object Reference (IDOR)

IDOR is an access-control vulnerability that occurs when an application allows users to access objects or records without properly checking whether they are authorised to access them.

The vulnerable training route is:

```text
/lab/vuln/records/1
```

## Exercise 1: Access a training record

Log into the vulnerable training environment.

Visit:

```text
http://SERVER_IP:5000/lab/vuln/records/1
```

Observe the displayed fictional record.

## Exercise 2: Change the record identifier

Change the final number:

```text
/lab/vuln/records/2
```

Then:

```text
/lab/vuln/records/3
```

Observe whether the application returns different records.

## Exercise 3: Explain the vulnerability

The vulnerable route checks whether the user is logged in but does not check whether that user is authorised to access the requested record.

This is an example of missing object-level authorisation.

Answer:

1. What is an object identifier?
2. Why should an application not trust a record ID supplied by the browser?
3. What is the difference between authentication and authorisation?
4. What should the server verify before returning a record?
5. How could this issue affect a real student portal?

## Secure implementation example

A secure application should associate each record with an owner and check ownership before returning it.

For example:

```python
record = db.execute(
    """
    SELECT id, full_name, email
    FROM student_records
    WHERE id = ? AND owner_id = ?
    """,
    (record_id, session["user_id"])
).fetchone()
```

The application must also ensure the database schema contains the appropriate ownership relationship and that administrator access is handled through explicit authorisation rules.

# 23. Understanding authentication and authorisation

These concepts are related but different.

**Authentication:** Verifies who a user is.

Example: A student enters a username and password.

**Authorisation:** Determines what an authenticated user is allowed to do.

Example: A student can access their own profile but cannot access administrator-only functions.

| Authentication | Authorisation |
|---|---|
| Verifies identity | Controls permissions |
| Usually occurs during login | Applies when accessing resources |
| Uses credentials or other identity factors | Uses roles, ownership and permissions |
| Answers: Who are you? | Answers: What may you access? |

# 24. Security comparison: Secure vs vulnerable implementation

| Security area | Secure implementation | Vulnerable training implementation |
|---|---|---|
| Password storage | Password hashes | Plain-text training passwords |
| SQL queries | Parameterized queries | String concatenation |
| Login | Hash verification | Unsafe SQL query |
| Record access | Authentication and ownership checks should be enforced | Missing record ownership check |
| Session | Session cleared on secure login/logout | Training session used for demonstrations |
| User input | Basic validation | Direct SQL query construction |

# 25. Practical assessment

Students must complete the following tasks.

## Task 1: Website development

Build the Iconic Hub Cyber Lab application.

Demonstrate:

- Login page.
- Dashboard.
- Student records.
- Feedback form.
- Logout function.

## Task 2: Linux hosting

Run the application on a Linux machine.

Demonstrate that another authorised machine on the same private network can access the website.

## Task 3: Wireshark

Capture the application traffic.

Identify:

- HTTP GET request.
- HTTP POST request.
- HTTP status code.
- Host header.
- Content-Type header.
- Cookie or Set-Cookie header, where present.

## Task 4: Burp Suite

Intercept a login request.

Identify the username and password parameters in the fictional training request.

Explain how Burp Suite allows a tester to inspect and modify requests.

## Task 5: Authentication testing

Use the isolated vulnerable training login to demonstrate the supplied SQL injection authentication-bypass case.

Explain the cause and the appropriate fix.

## Task 6: SQL injection

Use the vulnerable search feature to demonstrate how unsafe SQL construction can expose fictional records.

Explain why parameterized queries are necessary.

## Task 7: IDOR

Demonstrate the difference between accessing a permitted record and accessing a record without proper ownership checks.

Explain how object-level authorisation should be implemented.

# 26. Evidence and screenshots

Students should capture screenshots of their work.

Use the following naming convention:

| Screenshot | Required evidence |
|---|---|
| lab13_01 | Project directory and file structure |
| lab13_02 | Python virtual environment and dependencies |
| lab13_03 | Database initialisation |
| lab13_04 | Login page |
| lab13_05 | Successful student dashboard |
| lab13_06 | Student records page |
| lab13_07 | Feedback form |
| lab13_08 | Application accessed from another machine |
| lab13_09 | Wireshark HTTP capture |
| lab13_10 | HTTP request inspection |
| lab13_11 | Burp Suite intercepted login request |
| lab13_12 | Vulnerable training login demonstration |
| lab13_13 | SQL injection search demonstration |
| lab13_14 | IDOR training demonstration |
| lab13_15 | Secure and vulnerable implementation comparison |

Do not include actual passwords, session cookies or sensitive personal information in submitted screenshots.

# 27. Troubleshooting

## Problem 1: Flask module not found

Run:

```bash
source venv/bin/activate
pip install -r requirements.txt
```

## Problem 2: Port 5000 is already in use

Check:

```bash
sudo ss -ltnp | grep 5000
```

Stop the process that is occupying the port, or choose another available port and update the URL accordingly.

## Problem 3: Website cannot be accessed from another machine

Check the server IP:

```bash
ip addr
```

Check connectivity:

```bash
ping SERVER_IP
```

Check the Flask server is running and listening on the expected interface and port.

Check the firewall and VirtualBox network configuration.

## Problem 4: Database records are missing

Run:

```bash
python3 database.py
```

Then restart Flask.

## Problem 5: Vulnerable training pages return 404

Check that the application was started with:

```bash
export LAB_MODE=1
```

Then restart:

```bash
python3 app.py
```

## Problem 6: Burp Suite does not intercept traffic

Check:

- Burp Suite is running.
- Proxy Intercept is enabled.
- Firefox is configured to use `127.0.0.1:8080`.
- The browser is using the configured proxy.
- The request is being sent through the proxy.

# 28. Important security recommendations

The vulnerable training application is for classroom use only.

Students must observe the following rules:

1. Use only the supplied fictional accounts.
2. Use only the synthetic records created by the project.
3. Do not use the vulnerable application on a public hosting platform.
4. Do not test the application against external websites.
5. Keep the vulnerable environment on a private, isolated network.
6. Do not use real student passwords or personal information.
7. Do not share session cookies or authentication tokens.
8. Disable vulnerable mode after the exercise.
9. Use parameterized SQL queries in real applications.
10. Use HTTPS for applications that handle sensitive information.
11. Apply proper authorisation checks to every protected resource.
12. Never treat successful login as proof that a user may access every record.

When the exercise is complete, stop the Flask server:

```text
CTRL + C
```

Close the vulnerable training environment and restart the application without setting `LAB_MODE=1`.

# 29. Take-home assignment

Answer the following questions:

1. What is HTTP?
2. What is the difference between HTTP and HTTPS?
3. Explain the client-server model.
4. What is the difference between GET and POST?
5. What is an HTTP request header?
6. What is an HTTP response status code?
7. What is a session cookie?
8. What is the difference between authentication and authorisation?
9. What is SQL injection?
10. What is an authentication bypass?
11. What is IDOR?
12. Why should passwords be hashed?
13. Why are parameterized SQL queries important?
14. What information can Wireshark reveal from an unencrypted HTTP request?
15. What is the role of Burp Suite in web application testing?
16. Does HTTPS automatically prevent SQL injection? Explain your answer.

## Practical submission

Submit:

- Your complete project source code.
- Screenshots of the working application.
- Evidence of Linux hosting.
- Wireshark capture screenshots.
- Burp Suite request screenshots.
- Your explanations of the controlled vulnerability demonstrations.
- A short report comparing the secure and vulnerable implementations.

# 30. Conclusion

In this exercise, students build a functional web application and learn how browsers communicate with web servers.

They gain practical experience writing HTML, CSS and Python code, connecting an application to a database, creating login functionality and hosting a website on Linux.

They also learn how to inspect HTTP requests and responses using Wireshark and Burp Suite.

Finally, they examine how weaknesses in authentication, SQL query construction and access-control checks can affect web applications.

This practical establishes the foundation for the next lessons in web application security.

# 31. Next exercise

**ICH-EH103 — LAB 14: BURP SUITE FUNDAMENTALS**

Students will build on this application by learning how to configure Burp Suite, intercept browser requests, examine parameters, inspect responses and perform controlled manual security testing.

# 32. References

1. MDN Web Docs — HTTP Overview: https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview
2. MDN Web Docs — How the Web Works: https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Web_standards/How_the_web_works
3. Flask Documentation: https://flask.palletsprojects.com/
4. Python Documentation: https://docs.python.org/3/
5. SQLite Documentation: https://www.sqlite.org/docs.html
6. Wireshark Documentation: https://www.wireshark.org/docs/
7. PortSwigger Web Security Academy: https://portswigger.net/web-security
8. OWASP Web Security Testing Guide: https://owasp.org/www-project-web-security-testing-guide/

---

**ICONIC HUB**

*Secure. Build. Innovate.*

*Where Web Development Meets Cybersecurity.*
