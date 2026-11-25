# CivicPulse — Civic Issue & Pothole Reporter

A full-stack web app for reporting, tracking, and resolving civic issues (potholes, water leaks, streetlights, garbage).

## Stack
- **Backend**: Node.js + Express + better-sqlite3
- **Auth**: JWT (bcrypt passwords, role-based access)
- **Security**: helmet, express-rate-limit, CORS, file-type validation
- **Frontend**: Vanilla HTML/CSS/JS (no build step)
- **Database**: SQLite (file: `data/civicpulse.db`)

## Quick Start

### 1. Install dependencies
```bash
cd "c:\Users\hp\OneDrive\Desktop\IBM FOLDER 3\civic-reporter"
npm install
```

### 2. Create demo accounts
```bash
node server/seed.js
```

### 3. Start the server
```bash
npm start
# or for auto-reload during development:
npm run dev
```

### 4. Open the app
Visit **http://localhost:3000** in your browser.

## Demo Credentials

| Role      | Email                | Password |
|-----------|----------------------|----------|
| Citizen   | citizen@demo.com     | demo123  |
| Authority | authority@demo.com   | demo123  |
| Admin     | admin@demo.com       | demo123  |

## Project Structure

```
civic-reporter/
├── public/
│   ├── index.html          ← Login / Register page
│   └── app.html            ← Main app (protected)
├── server/
│   ├── index.js            ← Express server entry point
│   ├── auth.js             ← JWT helpers + middleware
│   ├── db.js               ← SQLite schema + connection
│   ├── seed.js             ← Demo account seed script
│   └── routes/
│       ├── authRoutes.js         ← /api/auth/*
│       ├── issueRoutes.js        ← /api/issues/*
│       ├── notificationRoutes.js ← /api/notifications/*
│       └── dashboardRoutes.js    ← /api/dashboard/*
├── data/
│   ├── civicpulse.db       ← SQLite database (auto-created)
│   └── uploads/            ← Uploaded photos (auto-created)
└── package.json
```

## API Endpoints

### Auth
| Method | Path               | Access  | Description             |
|--------|--------------------|---------|-------------------------|
| POST   | /api/auth/register | Public  | Register new account    |
| POST   | /api/auth/login    | Public  | Login + get JWT         |
| GET    | /api/auth/me       | Any     | Get current user        |
| PUT    | /api/auth/profile  | Any     | Update profile          |

### Issues
| Method | Path                       | Access    | Description               |
|--------|----------------------------|-----------|---------------------------|
| POST   | /api/issues                | Any       | Create issue (+ photo)    |
| GET    | /api/issues                | Any       | List issues (filterable)  |
| GET    | /api/issues/public         | Public    | Public map data           |
| GET    | /api/issues/:id            | Any       | Get issue + timeline      |
| PATCH  | /api/issues/:id/status     | Authority | Update status             |
| PATCH  | /api/issues/:id/assign     | Authority | Assign to department      |
| POST   | /api/issues/:id/upvote     | Any       | Toggle upvote             |
| POST   | /api/issues/:id/after-photo| Authority | Upload after-fix photo    |

### Dashboard (Authority only)
| Method | Path                     | Access    | Description              |
|--------|--------------------------|-----------|--------------------------|
| GET    | /api/dashboard/stats     | Authority | Summary stats            |
| GET    | /api/dashboard/hotspots  | Authority | Repeat problem locations |

### Notifications
| Method | Path                          | Access | Description        |
|--------|-------------------------------|--------|--------------------|
| GET    | /api/notifications            | Any    | My notifications   |
| PATCH  | /api/notifications/read-all   | Any    | Mark all read      |
| PATCH  | /api/notifications/:id/read   | Any    | Mark one read      |

## Security Features
- **bcrypt** password hashing (cost factor 12)
- **JWT** tokens (7-day expiry, signed with secret)
- **Rate limiting**: 200 req/15min general, 10 req/15min on auth routes
- **Helmet** HTTP security headers
- **Role-based access**: citizen / authority / admin
- **Anonymous reporting**: reporter ID not stored
- **File validation**: image type + 10 MB size limit
- **CORS**: restricted to localhost origins

## Environment Variables (optional)
```
PORT=3000
JWT_SECRET=your-very-long-random-secret
```
