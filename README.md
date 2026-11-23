# 📚 Your Status — Study Accountability Platform

A full-stack web application designed to help students track their study activities, maintain accountability, compete with peers, and achieve their academic goals through gamification and community engagement.

---

## 🎯 Overview

**Your Status** is a collaborative study tracking platform where students can:
- 📊 Log and monitor study sessions
- 🏆 Compete on leaderboards with other students
- 👥 Join study groups and collaborate
- 📈 Track performance analytics and progress
- 💬 Get real-time notifications and updates
- 🎮 Earn achievements through consistent studying

---

## 🚀 Tech Stack

### **Backend**
- **Framework:** Node.js with Express.js
- **Database:** MongoDB
- **Authentication:** JWT (JSON Web Tokens) + Firebase Admin SDK
- **Real-time Communication:** Socket.IO
- **File Upload:** Cloudinary + Multer
- **Image Processing:** Sharp
- **Security:** bcryptjs for password hashing

### **Frontend**
- **Framework:** React 19 with Vite (ES modules)
- **Styling:** Tailwind CSS (inferred from project)
- **State Management:** Zustand
- **HTTP Client:** Axios
- **Real-time Updates:** Socket.IO Client
- **UI Components:** React Icons + React Hot Toast
- **Routing:** React Router v7
- **Charts:** Recharts for data visualization
- **Date Handling:** Day.js

---

## 📋 Project Structure

```
YOUR_STATUS/
├── Backend/                    # Node.js Express server
│   ├── src/
│   │   ├── config/            # Configuration files
│   │   ├── middleware/        # Express middleware
│   │   ├── models/            # MongoDB models
│   │   ├── routes/            # API endpoints
│   │   ├── sockets/           # Socket.IO event handlers
│   │   └── utils/             # Helper functions
│   ├── uploads/               # File uploads storage
│   ├── index.js               # Server entry point
│   └── package.json
│
├── Frontend/                   # React + Vite app
│   ├── src/
│   │   ├── pages/             # Page components
│   │   ├── components/        # Reusable components
│   │   ├── api/               # API client & endpoints
│   │   ├── config/            # Configuration (Firebase, etc.)
│   │   └── store/             # Zustand state management
│   ├── public/                # Static assets
│   ├── index.html             # HTML entry point
│   ├── vite.config.js         # Vite configuration
│   └── package.json
│
└── README.md                  # This file
```

---

## 🛠️ Installation & Setup

### **Prerequisites**
- Node.js (v14 or higher)
- npm (v6 or higher)
- MongoDB (local or Atlas cloud)

### **Backend Setup**

```bash
cd Backend

# Install dependencies
npm install

# Create .env file
cp .env.example .env

# Configure your environment variables
# MONGO_URI=your_mongodb_connection_string
# PORT=8900
# JWT_SECRET=your_jwt_secret
# NODE_ENV=development

# Start development server
npm run dev

# Or start production server
npm start
```

**Backend runs on:** `http://localhost:8900`

### **Frontend Setup**

```bash
cd Frontend

# Install dependencies
npm install

# Create .env file
cp .env.example .env

# Configure environment variables
# VITE_BACKEND_URL=http://localhost:8900
# VITE_FIREBASE_API_KEY=your_firebase_key
# VITE_FIREBASE_AUTH_DOMAIN=your_firebase_domain
# VITE_FIREBASE_PROJECT_ID=your_firebase_project_id

# Start development server
npm run dev

# Or build for production
npm run build
```

**Frontend runs on:** `http://localhost:5173`

---

## 🔑 Key Features

### **1. User Authentication**
- Email/Password registration and login
- Google OAuth integration
- JWT-based sessions
- Secure password hashing with bcryptjs

### **2. Activity Tracking**
- Start, pause, resume, and stop study sessions
- Track duration and subject categories
- Log activities with descriptions and screenshots
- Real-time activity status updates

### **3. Analytics Dashboard**
- Personal productivity metrics
- Study time trends and patterns
- Subject-wise performance breakdown
- Weekly/Monthly progress tracking

### **4. Leaderboards**
- Global student rankings
- Points-based scoring system
- Real-time leaderboard updates
- Competitive gamification

### **5. Study Groups**
- Create and join study groups
- Group chat and announcements
- Shared learning resources
- Group performance tracking

### **6. Notifications**
- Real-time study reminders
- Achievement notifications
- Group activity updates
- Performance milestones

---

## 🔌 API Endpoints

