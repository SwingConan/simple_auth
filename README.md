<img width="1093" height="627" alt="image" src="https://github.com/user-attachments/assets/3f7a5ab4-3ad3-4929-b8b3-42b4923fac14" /># simple_auth

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

<img width="1095" height="666" alt="image" src="https://github.com/user-attachments/assets/576cb05f-3ace-4943-813a-3a34a7bbd46e" />

- `GET /secure` → requires Basic Auth

How to test:
- In **Authorization** tab → Type: **Basic Auth**
- Username: `admin`
- Password: `12345`

Expected results:
- Correct credentials → HTTP 200 with success message
  <img width="1083" height="642" alt="image" src="https://github.com/user-attachments/assets/0c97cf29-2730-4830-8043-c5516b33573d" />

- Wrong or missing credentials → HTTP 401/403
<img width="1100" height="618" alt="image" src="https://github.com/user-attachments/assets/a871dd7c-8e9d-4b83-b744-692ef793eab9" />

### 4.2 Cookie Auth
Endpoints:
- `POST /login` – Body JSON:
```json
{ "username": "admin", "password": "12345" }
```
<img width="1077" height="620" alt="image" src="https://github.com/user-attachments/assets/319bed03-4562-48fc-bf4f-8d098bd3c17d" />

<img width="1105" height="606" alt="image" src="https://github.com/user-attachments/assets/7e9fe01f-9f0c-42b6-95fe-88d5fc5646b4" />

Creates an `auth_cookie_token` (httpOnly, TTL ~5 minutes) and stores a record in `cookieApp.cookies`.

- `GET /profile` – Requires valid cookie; returns user info if still valid.
  <img width="1093" height="627" alt="image" src="https://github.com/user-attachments/assets/e112a088-0f51-43e4-9da2-c7dd474479e8" />

<img width="1102" height="894" alt="image" src="https://github.com/user-attachments/assets/c86062d8-0b76-4742-9816-9124792d6158" />

- `POST /logout` – Clears cookie on client and deletes record from database.
<img width="1096" height="630" alt="image" src="https://github.com/user-attachments/assets/7f0235db-83d2-4491-82ac-884a259ee70a" />

<img width="1075" height="889" alt="image" src="https://github.com/user-attachments/assets/c4f77a46-633c-48b2-9870-a4c120a20a35" />

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
