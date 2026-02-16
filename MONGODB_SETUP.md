# MongoDB Database Setup Guide

## Overview
The Student Career & Growth Assistant uses MongoDB with 8 collections to store all data.

---

## Collections & Schemas

### 1. **Users** (users)
Stores user authentication and profile data.

```javascript
{
  _id: ObjectId,
  name: String,
  email: String (unique),
  password: String (hashed with bcryptjs),
  interests: [String], // Career paths user is interested in
  createdAt: Date,
  resume: ObjectId (reference to Resume)
}
```

**Example:**
```json
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "name": "John Doe",
  "email": "john@example.com",
  "password": "$2a$10$...",
  "interests": ["Software Developer", "Web Developer"],
  "createdAt": "2024-01-20T10:30:00Z"
}
```

---

### 2. **CareerGuide** (careergguides)
Complete career path information with detailed roadmaps.

```javascript
{
  _id: ObjectId,
  name: String,
  description: String,
  photo: String (URL),
  roadmap: String,
  detailedRoadmap: {
    steps: [
      {
        title: String,
        description: String
      }
    ],
    skills: [String],
    tools: [String],
    projects: [String],
    youtubeChannels: [
      {
        name: String,
        url: String
      }
    ],
    books: [
      {
        title: String,
        author: String,
        link: String
      }
    ]
  },
  youtubeChannels: [
    {
      name: String,
      url: String
    }
  ],
  notes: [
    {
      title: String,
      link: String
    }
  ],
  books: [
    {
      title: String,
      author: String,
      link: String
    }
  ]
}
```

**Available Career Paths:**
1. Software Developer / Software Engineer
2. Web Developer (Frontend / Backend / Full Stack)
3. Machine Learning Engineer
4. AI Engineer
5. UI/UX Designer
6. Game Developer
7. Business Analyst (Tech-based)
8. Database Administrator (DBA)

**Example:**
```json
{
  "_id": ObjectId("507f1f77bcf86cd799439012"),
  "name": "Software Developer / Software Engineer",
  "description": "Core Skills: Programming (C++ / Java / Python), DSA, OOP, Git",
  "photo": "https://images.unsplash.com/...",
  "roadmap": "1. Learn one main language...",
  "detailedRoadmap": {
    "steps": [
      {
        "title": "Choose & Learn Main Language",
        "description": "Select C++ or Java..."
      }
    ],
    "skills": ["Programming (C++/Java/Python)", "Data Structures & Algorithms", ...],
    "tools": ["Git/GitHub", "VS Code", "Linux/Terminal"],
    "projects": ["Student Management System", "Online Quiz System"],
    "youtubeChannels": [
      {"name": "Apna College", "url": "https://www.youtube.com/c/ApnaCollege"},
      {"name": "Love Babbar", "url": "https://www.youtube.com/c/LoveBabbar1"}
    ],
    "books": [
      {
        "title": "Cracking the Coding Interview",
        "author": "Gayle Laakmann McDowell",
        "link": "https://..."
      }
    ]
  }
}
```

---

### 3. **Hackathons** (hackathons)
Hackathon event listings with details.

```javascript
{
  _id: ObjectId,
  title: String,
  description: String,
  date: Date,
  location: String,
  prize: String,
  registrationLink: String (URL),
  tags: [String],
  image: String (URL),
  createdAt: Date,
  updatedAt: Date
}
```

**Example:**
```json
{
  "_id": ObjectId("507f1f77bcf86cd799439013"),
  "title": "HackIndia 2024",
  "description": "24-hour national hackathon",
  "date": "2024-02-15T09:00:00Z",
  "location": "Bangalore, India",
  "prize": "₹5,00,000",
  "registrationLink": "https://hackindia.com/register",
  "tags": ["Web", "AI", "IoT"],
  "image": "https://...",
  "createdAt": "2024-01-20T10:30:00Z",
  "updatedAt": "2024-01-20T10:30:00Z"
}
```

---

