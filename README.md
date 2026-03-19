<h1 align="center">Test Sniper</h1>

<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Inter&weight=400&size=20&pause=300&color=888888&center=true&vCenter=true&width=500&lines=Slot+monitoring%2C+instant+notifications;Built+with+FastAPI+%26+React;Live+at+testsniper.net" alt="Typing SVG" />
</p>

<p align="center">
  <a href="https://testsniper.net">
    <img src="https://img.shields.io/badge/Live-testsniper.net-4CAF50?style=for-the-badge" />
  </a>
</p>

---

### Tech Stack

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" />
  <img src="https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" />
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" />
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" />
</p>

---

### What it does

Test Sniper monitors booking platforms for newly available slots and notifies users the instant one opens. Instead of repeatedly refreshing a page and missing the window, users register their target and get alerted in real time so they can act before anyone else.

---

### How it works

```
  Scheduler
     │
     ▼
  Poller  ──────────────────────────────────────────────────►  External booking sites
     │                                                          (HTTP polling)
     ▼
  PostgreSQL  (slot state, user subscriptions, notification log)
     │
     ▼
  Notifier  ──►  User (email / in-app)
     │
     ▼
  FastAPI  ◄──►  React frontend  ◄──►  User (browser)
```

A background scheduler polls target URLs on a configurable interval. When a slot transitions from unavailable to available, the change is persisted and the notification service dispatches alerts to all subscribed users. The FastAPI backend handles user accounts, subscriptions, and exposes the data to the React frontend.

---

### Key Engineering Decisions

- **Polling over webhooks**: Target platforms don't expose event APIs, so the poller fetches and diffs slot state on each cycle. Interval and concurrency limits are tuned per-target to stay within acceptable request rates.
- **State diffing in the DB**: Slot availability is stored per-check, so the system can detect transitions (unavailable to available) reliably across restarts without holding state in memory.
- **Containerised deployment**: The frontend, backend, and PostgreSQL database each run as their own Docker container, making it easy to scale, restart, or replace components independently. Images are built and pushed automatically via GitHub Actions on every push to the private repo.
- **Async-first backend**: FastAPI with async endpoints keeps the API responsive under load even when the poller and notifier are running concurrently in the same process space.

---

*Source is maintained in a private repository.*
