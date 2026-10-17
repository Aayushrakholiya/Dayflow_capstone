<div align="center">

# 📅 DayFlow — Smart Calendar

**A smart calendar app that makes your routine productive — plan events, sync Google & Microsoft calendars, get smart notifications, check the weather for your plans, and never miss an invite.**

React 19 · Express 5 · Prisma · PostgreSQL · Redis · Firebase

<br>

![React](https://img.shields.io/badge/React_19-20232A?style=flat-square&logo=react&logoColor=61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Express](https://img.shields.io/badge/Express_5-000000?style=flat-square&logo=express&logoColor=white)
![Prisma](https://img.shields.io/badge/Prisma_7-2D3748?style=flat-square&logo=prisma&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis_DC382D?style=flat-square&logo=redis&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase_FCM-FFCA28?style=flat-square&logo=firebase&logoColor=black)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)

</div>

---

## 📌 Table of Contents

- [About](#-about)
- [Repository Structure](#-repository-structure)
- [Features](#-features)
- [Architecture](#%EF%B8%8F-architecture)
- [Project Structure](#%EF%B8%8F-project-structure)
- [Getting Started](#-getting-started)
- [Team](#-team)

## 👋 About

DayFlow (built by team **CoffeeCoders**) is a full-stack smart calendar application. It does everything a calendar should — events, tasks, reminders, invites — and then goes further: it syncs your existing Google and Microsoft calendars in the background, sends smart event notifications through Firebase Cloud Messaging and email, tells you the weather where your event happens, and helps you find and attach locations with an interactive map.

## 🗂️ Repository Structure

This monorepo contains the complete application (frontend + backend). It is also split into two standalone deployment repos, which is how the app is actually deployed to production:

| Repo | What it is |
| --- | --- |
| **[Dayflow_capstone](https://github.com/Aayushrakholiya/Dayflow_capstone)** *(this repo)* | The complete application — full source in one place, Docker Compose for local infrastructure |
| **[dayflow-frontend](https://github.com/Aayushrakholiya/dayflow-frontend)** | Frontend only, extracted and deployed separately (Vercel) |
| **[dayflow-backend](https://github.com/Aayushrakholiya/dayflow-backend)** | Backend only, extracted and deployed separately (Vercel) |

> **Why the split?** Deploying the whole application at once proved impractical, so the frontend and backend were separated into their own repos and deployed independently — and it works exactly as intended. The two deployment repos are kept in sync with the source in this one.

## ✨ Features

- 📆 **Full calendar** — create, edit, and manage events and tasks with a modern calendar view
- 🔄 **Google & Microsoft Calendar sync** — OAuth2 import of your existing calendars, kept fresh by a background worker
- 🧠 **Smart notifications** — Firebase Cloud Messaging push notifications + email, with scheduled reminder jobs
- ✉️ **Event invites** — invite guests by email, they RSVP right from the invite
- 🌦️ **Weather aware** — see the forecast for your event's location
- 🗺️ **Location services** — pick and search event locations on an interactive Leaflet/Google map
- 👤 **User accounts** — signup, login, password reset, and user profiles with secure (argon2-hashed) auth
- ⚡ **Real-time** — WebSocket support for live updates

## 🏗️ Architecture

```text
┌─────────────────────────┐         ┌──────────────────────────────────┐
│  Frontend (React 19)    │  REST   │  Backend (Express 5 + TS)        │
│  Vite · MUI · Leaflet   │ ◀─────▶ │  auth · events · tasks · users   │
│  Firebase FCM client    │  + WS   │  calendar sync · weather · mail  │
└─────────────────────────┘         └───────┬──────────────┬───────────┘
                                            │              │
                              ┌─────────────▼───┐   ┌──────▼─────────┐
                              │  PostgreSQL     │   │  Redis + BullMQ│
                              │  (Prisma ORM)   │   │  calendar      │
                              │  users · events │   │  worker jobs   │
                              │  tasks · tokens │   │  notifications │
                              └─────────────────┘   └────────────────┘
```

- **PostgreSQL + Prisma 7** — users, events, tasks, notifications, imported events, and external calendar tokens
- **Redis + BullMQ** — a dedicated worker process processes calendar-sync and notification jobs off the request path
- **Firebase FCM** — push notifications straight to the browser
- **Docker Compose** — spins up Postgres 16 and Redis 7 for local development

## 🗂️ Project Structure

```text
Dayflow_capstone/
├── frontend/                 ⚛️ React 19 + Vite + MUI
│   ├── src/pages/            Home, Login, Signup, Main, About, Help
│   ├── src/components/       Calendar view, sidebar, conflict checker,
│   │                         weather widget, location services, invites
│   └── src/services/         Firebase Cloud Messaging client
├── backend/                  🚂 Express 5 + TypeScript
│   ├── src/                  Routes: signup, login, events, tasks, users,
│   │                         Google/Microsoft calendar, weather, invites
│   ├── src/services/         FCM + notification services
│   ├── src/workers/          BullMQ calendar-sync worker
│   └── prisma/               Schema + migrations
└── docker-compose.yml        🐳 Postgres 16 + Redis 7 for local dev
```

## 🚀 Getting Started

**Prerequisites:** Node.js, Docker, and a Firebase project (for push notifications) plus Google/Microsoft OAuth credentials (for calendar sync).

```bash
git clone https://github.com/Aayushrakholiya/Dayflow_capstone.git
cd Dayflow_capstone

# 1. start local infrastructure
docker compose up -d          # Postgres 16 + Redis 7

# 2. backend
cd backend
npm install
npx prisma migrate deploy     # set up the database
npm run dev                   # API server on ts-node/nodemon
# (separate terminal) npm run dev:worker   # background jobs

# 3. frontend (new terminal)
cd ../frontend
npm install
npm run dev                   # Vite dev server
```

Production deploys happen from the two split repos: `dayflow-frontend` and `dayflow-backend` (both configured for Vercel via `vercel.json`).

## 👥 Team

Built by **CoffeeCoders** for our capstone project.

---

<div align="center">

⭐ **Star this repo if you find it useful** ⭐

</div>
