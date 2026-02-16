# MongoDB Commands & Queries Guide

## 🚀 Quick Start Commands

### Connect to MongoDB
```bash
mongosh
```

### Select Database
```javascript
use studentgroth
```

### Show All Collections
```javascript
show collections
```

---

## 🔧 Database Setup Commands

### Create Database (Auto-created on first insert)
```javascript
use studentgroth
```

### Create Collections
```javascript
db.createCollection("users")
db.createCollection("careergguides")
db.createCollection("hackathons")
db.createCollection("internships")
db.createCollection("resumes")
db.createCollection("subjects")
```

### Create Indexes for Performance
```javascript
// Users collection
db.users.createIndex({ "email": 1 }, { unique: true })
db.users.createIndex({ "createdAt": -1 })

// CareerGuides collection
db.careergguides.createIndex({ "name": 1 })

// Hackathons collection
db.hackathons.createIndex({ "date": -1 })
db.hackathons.createIndex({ "tags": 1 })

// Internships collection
db.internships.createIndex({ "skills": 1 })
db.internships.createIndex({ "deadline": 1 })
db.internships.createIndex({ "company": 1 })

// Resumes collection
db.resumes.createIndex({ "userId": 1 })
db.resumes.createIndex({ "createdAt": -1 })
```

---

## 📋 CRUD Operations

### CREATE (Insert Documents)

#### Insert Single Document
```javascript
db.users.insertOne({
  name: "John Doe",
  email: "john@example.com",
  password: "hashed_password",
  interests: ["Software Developer"],
  createdAt: new Date()
})
```

#### Insert Multiple Documents
```javascript
db.hackathons.insertMany([
  {
    title: "HackIndia 2024",
    description: "24-hour hackathon",
    date: new Date("2024-02-15"),
    location: "Bangalore",
    prize: "₹5,00,000"
  },
  {
    title: "TechFest 2024",
    description: "Tech festival",
    date: new Date("2024-03-20"),
    location: "Mumbai",
    prize: "₹10,00,000"
  }
])
```

---

### READ (Query Documents)

#### Find All Documents
```javascript
db.users.find()
db.careergguides.find()
db.internships.find()
```

#### Find by ID
```javascript
db.users.findOne({ _id: ObjectId("507f1f77bcf86cd799439011") })
```

#### Find by Name
```javascript
// Find career guide
db.careergguides.findOne({ name: "Software Developer / Software Engineer" })

// Find user
db.users.findOne({ name: "John Doe" })
```

#### Find by Email
```javascript
db.users.findOne({ email: "john@example.com" })
```

#### Find with Conditions
```javascript
// Find all hackathons after a date
db.hackathons.find({ date: { $gte: new Date("2024-01-01") } })

// Find internships in specific location
db.internships.find({ location: "Bangalore" })

// Find internships with specific skill
db.internships.find({ skills: "Python" })

// Find internships with ANY of multiple skills
db.internships.find({ skills: { $in: ["Python", "JavaScript"] } })

// Find all users with "Software Developer" in interests
db.users.find({ interests: "Software Developer" })
```

#### Find with Projection (Select Specific Fields)
```javascript
// Get only name and email
db.users.find({}, { name: 1, email: 1, _id: 1 })

// Exclude password field
db.users.find({}, { password: 0 })
```

#### Find with Sorting
```javascript
// Sort by date (newest first)
db.hackathons.find().sort({ date: -1 })

// Sort by date (oldest first)
db.hackathons.find().sort({ date: 1 })

// Sort by name (A-Z)
db.users.find().sort({ name: 1 })
```

#### Find with Limit
```javascript
// Get first 5 hackathons
db.hackathons.find().limit(5)

// Get first 10 users
db.users.find().limit(10)
```

#### Count Documents
```javascript
// Count total users
db.users.countDocuments()

// Count career guides
db.careergguides.countDocuments()

// Count with condition
db.internships.countDocuments({ location: "Bangalore" })
```

---

### UPDATE (Modify Documents)

#### Update Single Field
```javascript
db.users.updateOne(
  { email: "john@example.com" },
  { $set: { name: "John Smith" } }
)
```

#### Update Multiple Fields
```javascript
db.users.updateOne(
  { _id: ObjectId("507f1f77bcf86cd799439011") },
  {
    $set: {
      name: "John Doe",
      interests: ["Software Developer", "Web Developer"]
    }
  }
)
```

#### Add to Array
```javascript
// Add career to interests
db.users.updateOne(
  { _id: ObjectId("507f1f77bcf86cd799439011") },
  { $addToSet: { interests: "Machine Learning Engineer" } }
)
```

