# WinStreak
**Gamified AI Calorie Tracker**

## Why
I've always struggled to lose weight over the long term. At the same time, I've always been addicted to video games. Climbing ranks, progressing, and moving up leaderboards is something I've always enjoyed.
The idea behind this app is to make weight loss fun and healthily competitive.

---

## How it works

### Calorie tracking

Via a photo of your meal

<img width="292" height="633" alt="Simulator Screenshot - iPhone 16e - 2026-01-14 at 14 12 00" src="https://github.com/user-attachments/assets/a0e52b4e-8234-42fd-a632-8967249f7fbd" />

Via a text or voice description of the meal

<img width="292" height="633" alt="Simulator Screenshot - iPhone 16e - 2026-01-14 at 14 12 20" src="https://github.com/user-attachments/assets/62987ca6-908d-4a8a-9382-5c51559c4b7b" />


Via a food database (data provided by OpenFoodFacts)

<img width="292" height="633" alt="Simulator Screenshot - iPhone 16e - 2026-01-14 at 14 12 29" src="https://github.com/user-attachments/assets/bbcb609b-47d9-4190-bc7f-48154d143e05" />


---

### History
Each week, track your progress, monitor your BMI, and see your improvements in real time.

<img width="292" height="633" alt="Simulator Screenshot - iPhone 16e - 2026-01-14 at 14 15 45" src="https://github.com/user-attachments/assets/7c30f001-af56-4960-854e-99579de22a77" />


---

### Gamification

A streak system pushes you to never give up.
Every time you stay within your daily calorie limit, you earn 1 streak point.

<img width="292" height="633" alt="Simulator Screenshot - iPhone 16e - 2026-01-14 at 14 16 25" src="https://github.com/user-attachments/assets/846b9345-cb21-4254-a766-a9206d250c31" />


A ranking system lets you climb ranks every time you respect your daily calorie target.
Be careful not to slip up if you don't want to lose points.

<img width="292" height="633" alt="Simulator Screenshot - iPhone 16e - 2026-01-14 at 14 16 27" src="https://github.com/user-attachments/assets/c387a6f3-c018-4e7d-9759-25db3b72df0e" />

<img width="292" height="633" alt="Simulator Screenshot - iPhone 16e - 2026-01-14 at 14 16 32" src="https://github.com/user-attachments/assets/70bee43f-3f72-4887-9467-4107ab6ce3aa" />


A trophy system motivates completionists to keep using the app.

<img width="292" height="633" alt="Simulator Screenshot - iPhone 16e - 2026-01-14 at 14 16 41" src="https://github.com/user-attachments/assets/a9398a13-6951-4d48-aab2-d3249c494349" />


---

## Infrastructure & DevOps

### Architecture Overview

```
                    ┌─────────────────────────────────────────────────────────┐
                    │                        VPS                              │
                    │  ┌───────────────────────────────────────────────────┐  │
   Internet         │  │           Nginx Proxy Manager                     │  │
       │            │  │         (Reverse Proxy + SSL)                     │  │
       │            │  │              vpsapp.eatiq.be                      │  │
       ▼            │  └─────────────────┬────────────────────┬────────────┘  │
 ┌──────────┐       │                    │                    │               │
 │  Users   │───────┼────────────────────┼────────────────────┘               │
 └──────────┘       │                    ▼                                    │
                    │  ┌─────────────────────────────────────────────────┐    │
                    │  │              Docker Network (LAN)               │    │
                    │  │  ┌──────────┐  ┌──────────┐  ┌──────────────┐   │    │
                    │  │  │  back1   │  │  back2   │  │  PostgreSQL  │   │    │
                    │  │  │ (Apache  │  │ (Apache  │  │    17.6      │   │    │
                    │  │  │  +WSGI)  │  │  +WSGI)  │  │              │   │    │
                    │  │  │ +Celery  │  │ +Celery  │  └──────────────┘   │    │
                    │  │  └──────────┘  └──────────┘                     │    │
                    │  │        │              │       ┌──────────────┐  │    │
                    │  │        └──────┬───────┘       │  RabbitMQ    │  │    │
                    │  │               │               │  (Message    │  │    │
                    │  │               ▼               │   Broker)    │  │    │
                    │  │       ┌──────────────┐        └──────────────┘  │    │
                    │  │       │ Persistent   │                          │    │
                    │  │       │    Data      │        ┌──────────────┐  │    │
                    │  │       └──────────────┘        │ Uptime Kuma  │  │    │
                    │  │                               │ (Monitoring) │  │    │
                    │  │                               └──────────────┘  │    │
                    │  └─────────────────────────────────────────────────┘    │
                    └─────────────────────────────────────────────────────────┘
```

---

### Reverse Proxy & SSL Termination

**Nginx Proxy Manager** handles all incoming traffic:
- **SSL/TLS termination** with Let's Encrypt certificates (auto-renewal)
- **HTTP/2 support** for improved performance
- **Domain routing**: `vpsapp.eatiq.be` → backend containers
- **API-driven configuration** for automated deployments

---

