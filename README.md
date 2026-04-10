# 💬 Chat Application — Backend Developer Assignment

Welcome! This is a take-home assignment for a **Junior Backend Developer** position.  
Read all instructions carefully before starting.

---

## 📋 Overview

You are required to build a **real-time chat application** backend using **Python** and **FastAPI**.  
The assignment is divided into two groups. Complete **all tasks in Group A** and **one task from Group B**.

**Estimated Time:** 6–8 hours  
**Submission:** Push your code to a public GitHub repository and share the link.

---

## ✅ Requirements Before You Start

Make sure you have the following installed:

- Python 3.10+
- PostgreSQL
- Git

---

## Group A — Mandatory Tasks

> All three tasks below are required.

---

### Task 1: Environment & Dependencies

Set up a clean, isolated Python project for this application.

**Your task:**

- Create a virtual environment using `python3 -m venv venv`
- Install the following packages and pin them in a `requirements.txt`:
  - `FastAPI` — web framework
  - `Uvicorn` — ASGI server
  - `SQLAlchemy` or `SQLModel` — ORM
  - `psycopg2` — PostgreSQL adapter
  - `passlib` — password hashing
  - `python-jose` — JWT handling
- The application must run with `uvicorn app.main:app --reload`

**Deliverable:** A working project that installs cleanly from `requirements.txt` and starts without errors.

---

### Task 2: JWT Authentication & Role-Based Access Control (RBAC)

Implement a secure authentication system with user roles.

**Your task:**

- Design a `User` database model using SQLAlchemy that includes a `role` field. Roles should be either `admin` or `user`.
- Implement the following endpoints:

  | Method | Endpoint | Description |
  |---|---|---|
  | `POST` | `/auth/signup` | Accept user credentials, hash the password, and store the user with an assigned role |
  | `POST` | `/auth/login` | Verify credentials and return a JWT token signed with HS256, with the user's role embedded in the token payload |

- Implement a **reusable FastAPI dependency** that can restrict access to any route based on the user's role.

**Constraints:**
- Passwords must never be stored in plain text.
- The JWT token must include an expiry.
- Role checks must be enforced via a dependency, not hardcoded per route.

**Deliverable:** Working `/signup` and `/login` endpoints, and a demonstrated RBAC dependency used on at least one protected route.

---

### Task 3: Protected WebSocket Chat

Build a real-time chat system secured with JWT authentication.

**Your task:**

- Create a WebSocket endpoint at `/ws/{room_id}` that handles real-time chat connections.
- Secure the WebSocket connection by verifying the JWT token (passed as a query parameter).
- Implement the following behavior:
  1. **On connection:** Fetch and send the most recent messages from the database for that room using **cursor-based pagination**.
  2. **On incoming message:** Broadcast the message to all currently connected clients in the same room, and save it to the database.
  3. **On disconnection:** Clean up the connection gracefully.

**Constraints:**
- Unauthenticated connections must be rejected.
- Multiple rooms must be supported simultaneously.
- Messages must persist in PostgreSQL.

**Deliverable:** A working WebSocket endpoint that supports multi-client, multi-room real-time messaging.

---

## Group B — Choose Any One Task

> Complete **one** of the two tasks below.

---

### Task 1: PostgreSQL Persistence & Data Modelling

Design and implement the full database layer for the application.

**Your task:**

- Define the following SQLAlchemy models:

  | Model | Required Fields |
  |---|---|
  | `User` | id, username, email, hashed password, role |
  | `Room` | id, name, description, created_at |
  | `Message` | id, content, timestamp, user_id (FK), room_id (FK) |

- Define proper **relationships** between models (e.g., a Room has many Messages, a User has many Messages).
- Implement **cursor-based pagination** when fetching message history for a room — do not use offset-based pagination.

**Constraints:**
- Use SQLAlchemy ORM — raw SQL queries are not acceptable.
- Foreign key constraints must be enforced at the database level.

**Deliverable:** Fully defined models with relationships, and a working paginated message history query.

---

### Task 2: Extended Feature (Your Choice)

Implement **one** of the following features and document your choice in the README:

- **Redis Pub/Sub** — Replace the in-memory WebSocket connection manager with Redis Pub/Sub to support running multiple server instances.
- **Rate Limiting** — Add per-user message rate limiting on the WebSocket endpoint to prevent spam. Define and justify your chosen limits.
- **Message Read Receipts** — Track which users have read which messages. Design the data model and expose an endpoint to mark messages as read.

**Deliverable:** Working implementation with a brief explanation in your README of what you built and why you made the design choices you did.

---

## 📁 Expected Project Structure

Your submission should follow a structure similar to this:

```
chat-app/
├── app/
│   ├── main.py
│   ├── database.py
│   ├── models.py
│   ├── schemas.py
│   ├── auth.py
│   └── routers/
│       ├── auth.py
│       └── chat.py
├── .env.example
├── requirements.txt
└── README.md
```

---

## 📝 Submission Checklist

Before submitting, make sure:

- [ ] The project runs from a fresh clone with no errors
- [ ] A `.env.example` file is included (with no real secrets)
- [ ] All Group A tasks are complete
- [ ] One Group B task is complete
- [ ] Your own `README.md` explains how to run the project locally
- [ ] Code is clean, readable, and consistently formatted

---

## ⚖️ Evaluation Criteria

| Area | What We Look For |
|---|---|
| **Correctness** | Does the application work as described? |
| **Code Quality** | Is the code readable, organized, and consistent? |
| **Security** | Are passwords hashed? Is JWT handled correctly? Are routes protected? |
| **Database Design** | Are models well-structured with proper relationships? |
| **Error Handling** | Are edge cases handled gracefully (bad token, missing room, etc.)? |
| **Documentation** | Is the README clear enough for someone to run the project? |

---

## ❓ Questions

If anything is unclear, please reach out before starting rather than making assumptions.  
Good luck! 🚀