#### Add Multiple Items to Array
```javascript
db.users.updateOne(
  { _id: ObjectId("507f1f77bcf86cd799439011") },
  {
    $addToSet: {
      interests: {
        $each: ["UI/UX Designer", "Game Developer"]
      }
    }
  }
)
```

#### Remove from Array
```javascript
db.users.updateOne(
  { _id: ObjectId("507f1f77bcf86cd799439011") },
  { $pull: { interests: "Software Developer" } }
)
```

#### Update Multiple Documents
```javascript
db.internships.updateMany(
  { source: "LinkedIn" },
  { $set: { status: "active" } }
)
```

#### Replace Entire Document
```javascript
db.users.replaceOne(
  { _id: ObjectId("507f1f77bcf86cd799439011") },
  {
    name: "New Name",
    email: "newemail@example.com",
    interests: []
  }
)
```

---

### DELETE (Remove Documents)

#### Delete Single Document
```javascript
db.users.deleteOne({ _id: ObjectId("507f1f77bcf86cd799439011") })
```

#### Delete Multiple Documents
```javascript
// Delete all hackathons from 2023
db.hackathons.deleteMany({ date: { $lt: new Date("2024-01-01") } })

// Delete all internships from a specific source
db.internships.deleteMany({ source: "LinkedIn" })
```

#### Delete All Documents in Collection
```javascript
db.users.deleteMany({})
db.careergguides.deleteMany({})
```

#### Drop Entire Collection
```javascript
db.users.drop()
```

---

## 🔍 Useful Queries for Your App

### Career Guide Queries

#### Get All Career Guides
```javascript
db.careergguides.find({}, { name: 1, description: 1 })
```

#### Get Career Guide with Full Roadmap
```javascript
db.careergguides.findOne({ name: "Software Developer / Software Engineer" })
```

#### Get Just the Detailed Roadmap
```javascript
db.careergguides.findOne(
  { name: "Software Developer / Software Engineer" },
  { detailedRoadmap: 1 }
)
```

#### Get All YouTube Channels
```javascript
db.careergguides.findOne(
  { name: "Web Developer (Frontend / Backend / Full Stack)" },
  { "detailedRoadmap.youtubeChannels": 1 }
)
```

### User Queries

#### Get User by Email
```javascript
db.users.findOne({ email: "john@example.com" })
```

#### Find Users Interested in Software Developer
```javascript
db.users.find({ interests: "Software Developer" })
```

#### Count Users per Interest
```javascript
db.users.aggregate([
  { $unwind: "$interests" },
  { $group: { _id: "$interests", count: { $sum: 1 } } },
  { $sort: { count: -1 } }
])
```

### Internship Queries

#### Get All Internships
```javascript
db.internships.find()
```

#### Find Internships by Skill
```javascript
// Exact skill match
db.internships.find({ skills: "Python" })

// Any of multiple skills
db.internships.find({ skills: { $in: ["Python", "JavaScript"] } })

// All skills
db.internships.find({ skills: { $all: ["Python", "JavaScript"] } })
```

#### Find Internships by Location
```javascript
db.internships.find({ location: "Bangalore" })
```

#### Find Active Internships (Not Expired)
```javascript
db.internships.find({ deadline: { $gte: new Date() } })
```

#### Get Internships Sorted by Deadline
```javascript
db.internships.find().sort({ deadline: 1 })
```

#### Count Internships by Company
```javascript
db.internships.aggregate([
  { $group: { _id: "$company", count: { $sum: 1 } } },
  { $sort: { count: -1 } }
])
```

### Resume Queries

#### Get User's Resume
```javascript
db.resumes.findOne({ userId: ObjectId("507f1f77bcf86cd799439011") })
```

#### Find Resumes with Specific Skills
```javascript
db.resumes.find({ skills: "Python" })
```

#### Get Recent Resumes (Last 7 Days)
```javascript
db.resumes.find({
  updatedAt: { $gte: new Date(Date.now() - 7 * 24 * 60 * 60 * 1000) }
}).sort({ updatedAt: -1 })
```

### Hackathon Queries

#### Get Upcoming Hackathons
```javascript
db.hackathons.find({ date: { $gte: new Date() } }).sort({ date: 1 })
```

#### Get Hackathons by Tag
```javascript
db.hackathons.find({ tags: "Web" })
```

#### Count Hackathons by Location
```javascript
db.hackathons.aggregate([
  { $group: { _id: "$location", count: { $sum: 1 } } },
  { $sort: { count: -1 } }
])
```

