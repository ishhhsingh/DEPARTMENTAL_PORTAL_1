# 🎓 DPT Portal — SMIT Departmental Timetable Portal

## ⚡ Quick Start (Docker — Recommended)

### Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running

### Run the whole project
```bash
# 1. Clone / unzip the project
cd dpt-portal

# 2. Start everything (MySQL + Backend + Frontend)
docker-compose up --build

# First build takes ~3-5 minutes (downloads Java, Node, MySQL images)
# On subsequent runs: docker-compose up  (much faster)
```

### 3. Open in browser
```
http://localhost
```

### Stop
```bash
docker-compose down          # stops containers (data preserved)
docker-compose down -v       # stops + deletes database (fresh start)
```

---

## 🔑 Default Credentials

| Role    | Username    | Password   |
|---------|-------------|------------|
| Admin   | admin       | admin123   |
| Faculty | CA-FAC001   | 12345      |
| Faculty | CA-FAC002   | 12345      |
| Student | 202300001   | 123        |

> Students: use your Registration Number as username. Default password is `123`.

---

## 🗄️ Database

MySQL 8.0 runs in Docker with **persistent volume** (`mysql_data`).  
Data survives container restarts. Only `docker-compose down -v` deletes it.

Fresh databases are seeded automatically from `backend/src/main/resources/seed/schedule_seed.csv`, which was generated from the uploaded Even Semester 2026 timetable workbook. The original workbook is kept at `data/TIME_TABLE_EVEN_SEM_2026_Mini_project.xlsx`.

Connect with MySQL Workbench / DBeaver:
- Host: `localhost:3306`
- Database: `timetable_db`
- User: `dptuser` / Password: `dptpass2026`
- Root Password: `dptroot2026`

---

## 🛠️ Local Development (without Docker)

### Backend
```bash
cd backend
# Set env vars or edit application.properties DB credentials
mvn spring-boot:run
# Runs on http://localhost:8080
```

### Frontend
```bash
cd frontend
npm install
npm start
# Runs on http://localhost:3000 (proxies /api → localhost:8080)
```

---

## 📁 Project Structure

```
dpt-portal/
├── docker-compose.yml        ← Run everything with one command
├── backend/
│   ├── Dockerfile
│   └── src/main/java/com/timetable/
│       ├── config/           ← Security, CORS, DataInitializer (seeds DB)
│       ├── controller/       ← REST endpoints
│       ├── entity/           ← JPA entities (User, Schedule)
│       ├── service/          ← Business logic
│       └── security/         ← JWT filter
├── frontend/
│   ├── Dockerfile
│   ├── nginx.conf            ← Serves React + proxies /api to backend
│   └── src/
│       ├── pages/            ← StudentDashboard, FacultyDashboard, AdminDashboard, LoginPage
│       ├── components/       ← Navbar, TimetableView, UploadZone
│       └── services/api.js   ← Axios API calls
├── data/                     ← Uploaded source timetable workbook
├── backend/src/main/resources/seed/schedule_seed.csv ← Startup DB seed
└── sample_schedule.csv       ← Sample CSV for bulk upload
```

---

## 📋 CSV Upload Format (Admin)

```csv
program,day,timeSlot,subject,facultyId,facultyName,room
BCA,Monday,9:00-10:00 AM,Machine Learning,CA-FAC004,Mr. Dipendra Gurung,A208
```

---

## 🌐 Ports

| Service  | Port |
|----------|------|
| Website  | 80   |
| Backend  | 8080 |
| MySQL    | 3306 |
