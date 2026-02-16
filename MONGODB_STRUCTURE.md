# MongoDB Database Structure - Visual Guide

## Database: `studentgroth`

```
studentgroth/
│
├── users (Collection)
│   └── {
│       _id: ObjectId,
│       name: String,
│       email: String,
│       password: String (hashed),
│       interests: [String],
│       createdAt: Date
│     }
│
├── careergguides (Collection) ⭐ NEW
│   └── {
│       _id: ObjectId,
│       name: String (Career path name),
│       description: String,
│       photo: String (URL),
│       roadmap: String (Basic overview),
│       
│       detailedRoadmap: {  ⭐ NEW FEATURE
│         steps: [
│           { title: String, description: String },
│           { title: String, description: String },
│           ... (5 steps total)
│         ],
│         skills: [String],              // 8-10 core skills
│         tools: [String],               // 5-6 technologies
│         projects: [String],            // 5-6 project ideas
│         youtubeChannels: [
│           { name: String, url: String },
│           ... (3-4 channels)
│         ],
│         books: [
│           { title: String, author: String, link: String },
│           ... (1-2 books)
│         ]
│       },
│       
│       youtubeChannels: [
│         { name: String, url: String }
│       ],
│       notes: [
│         { title: String, link: String }
│       ],
│       books: [
│         { title: String, author: String, link: String }
│       ]
│     }
│
├── hackathons (Collection)
│   └── {
│       _id: ObjectId,
│       title: String,
│       description: String,
│       date: Date,
│       location: String,
│       prize: String,
│       registrationLink: String,
│       tags: [String],
│       image: String,
│       createdAt: Date,
│       updatedAt: Date
│     }
│
├── internships (Collection)
│   └── {
│       _id: ObjectId,
│       title: String,
│       company: String,
│       location: String,
│       skills: [String],        // For filtering
│       stipend: String,
│       duration: String,
│       deadline: Date,
│       applyLink: String,
│       source: String,
│       description: String,
│       createdAt: Date
│     }
│
├── resumes (Collection)
│   └── {
│       _id: ObjectId,
│       userId: ObjectId (Reference to users),
│       fullName: String,
│       email: String,
│       phone: String,
│       skills: [String],
│       experience: [
│         {
│           jobTitle: String,
│           company: String,
│           duration: String,
│           description: String
│         }
│       ],
│       education: [
│         {
│           degree: String,
│           institution: String,
│           year: String,
│           cgpa: String
│         }
│       ],
│       projects: [
│         {
│           title: String,
│           description: String,
│           link: String
│         }
│       ],
│       certifications: [
│         {
│           name: String,
│           issuer: String,
│           date: String,
│           link: String
│         }
│       ],
│       createdAt: Date,
│       updatedAt: Date
│     }
│
├── subjects (Collection) [Legacy - Optional]
│   └── {
│       _id: ObjectId,
│       name: String,
│       description: String,
│       photo: String,
│       roadmap: String,
│       youtubeChannels: [{ name, url }],
│       notes: [{ title, link }],
│       books: [{ title, author, link }]
│     }
│
└── sessions (Collection) [Optional - if using sessions]
    └── {
        sessionID: String,
        userId: ObjectId,
        createdAt: Date,
        expiresAt: Date
      }
```

---

## Collection Details

### 1️⃣ Users Collection
**Purpose**: Store user accounts and preferences
**Indexes**:
- `email` (unique) - For login
- `_id` (primary) - For user identification

**Data Types**:
- `interests`: Array of career path names user is interested in
- `password`: Bcrypt hashed (never store plain text)

---

### 2️⃣ CareerGuides Collection ⭐ NEW
**Purpose**: Complete career path information with detailed learning roadmaps
**Size**: 8 documents (one per career path)
**Indexes**:
- `_id` (primary)
- `name` (for quick lookup)

**Career Paths**:
1. Software Developer / Software Engineer
2. Web Developer (Frontend/Backend/Full Stack)
3. Machine Learning Engineer
4. AI Engineer
5. UI/UX Designer
6. Game Developer
7. Business Analyst (Tech-based)
8. Database Administrator (DBA)

**Key Features**:
- `detailedRoadmap.steps`: 5-step progression from beginner to advanced
- `detailedRoadmap.skills`: Essential skills to develop
- `detailedRoadmap.tools`: Technologies and tools to master
- `detailedRoadmap.projects`: Real-world project ideas
- `detailedRoadmap.youtubeChannels`: YouTube channels for learning
- `detailedRoadmap.books`: Recommended textbooks

