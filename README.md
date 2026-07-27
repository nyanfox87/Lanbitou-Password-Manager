# Lanbitou Password Manager

A self-hosted, end-to-end encrypted password manager. Encryption and decryption happen entirely on the client — the server only ever sees ciphertext, and supports sharing individual passwords with other users or groups under read/write/delete permissions.

## Features

- **End-to-end encryption (E2EE)** — passwords are encrypted client-side with AES-GCM before they're sent; keys are derived with PBKDF2 and never leave the browser.
- **JWT authentication** with Argon2 password hashing.
- **Granular sharing** — grant `read`, `write`, or `delete` access to a password for individual users or whole groups.
- **Group management** — create groups and add members with per-member permissions.

## Structure

```
Lanbitou-Password-Manager/
├── server/               Flask API (auth, storage, permissions, groups)
│   ├── app.py
│   ├── models.py
│   ├── auth.py
│   ├── storage.py
│   ├── get_passwords.py
│   ├── permission_storage.py
│   ├── groups.py
│   ├── reset_db.py
│   ├── requirements.txt
│   └── README.md         API reference
│
├── frontend/             Next.js web client
│   ├── src/
│   │   ├── app/          pages: landing, login, register, setup, vault
│   │   ├── components/
│   │   ├── contexts/     AuthContext
│   │   └── lib/          api.ts, crypto.ts (PBKDF2, AES-GCM), jwt.ts
│   └── package.json
│
└── client_test/          Python scripts for exercising the API
```

## Getting Started

### 1. Backend (Flask)

```bash
cd server
pip install -r requirements.txt
python reset_db.py      # initialize the database
flask run                # starts on http://localhost:5000
```

The backend uses JWT-based authentication with Argon2 password hashing. All sensitive data is stored using end-to-end encryption — the server only ever stores encrypted password entries and never sees plaintext or encryption keys. See [`server/README.md`](server/README.md) for the full API reference.

### 2. Frontend (Next.js)

```bash
cd frontend
npm install
npm run dev              # starts on http://localhost:3000
```

## Tech Stack

| Layer    | Technology |
|----------|------------|
| Frontend | Next.js, React, TypeScript, Tailwind CSS |
| Backend  | Flask, Flask-SQLAlchemy, Flask-JWT-Extended |
| Auth     | JWT + Argon2 |
| Crypto   | PBKDF2 + AES-GCM (client-side) |
| Database | SQLite |
