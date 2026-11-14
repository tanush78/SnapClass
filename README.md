# SnapClass

### AI-assisted classroom attendance with Python and Streamlit

SnapClass brings student enrollment, face recognition, voice identification, and attendance records into one classroom application. Teachers manage subjects and review attendance before saving it; students register a profile and join classes using a subject code or QR link.

**Stack:** Python · Streamlit · dlib · scikit-learn · Resemblyzer · Supabase

## Features

- **Teacher and student interfaces:** separate registration, login, and dashboard flows.
- **Face-based attendance:** extract face embeddings from classroom photos and compare them with enrolled students.
- **Voice-based attendance:** create voice profiles and identify enrolled students from recorded audio.
- **Class enrollment:** create subjects and share join codes or QR links.
- **Attendance review:** inspect detected students before confirming and saving records to Supabase.
- **Reports:** view attendance summaries by subject and session.

## How it works

```text
Student registration → face / voice embeddings → Supabase
Teacher selects a subject → classroom photo or audio → recognition
Recognition results → teacher review → saved attendance records
```

The face pipeline uses dlib embeddings and distance-based matching. The voice pipeline uses Resemblyzer embeddings and similarity matching. Recognition thresholds are implemented in the pipeline modules.

## Run locally

Use Python 3.12 or newer; dependency availability may vary by platform.

```bash
git clone https://github.com/tanush78/SnapClass.git
cd SnapClass
python -m venv .venv
# macOS / Linux
source .venv/bin/activate
# Windows PowerShell: .venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

Create `.streamlit/secrets.toml` with your own Supabase project settings:

```toml
SUPABASE_URL = "https://YOUR_PROJECT.supabase.co"
SUPABASE_KEY = "YOUR_SUPABASE_KEY"
```

The application expects the tables `teachers`, `students`, `subjects`, `subject_students`, and `attendance_logs`, with the relationships used in [`src/database/db.py`](src/database/db.py). Database migrations are not included, so the schema must be configured separately before the app can be used end to end. Keep real credentials out of Git.

```bash
streamlit run app.py
```

For camera and microphone features, allow the browser to access those devices. Use a configured database and registered test profiles when trying recognition.

## Repository guide

```text
SnapClass/
├── app.py                 # Streamlit entry point
├── requirements.txt       # Application dependencies
└── src/
    ├── components/        # Reusable dialogs, headers, and cards
    ├── database/          # Supabase connection and data operations
    ├── pipelines/         # Face and voice recognition
    ├── screens/           # Home, teacher, and student screens
    └── ui/                # Shared layout and styling
```

## Current scope

This is a course-based learning project. Recognition depends on enrollment quality, lighting, and audio conditions; teachers review results before saving. It is not presented as a production deployment.

Useful next steps include database migrations, repeatable recognition evaluation, automated tests, and deployment documentation.

## Acknowledgement

This repository is Tanush's copy of the SnapClass course project taught by Shradha Khapra / Apna College. The original course implementation remains credited to its creators. The earlier course history is preserved on `course-history-backup`; follow the original project's applicable license and usage terms.
