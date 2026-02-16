# Migrating Local MongoDB to MongoDB Atlas

This document explains how to move your local DB (mongodb://127.0.0.1:27017/student_career_assistant) to MongoDB Atlas and update the project to use Atlas.

## 1) Prepare Atlas
1. Sign in to https://cloud.mongodb.com and create a free (M0) cluster.
2. In *Database Access*, create a DB user with a password (note the username and password).
3. In *Network Access*, add your IP (or 0.0.0.0/0 for development).
4. Get the connection string from **Connect** → **Connect your application**. It looks like:

   mongodb+srv://<USER>:<PASSWORD>@cluster0.zp4eqt2.mongodb.net/<DBNAME>?retryWrites=true&w=majority

> If your password contains special characters, URL-encode it. Example (node/JS):
> encodeURIComponent('4aNeAZVte*u27@s')

## 2) Configure project to use Atlas (already applied)
- Copy `.env.example` to `.env` in the `backend/` folder and set your `MONGO_URI`.
  - Example `.env`:
    MONGO_URI=mongodb+srv://ishvapatel2006_db_user:ENCODED_PASSWORD@cluster0.zp4eqt2.mongodb.net/student_career_assistant?retryWrites=true&w=majority
- The server now loads environment variables via `dotenv` (server.js).
- `config/db.js` uses `process.env.MONGO_URI` with a local fallback.

## 3) Migrating data (two common ways)

### Option A: mongodump / mongorestore (recommended for full DB)
1. Install MongoDB Database Tools: https://www.mongodb.com/docs/database-tools/installation/
2. Dump local DB:
   ```bash
   mongodump --uri="mongodb://127.0.0.1:27017/student_career_assistant" --out=./dump
   ```
3. Restore to Atlas (specify the DB in the URI):
   ```bash
   mongorestore --uri="mongodb+srv://<USER>:<ENCODED_PASSWORD>@cluster0.zp4eqt2.mongodb.net/student_career_assistant" ./dump/student_career_assistant
   ```

### Option B: mongoexport / mongoimport (JSON; useful for selective collections)
1. Export a collection:
   ```bash
   mongoexport --uri="mongodb://127.0.0.1:27017/student_career_assistant" -c subjects -o subjects.json
   ```
2. Import into Atlas:
   ```bash
   mongoimport --uri="mongodb+srv://<USER>:<ENCODED_PASSWORD>@cluster0.zp4eqt2.mongodb.net/student_career_assistant" -c subjects --file subjects.json
   ```

## 4) Test locally
1. Create `backend/.env` (not committed) and set `MONGO_URI`.
2. Start backend: `cd backend && npm start`.
3. Verify logs show `MongoDB Connected`. Test the API endpoints (e.g., `GET /api/subjects/all`).

## 5) Deploy and secure
- When deploying, set `MONGO_URI` as an environment variable in your host (Heroku/Render/Azure etc.) rather than committing `.env`.
- Rotate credentials if they were shared accidentally.

---
If you want, I can generate the exact `mongorestore` command with your encoded password (I won't store it) — tell me if you want me to create the command for you now.