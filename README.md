# simple_auth

## Overview
This folder demonstrates two basic authentication mechanisms using **Node.js + Express**:
- **Basic Auth**: authentication via HTTP Authorization header.
- **Cookie Auth**: authentication using cookies stored in MongoDB with TTL.

Structure:
```
simple_auth/
├─ .gitignore
├─ basic_auth.js
├─ cookie_auth.js
├─ package.json
└─ package-lock.json
```

## Installation
Requirements:
- Node.js >= 18
- MongoDB running locally

Install dependencies:
```bash
npm install
```

## Run the servers

### Basic Auth
```bash
node basic_auth.js
```
Server runs at: http://localhost:3000

### Cookie Auth
```bash
node cookie_auth.js
```
Server runs at: http://localhost:3001  
(Make sure MongoDB is running)

## Testing with Postman

### 4.1 Basic Auth
Endpoints:
- `GET /` and `GET /public` → open access
<img width="1070" height="651" alt="image" src="https://github.com/user-attachments/assets/858fe96c-58af-4ee3-b48b-133c552c72b8" />

- `GET /secure` → requires Basic Auth

How to test:
- In **Authorization** tab → Type: **Basic Auth**
- Username: `admin`
- Password: `12345`

Expected results:
- Correct credentials → HTTP 200 with success message
- Wrong or missing credentials → HTTP 401/403

### 4.2 Cookie Auth
Endpoints:
- `POST /login` – Body JSON:
```json
{ "username": "admin", "password": "12345" }
```
Creates an `auth_cookie_token` (httpOnly, TTL ~5 minutes) and stores a record in `cookieApp.cookies`.

- `GET /profile` – Requires valid cookie; returns user info if still valid.
- `POST /logout` – Clears cookie on client and deletes record from database.

Check MongoDB (optional):
```bash
mongosh
use cookieApp
db.cookies.find().pretty()
```

## Notes
- Cookie TTL: 5 minutes (`expires: 60*5` in schema).
- MongoDB connection string is hardcoded as: `mongodb://127.0.0.1:27017/cookieApp`.
- `.gitignore` is configured to ignore `node_modules`, `.env`, and temp files.

## 6️⃣ License
For educational and practice purposes as guided by the lecturer.