### High Availability & Zero-Downtime Deployment

The backend runs with **Blue-Green deployment** for zero downtime:

| Component | Container 1 | Container 2 | Purpose |
|-----------|-------------|-------------|---------|
| Backend   | `back1`     | `back2`     | Active/Standby |

**Deployment process:**
1. Build new Docker image
2. Start inactive container with new image
3. Health check & warm-up (30s)
4. Update Nginx Proxy Manager via API to route traffic to new container
5. Stop old container
6. Restart RabbitMQ workers

This ensures **zero downtime** during deployments.

---

### Container Stack

| Service | Image | Role |
|---------|-------|------|
| **back1/back2** | `eatiq_app_back` (Debian Bookworm) | Django + Apache + mod_wsgi + Celery workers |
| **db** | `postgres:17.6-bookworm` | Primary database with pg_trgm extension |
| **rabbit** | `rabbitmq:4.1.4` | Message broker for async tasks |
| **uptime-kuma** | `louislam/uptime-kuma` | Infrastructure monitoring |

All containers communicate via an isolated Docker bridge network (`lan`).

---

### CI/CD Pipeline

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Dev Local  │────▶│  buildProd   │────▶│   SCP/SSH    │────▶│  run.sh      │
│  (test_env)  │     │    .sh       │     │   to VPS     │     │  (deploy)    │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
       │                    │                    │                    │
       │                    │                    │                    │
       ▼                    ▼                    ▼                    ▼
  Code changes        Static files         Sync files to       Docker build
  in test_env         collection           remote server       Blue-green swap
                      Env switching                             Proxy update
                      rsync to prod_env
```

**Backend CI/CD (`buildProd.sh`):**
1. Collect Django static files
2. Swap environment variables (`.env.dev` ↔ `.env.prod`)
3. Rsync to local `prod_env` (excluding large data files)
4. Generate `requirements.txt`
5. Deploy via SCP to VPS
6. Execute remote `run.sh` for Docker orchestration

**Frontend CI/CD:**
- **EAS Build** (Expo Application Services) for iOS/Android builds
- Automated versioning with `autoIncrement` for production builds
- Three build profiles: `development`, `preview`, `production`

---

### Networking & Security

| Layer | Technology | Purpose |
|-------|------------|---------|
| **Edge** | Nginx Proxy Manager | Reverse proxy, SSL termination, WAF |
| **Transport** | HTTPS (TLS 1.3) | Encrypted client-server communication |
| **Application** | JWT (djangorestframework-simplejwt) | Stateless authentication |
| **Container** | Docker bridge network | Service isolation |
| **Database** | PostgreSQL with limited exposure | Port 5432 only within Docker network |

---

### Persistent Storage

```
/root/eatiq_app_env/
├── back_env/
│   ├── docker-compose.yml
│   ├── run.sh
│   └── image/
│       └── Dockerfile
└── persistentData/
    ├── db/                    # PostgreSQL data volume
    ├── db.sqlite3             # Legacy SQLite (if used)
    └── media/                 # User uploaded files
```

Data persistence is achieved through Docker volume mounts, ensuring data survives container restarts and updates.

---

### Monitoring & Alerting

**Uptime Kuma** monitors the entire infrastructure:

| Feature | Description |
|---------|-------------|
| **HTTP(S) Monitoring** | Endpoint availability checks on API routes |
| **Container Monitoring** | Docker container health status |
| **Response Time Tracking** | Latency metrics and historical graphs |
| **Alerting** | Notifications via email, Discord, Slack, etc. |
| **Status Page** | Public/private status page for uptime visibility |
| **Certificate Expiry** | SSL certificate expiration alerts |

Additional logging:
- **Apache logs**: `${APACHE_LOG_DIR}/error.log` and `access.log`
- **Docker logs**: `docker logs -f eatiq_app_back1`
- **Celery logs**: Background task execution logs (info level)

---

## Code

### Frontend

#### React Native
I use React Native to handle the frontend.
It allows me to develop for both iOS and Android with most of the code shared.

#### Expo
I use Expo to easily package the app into `.ipa` and `.aab` files and publish it on the App Store and Play Store.

---

### Backend

#### Django
I use Django (a Python framework) for the backend.
Built-in JWT authentication makes it easy to handle auth without reinventing the wheel, and the serializer and ORM systems make communication with the frontend and the database straightforward.

#### Apache + mod_wsgi
Production-grade WSGI server running Django behind Apache HTTP Server.
`WSGIDaemonProcess` ensures proper process management and isolation.

#### PostgreSQL
The database used is PostgreSQL 17.6.
The trigram indexing system (`pg_trgm`) allows the food database to respond in **10–50 ms** despite having more than **200,000 entries**.

#### RabbitMQ + Celery
RabbitMQ is used as the message broker for **Celery** workers.
Handles scheduled tasks (django-celery-beat) and async job processing.

---

## Download

**iOS**
https://apps.apple.com/be/app/streakgg/id6754155267?l=fr-FR

**Android**
https://play.google.com/store/apps/details?id=com.anonymous.calcounter&hl=fr
