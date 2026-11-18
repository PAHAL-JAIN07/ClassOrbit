# 🌌 ClassOrbit — Teacher & Student Learning Platform

ClassOrbit is a full-stack Python web application designed to bring classroom learning workflows into one place. It provides separate portals for teachers and students, assessment-based learning-gap indicators, smart mark-sheet import, subject-wise attendance tracking, and class-scoped learning resources.

> **Project status:** Student/teacher platform MVP. Review the limitations and deployment checklist below before using it with real student data or publishing it to a public server.

## ✨ Features

### 🔐 Accounts and access
- Separate teacher and student registration/login flows.
- Password hashing and role-based route protection.
- Teacher-created classes and student class-join codes.

### 👩‍🏫 Teacher portal
- Create and manage classes and subjects.
- Record concept-based assessments and student marks.
- View class-level learning-gap summaries.
- Record subject/date attendance for students.
- Upload class resources such as notes, assignments, instructions, and notices.

### 🎓 Student portal
- View personal learning-gap indicators based on recorded assessment results.
- View subject-wise attendance and attendance recovery estimates.
- Browse and download resources shared with the student's class.

### 📥 Smart mark-sheet import
- Import supported CSV, XLSX, PPTX, PDF, and image files.
- Extract available tabular/text data; OCR can be used for images and scanned PDFs when Tesseract is installed.
- Review and edit extracted rows before confirming the import.
- The current MVP expects a table-like layout containing student names and numeric marks.

### 📊 Attendance and learning insights
- Calculate attendance separately for each subject.
- Show a warning when attendance is below the configured 75% default target.
- Estimate the number of consecutive future classes a student must attend to reach the target.
- Summarize assessment performance by recorded concept.

### 🎨 Interface
- Responsive teacher and student pages.
- Dark/light theme preference.
- Animated interface elements and dashboard visualizations.

## 🧰 Tech stack

- **Backend:** Python, Flask, Flask-Login
- **Database:** SQLite by default; MySQL supported through SQLAlchemy/PyMySQL
- **Frontend:** HTML, CSS, JavaScript, Jinja templates
- **Data processing:** Pandas, openpyxl
- **Document extraction:** PyMuPDF, python-pptx, pytesseract, Pillow
- **Charts:** Chart.js

## 📁 Project structure

```text
ClassOrbit/
├── app/
│   ├── routes/             # Authentication, teacher, student, resource routes
│   ├── services/            # File extraction services
│   ├── templates/           # Jinja HTML templates
│   ├── static/              # CSS and JavaScript
│   ├── models.py            # SQLAlchemy models
│   └── __init__.py          # Flask application factory
├── instance/                # Local database and uploads (not committed)
├── tests/                   # Automated tests
├── .env.example             # Environment variable template
├── .gitignore
├── config.py
├── requirements.txt
└── run.py
```

## ⚙️ Getting started (Windows PowerShell)

### 1. Requirements

- Python 3.11 or newer
- Git (for cloning/pushing to GitHub)
- PowerShell or the VS Code integrated terminal
- Optional: Tesseract OCR executable for image/scanned-PDF text extraction
- Optional: MySQL server if you do not want to use the default SQLite database

### 2. Clone the repository

```powershell
git clone https://github.com/YOUR-USERNAME/ClassOrbit.git
cd ClassOrbit
```

If you downloaded the project ZIP instead, extract it and open the `ClassOrbit` folder in VS Code.

### 3. Create a virtual environment

```powershell
py -m venv .venv
```

If PowerShell blocks activation scripts, allow activation **only for the current terminal session**:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
```

Alternative without changing PowerShell execution policy: use Command Prompt and run:

```bat
.venv\Scripts\activate.bat
```

### 4. Install dependencies

```powershell
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 5. Configure environment variables

Copy the example file:

```powershell
Copy-Item .env.example .env
```

Set a unique, long `SECRET_KEY` in `.env`. Do not commit `.env` to GitHub. SQLite is the default local database, so a separate database server is not required for the basic local setup.

You can also set a temporary key in the current PowerShell session:

```powershell
$env:SECRET_KEY = "replace-with-a-long-random-development-secret"
```

### 6. Run the application

```powershell
python run.py
```

Open **http://127.0.0.1:5000** in your browser.

## 🧪 Optional demo data

To create demonstration accounts and sample class data, run this from the project directory after installing dependencies:

```powershell
flask --app run.py seed-demo
```

Demo credentials:

| Role | Email | Password |
|---|---|---|
| Teacher | `teacher@demo.local` | `Teacher123!` |
| Student | `student@demo.local` | `Student123!` |

Demo class join code: `BCA3DEMO`

These credentials are public demonstration credentials. Never use them for a public deployment or real student information.

## 🧪 Run tests

```powershell
python -m unittest discover -s tests -v
```

## 🗄️ Optional MySQL configuration

Create a database and a dedicated database user, then set `DATABASE_URL` in your environment or `.env` file. Example:

```text
DATABASE_URL=mysql+pymysql://username:password@localhost/classorbit?charset=utf8mb4
```

URL-encode special characters in the database password. Keep credentials out of source control.

## 🔎 OCR setup

Image and scanned-PDF OCR requires the Tesseract application to be installed separately, in addition to the Python package. Install Tesseract for your operating system and ensure its executable is available on `PATH`. Digital PDFs, CSV, and XLSX files generally provide more reliable extraction than low-quality scans or handwriting.

## ⚠️ Current MVP limitations

- Smart Import is a review-and-confirm workflow, not automatic grading.
- The current extraction flow expects a table-like layout with student names and a numeric marks column. Unusual or complex layouts may require manual correction.
- Only records matching students enrolled in the selected class are eligible to be imported.
- The initial assessment model associates an assessment with a concept; advanced question-to-concept mapping is a future enhancement.
- The application does not automatically grade handwritten answers or determine a student's exact misconception from a total score.
- Attendance recovery estimates assume the student attends every future class and that sessions have equal weight. Institutions may apply different attendance policies.
- Tesseract OCR is optional and must be installed separately for image/scanned-PDF OCR.

## 🔒 Security notes before deployment

This repository is intended as an educational/local project starter. Before deploying it publicly or entering real student data:

- Use a strong, unique secret key and never commit `.env`, passwords, API keys, or database credentials.
- Use HTTPS and a production WSGI server; do not expose Flask's development server/debug mode publicly.
- Add and verify CSRF protection for all state-changing forms before public deployment.
- Configure secure session-cookie settings for HTTPS deployments.
- Review uploaded files for malicious content; enforce file-size/type limits and store uploads outside publicly served directories.
- Restrict access to student records and follow applicable privacy, consent, retention, and deletion requirements.
- Use database migrations for production schema changes rather than relying on automatic table creation.

## 🤝 Contributing

Contributions and suggestions are welcome. For substantial changes, open an issue first to discuss the proposed improvement. Please do not include real student records, credentials, or private uploaded files in issues or pull requests.

## 📄 License

No open-source license has been assigned yet. Unless a license is added, the source is provided for viewing and educational evaluation; reuse and redistribution are not automatically granted. Add a license file if you decide how you want others to use the project.

---

**Built with Python and Flask.** 🌱