---

### 3️⃣ Hackathons Collection
**Purpose**: Event listings and registration
**Auto-Updated**: Daily at 00:00 (midnight) via node-cron
**Indexes**:
- `date` (descending) - Sort upcoming events
- `tags` - Filter by category

**Data Fields**:
- `date`: Hackathon start date
- `location`: City/Online
- `registrationLink`: URL to register
- `tags`: Categories (Web, AI, IoT, etc.)

---

### 4️⃣ Internships Collection
**Purpose**: Internship opportunities with skill-based matching
**Filtering**: By location, source, and skills (comma-separated)
**Indexes**:
- `skills` - For filtering by skill
- `deadline` - Sort by deadline

**Skill Filtering Example**:
```javascript
// Filter internships that require Python OR JavaScript
GET /api/internships?skills=Python,JavaScript
```

---

### 5️⃣ Resumes Collection
**Purpose**: User resume data (saved in MongoDB)
**Relationships**: Links to `users` collection via `userId`
**Indexes**:
- `userId` - Find resume by user
- `createdAt` - Sort by date

**Sections**:
- Personal info
- Skills (array)
- Experience (array of jobs)
- Education (array of degrees)
- Projects (array with links)
- Certifications (array)

---

### 6️⃣ Subjects Collection [Legacy]
**Purpose**: Alternative career path storage (optional)
**Note**: CareerGuides now recommended instead
**Backward Compatible**: App checks both collections

---

## Connection String

### Local Development
```
mongodb://localhost:27017/studentgroth
```

### MongoDB Atlas (Cloud)
```
mongodb+srv://username:password@cluster-name.mongodb.net/studentgroth?retryWrites=true&w=majority
```

### Environment Variable (.env)
```
MONGODB_URI=mongodb://localhost:27017/studentgroth
```

---

## Sample Document - CareerGuide

### Software Developer / Software Engineer
```json
{
  "_id": ObjectId("507f1f77bcf86cd799439012"),
  "name": "Software Developer / Software Engineer",
  "description": "Core Skills: Programming (C++ / Java / Python), DSA, OOP, Git",
  "photo": "https://images.unsplash.com/photo-1517694712202-14dd9538aa97?w=400&h=300&fit=crop",
  "roadmap": "1. Learn one main language (Java or C++)\n2. Master DSA + OOP\n3. Build small console + backend projects\n4. Practice coding platforms",
  
  "detailedRoadmap": {
    "steps": [
      {
        "title": "Choose & Learn Main Language",
        "description": "Select C++ or Java. Learn syntax, loops, functions, and basic OOP concepts (class, object, inheritance)."
      },
      {
        "title": "Master Data Structures & Algorithms",
        "description": "Arrays, Strings, Linked List, Stack, Queue, Recursion, Trees, Graphs, Sorting, Searching."
      },
      {
        "title": "Core CS Fundamentals",
        "description": "DBMS (keys, normalization), OS (process, memory), Computer Networks (basics)."
      },
      {
        "title": "Practice & Build Projects",
        "description": "Solve DSA problems daily on LeetCode. Build 2-3 projects like Student Management System and Online Quiz System."
      },
      {
        "title": "Interview Preparation",
        "description": "Mock interviews, System design basics, Behavioral questions."
      }
    ],
    
    "skills": [
      "Programming (C++/Java/Python)",
      "Data Structures & Algorithms",
      "Object-Oriented Programming (OOP)",
      "Git & Version Control",
      "Basic OS & DBMS",
      "Problem Solving"
    ],
    
    "tools": [
      "Git/GitHub",
      "VS Code",
      "Linux/Terminal",
      "LeetCode",
      "GeeksforGeeks"
    ],
    
    "projects": [
      "Student Management System (CRUD)",
      "Online Quiz System (Backend focused)",
      "Bank Management System",
      "Library Management System"
    ],
    
    "youtubeChannels": [
      {
        "name": "Apna College",
        "url": "https://www.youtube.com/c/ApnaCollege"
      },
      {
        "name": "Love Babbar",
        "url": "https://www.youtube.com/c/LoveBabbar1"
      },
      {
        "name": "Kunal Kushwaha",
        "url": "https://www.youtube.com/c/KunalKushwaha"
      }
    ],
    
    "books": [
      {
        "title": "Cracking the Coding Interview",
        "author": "Gayle Laakmann McDowell",
        "link": "https://www.crackingthecodinginterview.com/"
      },
      {
        "title": "Introduction to Algorithms",
        "author": "Cormen, Leiserson, Rivest, Stein",
        "link": "https://mitpress.mit.edu/9780262033848/"
      }
    ]
  },
  
  "youtubeChannels": [
    { "name": "Apna College", "url": "https://www.youtube.com/c/ApnaCollege" },
    { "name": "Love Babbar", "url": "https://www.youtube.com/c/LoveBabbar1" }
  ],
  
  "notes": [
    { "title": "GeeksforGeeks DSA", "link": "https://www.geeksforgeeks.org/data-structures/" },
    { "title": "LeetCode Problems", "link": "https://www.leetcode.com/" }
  ],
  
  "books": [
    { "title": "Cracking the Coding Interview", "author": "Gayle Laakmann McDowell", "link": "https://www.crackingthecodinginterview.com/" }
  ]
}
```