### **Authentication**
```
POST   /api/auth/register        - Register new user
POST   /api/auth/login           - User login
POST   /api/auth/google          - Google OAuth login
GET    /api/auth/me              - Get current user profile
```

### **Activities**
```
POST   /api/activities           - Create new activity
GET    /api/activities           - Get user's activities
PUT    /api/activities/:id       - Update activity
DELETE /api/activities/:id       - Delete activity
POST   /api/activities/:id/start - Start activity timer
POST   /api/activities/:id/pause - Pause activity timer
POST   /api/activities/:id/resume- Resume activity timer
POST   /api/activities/:id/stop  - Stop activity timer
```

### **Analytics**
```
GET    /api/analytics            - Get user analytics
GET    /api/analytics/summary    - Get summary statistics
GET    /api/analytics/trends     - Get trend data
```

### **Leaderboard**
```
GET    /api/leaderboard          - Get global rankings
GET    /api/leaderboard/friends  - Get friend rankings
```

### **Groups**
```
POST   /api/groups               - Create group
GET    /api/groups               - Get user's groups
GET    /api/groups/:id           - Get group details
PUT    /api/groups/:id           - Update group
POST   /api/groups/:id/join      - Join group
POST   /api/groups/:id/leave     - Leave group
```

---

## 🔄 Real-time Features (Socket.IO)

The application uses Socket.IO for real-time updates:

```javascript
// Activity Events
socket.on('activity:started')
socket.on('activity:paused')
socket.on('activity:resumed')
socket.on('activity:completed')

// Leaderboard Events
socket.on('leaderboard:updated')

// Group Events
socket.on('group:message')
socket.on('group:announcement')

// Notification Events
socket.on('notification:received')
```

---

## 📊 Database Schema (MongoDB)

### **User Model**
```javascript
{
  _id: ObjectId,
  name: String,
  email: String (unique),
  password: String (hashed),
  avatar: String (URL),
  bio: String,
  totalStudyTime: Number,
  points: Number,
  level: Number,
  createdAt: Date,
  updatedAt: Date
}
```

### **Activity Model**
```javascript
{
  _id: ObjectId,
  userId: ObjectId (ref: User),
  subject: String,
  description: String,
  status: String (pending, in_progress, completed),
  startTime: Date,
  endTime: Date,
  duration: Number (in minutes),
  screenshot: String (URL),
  createdAt: Date,
  updatedAt: Date
}
```

### **Group Model**
```javascript
{
  _id: ObjectId,
  name: String,
  description: String,
  image: String (URL),
  members: [ObjectId] (ref: User),
  createdBy: ObjectId (ref: User),
  createdAt: Date,
  updatedAt: Date
}
```

---

## 🧪 Testing

### **Manual Testing**

#### **1. User Registration**
```bash
curl -X POST http://localhost:8900/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "Password@123"
  }'
```

#### **2. User Login**
```bash
curl -X POST http://localhost:8900/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john@example.com",
    "password": "Password@123"
  }'
```

#### **3. Start Study Activity**
```bash
curl -X POST http://localhost:8900/api/activities \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "subject": "Mathematics",
    "description": "Chapter 5 - Calculus"
  }'
```

---

## 🎨 UI/UX Design

### **Color Scheme**
- **Primary:** #3B82F6 (Blue)
- **Secondary:** #10B981 (Green)
- **Accent:** #F59E0B (Amber)
- **Background:** #F9FAFB (Light Gray)
- **Text:** #1F2937 (Dark Gray)

### **Key Pages**

#### **Landing Page**
- Hero section with project overview
- Call-to-action buttons (Sign Up / Login)
- Feature highlights
- Social proof / testimonials

#### **Dashboard**
```
┌─────────────────────────────────────┐
│          Navigation Bar              │
├─────────────────────────────────────┤
│ Welcome, [User]!    [Activity Stats] │
├─────────────────────────────────────┤
│                                     │
│  ┌──────────┐  ┌──────────────────┐ │
│  │ Quick    │  │ Today's Study    │ │
│  │ Activity │  │ Time: 4h 30m     │ │
│  │ Logger   │  │ Rank: #42        │ │
│  └──────────┘  └──────────────────┘ │
│                                     │
│  ┌────────────────────────────────┐ │
│  │     This Week Analytics        │ │
│  │  [Line Chart: Study Hours]     │ │
│  └────────────────────────────────┘ │
│                                     │
│  ┌────────────────────────────────┐ │
│  │     Recent Activities          │ │
│  │ 📚 Math - 1h 30m (Completed)   │ │
│  │ 📖 English - 45m (In Progress) │ │
│  └────────────────────────────────┘ │
│                                     │
└─────────────────────────────────────┘
```

