# AimTrainer

A browser-based FPS aim training app integrated with Discord. Users invoke a Discord bot command to receive a unique session link, complete aim exercises in the browser, and view their stats back in Discord, no account creation required.

---
## Web app Preview

![Landing Page](assets/landing_page.png)


## Features

- **Three training exercises:**
  - **Gridshot (Flicking)** — Click targets on a 5×5 grid as fast as possible. Measures score, accuracy, and average reaction time.
  - **Precision** — Targets spawn and despawn quickly. Three difficulty levels (Easy / Normal / Hard) adjust the disappear window. 30 targets per session.
  - **Tracking** — A single target strafes left/right. Keep your crosshair on it to drain its health. Measures time-on-target % and tracking score.
- **Discord integration** — `/aimtrain` generates a one-time token link; `/mystats` shows your performance summary in Discord.
- **Per-game sensitivity conversion** — Supports Valorant, CS2, Apex Legends, Overwatch 2, and Rainbow Six Siege.
- **Persistent stat tracking** — Session results are saved to PostgreSQL and queryable via the API or Discord bot.
- **Full-screen pointer-lock gameplay** with crosshair, pause/resume, and Web Audio sound effects.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 19, Vite, Three.js |
| Backend | Django 4.2, Django REST Framework |
| Database | PostgreSQL |
| Discord Bot | discord.py |

---

## Project Structure

```
aim_trainer_project/
├── app/                  # React + Vite frontend
│   └── src/
│       ├── App.jsx       # Routing
│       ├── landing.jsx   # Landing page
│       ├── mainmenu.jsx  # Exercise selection & settings
│       ├── gridshot.jsx  # Flicking game
│       ├── precision.jsx # Precision game
│       ├── tracking.jsx  # Tracking game
│       ├── results.jsx   # Post-session results
│       ├── gamesettings.jsx  # Sensitivity / volume context
│       └── sounds.js     # Web Audio effects
├── aimtrainer/           # Django backend
│   └── api/              # REST API (models, views, serializers)
└── discord_bot/
    └── bot.py            # /aimtrain and /mystats commands
```

---

## Getting Started

### Prerequisites

- Node.js 18+
- Python 3.10+
- PostgreSQL

---

### 1. Frontend

```bash
cd app
npm install
npm run dev        # http://localhost:5173
```

---

### 2. Backend

```bash
cd aimtrainer
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
pip install django djangorestframework django-cors-headers psycopg2-binary python-dotenv
```

Create `aimtrainer/.env`:

```env
DB_NAME=aimtrainer
DB_USER=postgres
DB_PASSWORD=your_password
DB_HOST=localhost
DB_PORT=5432
```

```bash
python manage.py migrate
python manage.py runserver   # http://localhost:8000
```

---

### 3. Discord Bot

```bash
cd discord_bot
pip install discord.py python-dotenv requests
```

Create `discord_bot/.env`:

```env
DISCORD_TOKEN=your_bot_token
DJANGO_API_URL=http://localhost:8000/api
SITE_URL=http://localhost:5173
```

```bash
python bot.py
```

---

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/token/` | Generate a session token for a Discord user |
| `POST` | `/api/token/refresh/` | Refresh an existing token |
| `GET` | `/api/user/?token=<token>` | Resolve a token to user info |
| `POST` | `/api/session/` | Save a completed game session |
| `GET` | `/api/stats/<discord_id>/` | Summary stats across all exercises |
| `GET` | `/api/stats/<discord_id>/<exercise>/` | Detailed stats for one exercise |

Tokens expire after 30 minutes.

---

## Discord Commands

| Command | Description |
|---|---|
| `/aimtrain` | Generate a unique session link to start training |
| `/mystats` | Display your aim training stats in Discord |

---
