# SpeedieReadie

SpeedieReadie is a speed reading web application built with **React + TypeScript (Vite)** for the frontend, **Django + Django REST Framework** for the backend, and **Firebase** (Authentication on the client, Firestore as the database). Upload texts — or import them from files, EPUB/PDF documents, or URLs — and read them back at your own customized speed.

> **Note:** This project is no longer under active development. `main` is the working branch.

---

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Project Structure](#project-structure)
- [Known Limitations](#known-limitations)
- [Demo](#demo)
- [Contributing](#contributing)
- [License](#license)

---

## Features

- Upload and view texts in a speed-reading format
- Customize the reading speed and settings
- Import from plain text, PDF, EPUB, or a URL
- Personal library stored per user in Firestore
- Firebase Authentication on the frontend

---

## Tech Stack

- **Frontend:** React 18, TypeScript, Vite, React Router, Axios, Firebase JS SDK, Bootstrap
- **Backend:** Django, Django REST Framework, django-cors-headers
- **Database:** Firebase Firestore (via the Firebase Admin SDK)
- **Dev database:** SQLite (local only — `db.sqlite3` is git-ignored, never commit it)

---

## Prerequisites

- Python 3.10+
- Node.js 18+ and npm
- A Firebase project with:
  - Firestore Database enabled
  - Email/Password sign-in enabled under **Authentication**
  - A **service account key** JSON for the backend — Firebase Console → Project Settings → Service accounts → Generate new private key
  - A **web app config** for the frontend — Firebase Console → Project Settings → Your apps

---

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/rupnowo/SpeedieReadie-Webapp.git
cd SpeedieReadie-Webapp
```

### 2. Backend setup (Django)

```bash
cd speedreadingapp
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

Create `speedreadingapp/.env` (see `.env.example` — never commit the real `.env`):

```
SECRET_KEY=<a-random-secret-key>
FIREBASE_CERT_PATH=/absolute/path/to/your-firebase-adminsdk.json
```

Generate a secret key with:

```bash
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
```

Create the local database and start the server:

```bash
python manage.py migrate
python manage.py runserver
```

The API will be available at `http://localhost:8000/api/`. The backend initializes the Firebase Admin SDK at startup, so it needs your service-account key and network access to Google.

### 3. Frontend setup (React + Vite)

```bash
cd speedreadingapp/frontend
npm install
```

Create `speedreadingapp/frontend/.env` (see `.env.example` — never commit the real `.env`):

```
VITE_FIREBASE_API_KEY=
VITE_FIREBASE_AUTH_DOMAIN=
VITE_FIREBASE_PROJECT_ID=
VITE_FIREBASE_STORAGE_BUCKET=
VITE_FIREBASE_MESSAGING_SENDER_ID=
VITE_FIREBASE_APP_ID=
```

Start the dev server:

```bash
npm run dev
```

The app will be available at `http://localhost:5173`. The Vite dev server proxies `/api` requests to the Django backend at `http://localhost:8000`.

---

## Project Structure

```
speedreadingapp/
├── manage.py
├── requirements.txt
├── .env.example
├── speedreadingapp/      # Django project package (settings, urls)
├── users/                # user profiles & auth API
├── UserLibrary/          # library / import / speed-read API (Firestore-backed)
├── api/                  # legacy endpoints (not wired into urls)
└── frontend/             # React + TypeScript Vite app
    ├── .env.example
    └── src/
        ├── api.ts            # backend API client
        ├── config/           # Firebase + axios configuration
        ├── pages/            # HomePage, Library, AddText, Login, ...
        └── components/
```

---

## Known Limitations

- Some frontend API paths in `src/api.ts` don't match the backend routes yet (library list, add-text, and delete call different paths than the backend serves); import-from-file and import-from-url line up end to end.
- The Django `LoginView` is an unimplemented stub — the frontend logs in through Firebase Authentication directly.
- An older `NoAuthSpeedieReadie` branch referenced in earlier docs no longer exists; `main` is the only branch.

---

## Demo

A 3-minute demo of an earlier version: https://www.youtube.com/watch?v=h5C-h2_8SuQ

---

## Contributing

This project is currently **not** accepting contributions, as development is no longer active. Feel free to fork it for your own use.

---

## License

This project is licensed under the MIT License — see [LICENSE](LICENSE).