---

## Queries & Indexing

### Create Indexes
```javascript
// Users
db.users.createIndex({ "email": 1 }, { unique: true })

// CareerGuides
db.careergguides.createIndex({ "name": 1 })

// Hackathons
db.hackathons.createIndex({ "date": -1 })
db.hackathons.createIndex({ "tags": 1 })

// Internships
db.internships.createIndex({ "skills": 1 })
db.internships.createIndex({ "deadline": 1 })

// Resumes
db.resumes.createIndex({ "userId": 1 })
```

### Sample Queries

**Find all careerguides:**
```javascript
db.careergguides.find()
```

**Find careerguide by name:**
```javascript
db.careergguides.findOne({ name: "Software Developer / Software Engineer" })
```

**Find internships by skill:**
```javascript
db.internships.find({ skills: "Python" })
```

**Find user's resume:**
```javascript
db.resumes.findOne({ userId: ObjectId("507f1f77bcf86cd799439011") })
```

**Update user interests:**
```javascript
db.users.updateOne(
  { _id: ObjectId("507f1f77bcf86cd799439011") },
  { $addToSet: { interests: "Software Developer" } }
)
```

---

## Data Flow

### How Data Loads

```
1. Server Starts (server.js)
   ↓
2. MongoDB Connected (config/db.js)
   ↓
3. loadDetailedRoadmaps() Called
   ↓
4. Clear existing CareerGuides
   ↓
5. Insert 8 new CareerGuides
   ↓
6. Console logs: ✅ Loaded 8 detailed roadmaps
   ↓
7. Frontend requests /api/subjects/all
   ↓
8. Backend returns CareerGuides (or falls back to Subjects)
   ↓
9. User clicks career path
   ↓
10. GET /api/subjects/:id returns full detailed roadmap
   ↓
11. Frontend renders step cards, skills, tools, projects, links
   ↓
12. User clicks YouTube link → Opens in new tab
   ↓
13. User clicks book link → Opens book page
```

---

## Performance Considerations

### Indexes for Fast Queries
- Emails indexed (unique) for login
- Career guide names indexed for lookups
- Skills indexed for filtering
- Dates indexed for sorting

### Document Size
- CareerGuides: ~15-20 KB each
- Hackathons: ~2-3 KB each
- Internships: ~1-2 KB each
- Resumes: ~5-10 KB each

### Connection Pooling
- MongoDB maintains connection pool
- Default: 10 connections
- Adjust if needed: `maxPoolSize: 50`

---

## Backup & Recovery

### Export Data
```bash
mongoexport --db studentgroth --collection careergguides --out careergguides.json
mongoexport --db studentgroth --collection internships --out internships.json
mongoexport --db studentgroth --collection hackathons --out hackathons.json
```

### Import Data
```bash
mongoimport --db studentgroth --collection careergguides --file careergguides.json
mongoimport --db studentgroth --collection internships --file internships.json
mongoimport --db studentgroth --collection hackathons --file hackathons.json
```

---

## Summary

✅ **8 CareerGuides** with complete detailed roadmaps
✅ **8 Hackathons** with event details
✅ **8+ Internships** with skill matching
✅ **User Accounts** with interests tracking
✅ **Resume Storage** with all sections
✅ **Automatic Data Loading** on server start
✅ **Clickable Resources** (YouTube, Books, Links)
✅ **MongoDB Indexes** for fast queries

All collections are optimized for the Student Career & Growth Assistant! 🚀