---

## 📊 Aggregation Examples

### Count Documents by Category
```javascript
db.internships.aggregate([
  { $group: { _id: "$source", total: { $sum: 1 } } }
])
```

### Most Common Skills in Internships
```javascript
db.internships.aggregate([
  { $unwind: "$skills" },
  { $group: { _id: "$skills", count: { $sum: 1 } } },
  { $sort: { count: -1 } },
  { $limit: 10 }
])
```

### Career Paths by Popularity
```javascript
db.users.aggregate([
  { $unwind: "$interests" },
  { $group: { _id: "$interests", users: { $sum: 1 } } },
  { $sort: { users: -1 } }
])
```

### Internships with Skills Distribution
```javascript
db.internships.aggregate([
  { $group: { 
      _id: "$company",
      count: { $sum: 1 },
      skills: { $push: "$skills" }
  }}
])
```

---

## 🛠️ Backup & Restore

### Export Data
```bash
# Export single collection
mongoexport --db studentgroth --collection careergguides --out careergguides.json

# Export all collections
mongoexport --db studentgroth --collection users --out users.json
mongoexport --db studentgroth --collection internships --out internships.json
mongoexport --db studentgroth --collection hackathons --out hackathons.json
mongoexport --db studentgroth --collection resumes --out resumes.json
```

### Import Data
```bash
# Import single collection
mongoimport --db studentgroth --collection careergguides --file careergguides.json

# Import all
mongoimport --db studentgroth --collection users --file users.json
mongoimport --db studentgroth --collection internships --file internships.json
mongoimport --db studentgroth --collection hackathons --file hackathons.json
mongoimport --db studentgroth --collection resumes --file resumes.json
```

---

## 🔐 Security Tips

### Create Admin User
```javascript
use admin
db.createUser({
  user: "admin",
  pwd: "strong_password",
  roles: ["root"]
})
```

### Create Database User
```javascript
use studentgroth
db.createUser({
  user: "app_user",
  pwd: "app_password",
  roles: ["readWrite"]
})
```

### Connect with Authentication
```bash
mongosh --username app_user --password --authenticationDatabase studentgroth
```

---

## 📈 Performance Tips

### Check Index Performance
```javascript
db.careergguides.aggregate([{ $indexStats: {} }])
```

### Explain Query Plan
```javascript
db.internships.find({ skills: "Python" }).explain("executionStats")
```

### Find Slow Queries
```javascript
db.setProfilingLevel(1)  // Log slow queries
db.system.profile.find({millis:{$gt:100}})
```

---

## 🧹 Maintenance Commands

### Check Database Statistics
```javascript
db.stats()
```

### Check Collection Size
```javascript
db.careergguides.stats()
```

### Compact Database
```javascript
db.runCommand({ compact: "users" })
```

### Rebuild Indexes
```javascript
db.careergguides.reIndex()
```

### Drop Index
```javascript
db.users.dropIndex("email_1")
```

---

## 🚨 Common Issues & Solutions

### Duplicate Key Error
```javascript
// Check unique index
db.users.getIndexes()

// Remove duplicate
db.users.deleteOne({ email: "duplicate@example.com" })

// Recreate index
db.users.dropIndex("email_1")
db.users.createIndex({ email: 1 }, { unique: true })
```

### Connection Timeout
```bash
# Increase timeout
mongosh --serverSelectionTimeoutMS 5000
```

### Out of Memory
```javascript
// Limit query results
db.hackathons.find().limit(100)
```

---

## 📚 Reference

### ObjectId Creation
```javascript
// Current timestamp
ObjectId("507f1f77bcf86cd799439011")

// Generate new
ObjectId()
```

### Date Operations
```javascript
// Current date
new Date()

// Specific date
new Date("2024-01-20")

// Date arithmetic (milliseconds)
new Date(Date.now() - 7 * 24 * 60 * 60 * 1000)  // 7 days ago
```

### Operators
```
$eq     - Equal to
$ne     - Not equal to
$gt     - Greater than
$gte    - Greater than or equal
$lt     - Less than
$lte    - Less than or equal
$in     - In array
$nin    - Not in array
$and    - Logical AND
$or     - Logical OR
$not    - Logical NOT
```

---

## ✅ Quick Checklist

- [x] Database created: `studentgroth`
- [x] Collections created
- [x] Indexes created
- [x] Sample data inserted
- [x] Queries tested
- [x] Backup created

---

**All MongoDB commands ready to use! 🚀**
