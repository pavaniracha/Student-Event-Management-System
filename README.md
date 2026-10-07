# Event Management (Node + Express + MongoDB)

This is a simple Event Management project with an Express backend and a static frontend. The backend was migrated from MySQL to MongoDB (using Mongoose). The API provides user registration/login and event listing/creation.

**Project structure**
- `backend/` — Express server and database code (`server.js`, `db.js`)
- `frontend/` — static HTML, CSS, JS for the UI
- `package.json` — project scripts and dependencies

**Server**
- Runs on port `5000` by default
- Uses MongoDB via Mongoose

## Prerequisites
- Node.js (v16+ recommended)
- npm (bundled with Node.js)
- MongoDB Community Server installed on your machine

## Install MongoDB (Windows)
- Download and install from: https://www.mongodb.com/try/download/community
- During installation, you can install as a Windows Service (recommended). If not using the service, you will run `mongod` to start the server.

To start MongoDB:
- If installed as a service (default option), run in PowerShell as Administrator:

```powershell
net start MongoDB
```

- If running directly, open a terminal and run:

```powershell
mongod --dbpath "C:\data\db"
```

Adjust `--dbpath` if you use a different data directory. Keep this terminal open while your server runs.

## Configure (optional)
The code uses a default MongoDB URI `mongodb://localhost:27017/event_management`. To use a different URI (for example a remote server or Atlas), set the `MONGO_URI` environment variable before starting the server.

PowerShell example:

```powershell
$env:MONGO_URI = "mongodb+srv://<user>:<pass>@cluster0.mongodb.net/event_management?retryWrites=true&w=majority"
npm start
```

## Install dependencies and run
From project root (`c:\Users\Pavani\Desktop\Event Management\Event Management`):

```powershell
cd "c:\Users\Pavani\Desktop\Event Management\Event Management"
npm install
npm start
```

You should see: `Server running on http://localhost:5000` and `MongoDB connected successfully` in the console (if `mongod` is running).

## API Endpoints
Base URL: `http://localhost:5000`

- POST `/register`
  - Body (JSON): `{ "name": "Alice", "email": "alice@example.com", "password": "secret" }`
  - Success: `{ "message": "User registered successfully" }`

- POST `/login`
  - Body (JSON): `{ "email": "alice@example.com", "password": "secret" }`
  - Success: `{ "message": "Login successful", "user": { ... } }`

- GET `/events`
  - Returns an array of events (JSON)

- POST `/events`
  - Body (JSON): `{ "title": "Party", "date": "2025-12-31T20:00:00.000Z", "location": "Hall A" }`
  - Success: `{ "message": "Event added successfully" }`

### Example curl (works from PowerShell with curl):

```powershell
curl -X POST http://localhost:5000/register -H "Content-Type: application/json" -d '{"name":"Test","email":"test@example.com","password":"pass"}'

curl -X POST http://localhost:5000/login -H "Content-Type: application/json" -d '{"email":"test@example.com","password":"pass"}'

curl http://localhost:5000/events

curl -X POST http://localhost:5000/events -H "Content-Type: application/json" -d '{"title":"New Event","date":"2025-12-15","location":"Room 1"}'
```

## Security notes & next steps
- Passwords are currently stored in plaintext. You should hash passwords (e.g., using `bcrypt`) before storing them.
- Add input validation (e.g., `express-validator`) and better error handling.
- Add authentication (JWT or sessions) to protect routes.
- Consider CORS restrictions if deploying frontend and backend on different domains.

## Frontend
The static frontend files are in the `frontend/` folder. They use the backend API at `http://localhost:5000`. Update frontend `fetch` calls if you host the backend elsewhere.

## Troubleshooting
- If `npm start` fails, check the console for errors. Common issues:
  - MongoDB not running — start `mongod` or the service
  - Port 5000 in use — change `PORT` in `backend/server.js`
  - Missing dependencies — run `npm install`

---
If you want, I can:
- Add password hashing using `bcrypt` and update the register/login flows
- Add environment variable support for `PORT` and `MONGO_URI`
- Add a Postman collection with example requests