#### **Activity Tracker**
```
┌─────────────────────────────────────┐
│      Study Activity Logger          │
├─────────────────────────────────────┤
│                                     │
│  Subject: [Mathematics ▼]           │
│  Description: [Chapter 5 - Calculus]│
│                                     │
│  ┌─────────────────────────────────┐│
│  │      ▶ START STUDYING           ││
│  └─────────────────────────────────┘│
│                                     │
│  Current Timer: 00:00:00            │
│                                     │
│  [⏸ Pause] [⏹ Stop]                 │
│                                     │
└─────────────────────────────────────┘
```

#### **Leaderboard**
```
┌─────────────────────────────────────┐
│          Global Leaderboard         │
├──────┬──────────────┬────────┬──────┤
│ Rank │ Name         │ Points │ Time │
├──────┼──────────────┼────────┼──────┤
│  1   │ 🥇 Alice     │ 2,500  │ 150h │
│  2   │ 🥈 Bob       │ 2,320  │ 145h │
│  3   │ 🥉 Charlie   │ 2,180  │ 138h │
│  42  │ 👤 You       │ 1,850  │ 112h │
└──────┴──────────────┴────────┴──────┘
```

---

## 🔐 Security Considerations

1. **Password Security:** Hashed with bcryptjs (salt rounds: 10)
2. **JWT Tokens:** Secure HTTP-only cookies (recommended)
3. **CORS:** Configured to allow frontend origin
4. **Environment Variables:** Never commit `.env` files
5. **Firebase Admin:** Used for OAuth verification
6. **Input Validation:** Server-side validation on all endpoints
7. **Rate Limiting:** Recommended for production (not implemented)

---

## 🚨 Known Limitations

- **Firebase OAuth:** Requires Firebase project credentials
- **Image Uploads:** Requires Cloudinary account configuration
- **Database:** Currently requires MongoDB setup
- **Real-time Features:** Limited to users on same connection

---

## 📦 Environment Variables

### **Backend (.env)**
```
NODE_ENV=development
PORT=8900
MONGO_URI=mongodb://localhost:27017/yourstatus
JWT_SECRET=your_super_secret_key_here
CLIENT_URL=http://localhost:5173
FIREBASE_PROJECT_ID=your_firebase_project
FIREBASE_PRIVATE_KEY=your_firebase_private_key
FIREBASE_CLIENT_EMAIL=your_firebase_email
CLOUDINARY_NAME=your_cloudinary_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret
```

### **Frontend (.env)**
```
VITE_BACKEND_URL=http://localhost:8900
VITE_FIREBASE_API_KEY=your_firebase_api_key
VITE_FIREBASE_AUTH_DOMAIN=your_firebase_auth_domain
VITE_FIREBASE_PROJECT_ID=your_firebase_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_firebase_storage_bucket
VITE_FIREBASE_MESSAGING_SENDER_ID=your_firebase_sender_id
VITE_FIREBASE_APP_ID=your_firebase_app_id
```

---

## 🤝 Contributing

1. Create a feature branch: `git checkout -b feature/your-feature`
2. Commit changes: `git commit -m "Add your feature"`
3. Push to branch: `git push origin feature/your-feature`
4. Submit a Pull Request

---

## 📄 License

This project is licensed under the ISC License.

---

## 👨‍💻 Author

**Your Status Team**  
Built with ❤️ for students

---

## 📞 Support

For issues, feature requests, or questions:
- 📧 Email: support@yourstatus.com
- 🐛 Issues: [GitHub Issues](https://github.com/yourusername/your-status)
- 💬 Discussions: [GitHub Discussions](https://github.com/yourusername/your-status)

---

## 🎓 Getting Started (Quick Start)

### **1. Clone the repository**
```bash
git clone https://github.com/yourusername/your-status.git
cd YOUR_STATUS
```

### **2. Install Backend**
```bash
cd Backend
npm install
cp .env.example .env
# Edit .env with your configuration
npm run dev
```

### **3. Install Frontend (in new terminal)**
```bash
cd Frontend
npm install
cp .env.example .env
# Edit .env with your configuration
npm run dev
```

### **4. Open in Browser**
Navigate to `http://localhost:5173`

### **5. Test with Demo Account**
- **Email:** demo@example.com
- **Password:** Demo@1234

---

**Happy Studying! 🚀**
