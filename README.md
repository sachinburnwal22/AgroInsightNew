# 🌾 AgroInsight - Smart Agricultural Intelligence Center

[![React](https://img.shields.io/badge/React-19-blue.svg?logo=react&logoColor=white)](https://react.dev/)
[![Next.js](https://img.shields.io/badge/Next.js-16.2-black.svg?logo=nextdotjs&logoColor=white)](https://nextjs.org/)
[![Django](https://img.shields.io/badge/Django-5-092E20.svg?logo=django&logoColor=white)](https://www.djangoproject.com/)
[![Python](https://img.shields.io/badge/Python-3.12-3776AB.svg?logo=python&logoColor=white)](https://www.python.org/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-v4-06B6D4.svg?logo=tailwindcss&logoColor=white)](https://tailwindcss.com/)
[![Framer Motion](https://img.shields.io/badge/Framer_Motion-v12-0055FF.svg?logo=framer&logoColor=white)](https://www.framer.com/motion/)
[![Gemini AI](https://img.shields.io/badge/Gemini_AI-2.5_Flash-8E75C2.svg?logo=google-gemini&logoColor=white)](https://deepmind.google/technologies/gemini/)

> **AgroInsight** is a futuristic, real-time AI-powered agricultural command center and dashboard designed to help Indian farmers make smarter decision, track crops, analyze weather, navigate government schemes, and listen to simplified farming news in their regional language.

---

## 🛰️ System Architecture

```
                 +---------------------------------------+
                 |          Next.js Frontend             |
                 |      (Dashboard, Map, AgriIntel)      |
                 +-------------------+-------------------+
                                     |
                                     |  HTTP REST Requests
                                     v
                 +-------------------+-------------------+
                 |           Django API                  |
                 |    (Auth, Models, Views, Sync)        |
                 +----+--------------+--------------+----+
                      |              |              |
    SQLite Database   |              |              |  Google Gemini API
    (Local Storage)   |              |              |  (Summaries & Translation)
                      v              |              v
             [( DB: SQLite )]        |       [🤖 Gemini AI]
                                     |
                                     v  PIB Feeds Sync
                           [📰 Official Gov RSS]
```

---

## ⚡ Main Modules

### 1. 📊 Agricultural Analytics & Dashboard
- **Interactive Map**: State-level data visualization of crop health, soil composition, and irrigation status.
- **Dynamic Stats**: Interactive graphs displaying average land holdings, active water usage, and crop distributions.

### 2. 📰 AgriIntel Center (Real-Time News & Policies)
- **Breaking News Ticker**: Smooth, legible horizontal marquee showing urgent farming announcements.
- **Hero Slider**: Sliding hero carousel cycling through top trending agricultural topics.
- **Language Switcher**: Toggles UI language instantly between English, Hindi, Punjabi, Bengali, and Tamil.
- **News Cards**: Rich glassmorphic animations showing publish dates, category badges, and sources.

### 3. 🏛️ Government Schemes & Subsidies Directory
- **State-Wise Schemes**: Dynamic list filters showing state availability (e.g. Punjab, Maharashtra) and crop dependencies.
- **AI Scheme Explainer**: Integrated Gemini chatbot explaining how to qualify and apply step-by-step in the farmer's language.

### 4. 🧠 AI Smart Advisor & Narrator (News Detail)
- **AI Farmer Advice**: Simple, translated bullet points outlining the action advice of news articles.
- **Voice Reader (TTS)**: Interactive audio controls (Play, Stop) utilizing native Web Speech synthesis to read advice aloud.

---

## 📂 Repository Directory Structure

```
AgroInsight/
 ├── frontend/                 # Next.js React client application
 │    ├── app/                 # Routes and layouts (agri-intel, crop-recommendation, community, etc.)
 │    ├── components/          # UI elements (FloatingNavbar, map graphs, advisory widgets)
 │    └── public/              # Graphic assets and icons
 │
 └── backend/                  # Django REST API application
      ├── agroapi/             # API views, models, services, and migrations
      ├── agroinsight/         # Django project configuration
      └── scripts/             # Explicit, non-destructive data migration tools
```

---

## ⚙️ Direct Setup & Launch

<details>
<summary>📂 Click to expand Setup Guide</summary>

### 1. Prerequisite Checks
Ensure you have **Python 3.12**, **Node.js >= 20**, and **npm** installed.

### 2. Launch Django Backend
```powershell
# Navigate to backend
cd backend

# Install Python dependencies
python -m pip install -r requirements.txt

# Set required local development configuration in PowerShell
$env:DJANGO_DEBUG = "true"
$env:DJANGO_SECRET_KEY = "replace-with-a-local-development-key"
# Optional integrations:
# $env:GEMINI_API_KEY = "..."
# $env:RAZORPAY_KEY_ID = "..."
# $env:RAZORPAY_KEY_SECRET = "..."

# Create/upgrade the separate Django database
python manage.py migrate

# Start Django API server
python manage.py runserver 127.0.0.1:8000
```
API runs locally at **`http://127.0.0.1:8000`**.

The Django API uses the separate `backend/migration_data/django.sqlite3` database and does not read the legacy `backend/.env` or write to the original Laravel SQLite files. A read-only backup and new destination are used for data migration.

### 3. Launch Next.js Frontend
```bash
# Open a new terminal inside frontend folder
cd frontend

# Install Node dependencies
npm install

# Start local React development server
npm run dev
```
Client runs locally at **`http://localhost:3000`**.

</details>

---

## 👨‍💻 Verification Guide

- Open **`http://localhost:3000/agri-intel`** to enter the AgriIntel module.
- Try switching languages in the header selector to verify immediate UI localizations.
- Open the notification bell in the navbar to check current alerts.
- Click **AI Explainer** on any scheme or open a news article to test the AI translation and hands-free **Voice Reader**.