### 4. **Internships** (internships)
Internship opportunities with skill-based filtering.

```javascript
{
  _id: ObjectId,
  title: String,
  company: String,
  location: String,
  skills: [String], // Required skills
  stipend: String,
  duration: String,
  deadline: Date,
  applyLink: String (URL),
  source: String, // "LinkedIn", "Internshala", "Government"
  description: String,
  createdAt: Date
}
```

**Example:**
```json
{
  "_id": ObjectId("507f1f77bcf86cd799439014"),
  "title": "Frontend Developer Intern",
  "company": "TechCorp",
  "location": "Bangalore",
  "skills": ["React", "JavaScript", "CSS"],
  "stipend": "₹15,000/month",
  "duration": "3 months",
  "deadline": "2024-02-28T23:59:59Z",
  "applyLink": "https://linkedin.com/jobs/search/?keywords=Frontend%20Developer%20Internship",
  "source": "LinkedIn",
  "description": "Develop responsive web applications",
  "createdAt": "2024-01-20T10:30:00Z"
}
```

---

### 5. **Resumes** (resumes)
User resume data with sections.

```javascript
{
  _id: ObjectId,
  userId: ObjectId (reference to User),
  fullName: String,
  email: String,
  phone: String,
  skills: [String],
  experience: [
    {
      jobTitle: String,
      company: String,
      duration: String,
      description: String
    }
  ],
  education: [
    {
      degree: String,
      institution: String,
      year: String,
      cgpa: String
    }
  ],
  projects: [
    {
      title: String,
      description: String,
      link: String
    }
  ],
  certifications: [
    {
      name: String,
      issuer: String,
      date: String,
      link: String
    }
  ],
  createdAt: Date,
  updatedAt: Date
}
```

**Example:**
```json
{
  "_id": ObjectId("507f1f77bcf86cd799439015"),
  "userId": ObjectId("507f1f77bcf86cd799439011"),
  "fullName": "John Doe",
  "email": "john@example.com",
  "phone": "+919876543210",
  "skills": ["Python", "JavaScript", "React", "MongoDB"],
  "experience": [
    {
      "jobTitle": "Frontend Developer Intern",
      "company": "TechCorp",
      "duration": "Jan 2023 - Jun 2023",
      "description": "Developed responsive web interfaces"
    }
  ],
  "education": [
    {
      "degree": "B.Tech",
      "institution": "MIT Bangalore",
      "year": "2023",
      "cgpa": "8.5"
    }
  ],
  "projects": [...],
  "certifications": [...],
  "createdAt": "2024-01-20T10:30:00Z",
  "updatedAt": "2024-01-20T15:45:00Z"
}
```

---

### 6. **Subjects** (subjects) - Legacy
Optional legacy collection for basic career paths.

```javascript
{
  _id: ObjectId,
  name: String,
  description: String,
  photo: String (URL),
  roadmap: String,
  youtubeChannels: [
    {
      name: String,
      url: String
    }
  ],
  notes: [
    {
      title: String,
      link: String
    }
  ],
  books: [
    {
      title: String,
      author: String,
      link: String
    }
  ]
}
```

---

## MongoDB Connection String

Use this format in your MongoDB:

```
mongodb://localhost:27017/studentgroth
```

**Or with MongoDB Atlas (Cloud):**

```
mongodb+srv://<username>:<password>@<cluster>.mongodb.net/studentgroth
```

---

## Setting Up MongoDB Locally

### 1. **Install MongoDB Community Edition**

**On Windows:**
- Download from: https://www.mongodb.com/try/download/community
- Run the installer
- MongoDB Community Edition will install as a service on port 27017

**On macOS:**
```bash
brew tap mongodb/brew
brew install mongodb-community
brew services start mongodb-community
```

**On Linux (Ubuntu):**
```bash
sudo apt-get update
sudo apt-get install -y mongodb
sudo systemctl start mongodb
```

---

## 2. **Verify MongoDB Installation**

Open a terminal and connect to MongoDB:

```bash
mongosh
```

You should see:
```
MongoClient> 
```

---

## 3. **Create Database & Collections**

```bash
# Connect to MongoDB
mongosh

# Use or create database
use studentgroth

# Create collections
db.createCollection("users")
db.createCollection("careergguides")
db.createCollection("hackathons")
db.createCollection("internships")
db.createCollection("resumes")
db.createCollection("subjects")

# Create indexes for better performance
db.users.createIndex({ "email": 1 }, { unique: true })
db.resumes.createIndex({ "userId": 1 })
db.hackathons.createIndex({ "date": -1 })
db.internships.createIndex({ "skills": 1 })

# Verify collections
show collections
```

---

## 4. **Backend Configuration**

In `backend/config/db.js`:

```javascript
const mongoose = require("mongoose");

const connectDB = async () => {
  try {
    const conn = await mongoose.connect("mongodb://localhost:27017/studentgroth", {
      useNewUrlParser: true,
      useUnifiedTopology: true
    });
    console.log("✅ MongoDB connected:", conn.connection.host);
  } catch (error) {
    console.error("❌ MongoDB connection error:", error);
    process.exit(1);
  }
};

module.exports = connectDB;
```

---

## 5. **Running the Backend**

```bash
# Navigate to backend directory
cd backend

# Install dependencies
npm install

# Start the server
npm start
# or
node server.js
```

You should see:
```
Server running on port 5000
✅ MongoDB connected: localhost
📡 Fetching initial data...
✅ Loaded 8 detailed roadmaps into database
```

---

## API Endpoints

### Career Guide Endpoints
```
GET  /api/subjects/all              - Get all career paths
GET  /api/subjects/:id              - Get career path details
POST /api/subjects/interested/:id    - Add to user interests
```

### Authentication Endpoints
```
POST /api/auth/register             - User registration
POST /api/auth/login                - User login
```

### Hackathon Endpoints
```
GET  /api/hackathons                - Get all hackathons
GET  /api/update-hackathons         - Manual update trigger
```

### Internship Endpoints
```
GET  /api/internships               - Get all internships
GET  /api/internships?skills=X,Y    - Filter by skills (comma-separated)
```

### Resume Endpoints
```
POST /api/resume/save               - Save resume
GET  /api/resume/:userId            - Get user's resume
```

---

## Sample Data

The app automatically loads sample data on startup:
- **8 Career Guides** with complete roadmaps
- **8 Hackathons** with details
- **8 Internships** with skill-based matching

---

## Important Notes

1. **Automatic Data Loading**: On server startup, the app loads:
   - Detailed roadmaps into CareerGuide collection
   - Sample internships
   - Latest hackathon data

2. **YouTube Links**: All YouTube channels are clickable and open in new tabs

3. **Resource Links**: Books and notes have external links for reference

4. **Skill-Based Filtering**: Internships can be filtered by multiple skills (comma-separated)

5. **Unique Constraints**: User emails must be unique in the system

---

## Troubleshooting

### MongoDB Not Starting
```bash
# Windows: Check if service is running
net start MongoDB

# macOS: Restart service
brew services restart mongodb-community

# Linux: Restart service
sudo systemctl restart mongodb
```

### Connection Error
- Verify MongoDB is running on port 27017
- Check connection string in `config/db.js`
- Ensure `studentgroth` database exists

### No Data Showing
- Server should auto-load data on startup
- Check console logs for `✅ Loaded X detailed roadmaps`
- If missing, manually seed database using provided scripts

---

## Backup & Export Data

### Export Collections
```bash
# Export all data
mongoexport --db studentgroth --collection careergguides --out careergguides.json
mongoexport --db studentgroth --collection internships --out internships.json
```

### Import Collections
```bash
# Import data
mongoimport --db studentgroth --collection careergguides --file careergguides.json
mongoimport --db studentgroth --collection internships --file internships.json
```

---

## That's It! 🎉

Your MongoDB is ready. Run the backend server and start using the app!
