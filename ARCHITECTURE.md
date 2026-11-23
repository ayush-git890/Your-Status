# 🏗️ Deployment Architecture

This document shows the system architecture after deployment to Vercel.

---

## 🌐 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        YOUR USERS                               │
│                    (In their browser)                           │
└───────────────────────────────┬─────────────────────────────────┘
                                │
                    HTTPS Connection (Secure)
                                │
        ┌───────────────────────┴───────────────────────┐
        │                                               │
┌───────▼──────────────────────┐         ┌──────────────▼────────┐
│  FRONTEND (React + Vite)     │         │  BACKEND (Node.js)    │
│  Hosted on Vercel            │         │  Hosted on Vercel     │
│                              │         │                       │
│  https://your-frontend.      │  HTTPS  │  https://your-backend.│
│  vercel.app                  │◄───────►│  vercel.app           │
│                              │  /api   │                       │
│  - React App                 │         │  - Express Server     │
│  - Routing                   │         │  - API Endpoints      │
│  - User Interface            │         │  - Authentication     │
│  - State Management          │         │  - Business Logic     │
└──────────────────────────────┘         └──────────────┬────────┘
                                                        │
                                     MongoDB Connection (Secure)
                                                        │
                        ┌───────────────────────────────┴────────────┐
                        │                                            │
                ┌───────▼──────────────────────────────────────────┐
                │  MONGODB ATLAS (Cloud Database)                 │
                │                                                 │
                │  mongodb+srv://user:pass@cluster.mongodb.net   │
                │                                                 │
                │  - Users Collection                            │
                │  - Activities Collection                       │
                │  - Groups Collection                           │
                │  - Notifications Collection                    │
                │  - Leaderboard Data                            │
                └─────────────────────────────────────────────────┘
```

---

## 🔄 Data Flow

### **User Registration Flow**

```
User Browser
    │
    │ 1. Submit form
    │ (Email, Password, Name)
    ▼
Frontend (React)
    │
    │ 2. Validate & send HTTPS POST
    │ /api/auth/register
    ▼
Backend (Express)
    │
    │ 3. Validate data
    │ 4. Hash password
    │ 5. Save to database
    ▼
MongoDB Atlas
    │
    │ 6. Store user document
    │
    ▼
Backend Response
    │
    │ 7. Return JWT token & user data
    ▼
Frontend
    │
    │ 8. Store token locally
    │ 9. Redirect to Dashboard
    ▼
User Dashboard
    │
    └─ Welcome! 🎉
```

---

## 🔐 Security Layers

```
┌─ HTTPS Encryption
│  └─ All connections between browser and servers are encrypted
│
├─ Password Hashing
│  └─ bcryptjs hashes passwords before storage
│
├─ JWT Authentication
│  └─ Secure tokens for authenticated requests
│
├─ Database Credentials
│  └─ Connection string stored only in Vercel env vars
│  └─ Never exposed in code or browser
│
├─ CORS Configuration
│  └─ Only frontend domain can access backend
│
└─ MongoDB Authentication
   └─ Username/password required for database access
```

---

## 📱 Component Breakdown

### **Frontend (Vercel)**
```
Vercel Edge Network (CDN)
    ├─ Your Domain (Custom or vercel.app)
    ├─ Static Assets (HTML, CSS, JS)
    ├─ React Components
    ├─ Socket.IO Client
    └─ Local Storage for JWT token
```

### **Backend (Vercel Serverless)**
```
Vercel Serverless Functions
    ├─ /api/auth (Authentication)
    ├─ /api/activities (Study sessions)
    ├─ /api/leaderboard (Rankings)
    ├─ /api/groups (Study groups)
    ├─ /api/analytics (Statistics)
    └─ /api/health (System status)
```

### **Database (MongoDB Atlas)**
```
Shared Cluster (Free tier)
    ├─ Authentication: Username + Password
    ├─ Encryption: TLS/SSL
    ├─ Backups: Automatic daily
    ├─ Availability: 99.9% uptime
    └─ Scalability: Upgrade anytime
```

---

## 🚀 Deployment Pipeline

```
Your Local Machine
    │
    │ git push origin main
    │
    ▼
GitHub Repository
    │
    │ Webhook notification
    │ (GitHub → Vercel)
    │
    ▼ ┌─────────────────────┐
    │ │ FRONTEND PIPELINE   │
    │ └─────────────────────┘
    │   1. Clone code
    │   2. npm install
    │   3. npm run build (Vite)
    │   4. Optimize bundle
    │   5. Deploy to CDN
    │   6. Test
    │   ✅ Live!
    │
    └─ ┌─────────────────────┐
       │ BACKEND PIPELINE    │
       └─────────────────────┘
         1. Clone code
         2. npm install
         3. Install dependencies
         4. Test connection to MongoDB
         5. Deploy as serverless functions
         6. Test health check
         ✅ Live!
```

---

## 🌍 Global Availability

### **Frontend (CDN - Edge Network)**
```
Your Content ──┐
               ├─► Vercel Global Edge Network
               │
               ├─► Cached in 100+ locations worldwide
               ├─► Served from nearest location to user
               └─► Sub-100ms load times globally
```

### **Backend (Serverless)**
```
API Requests ──┐
               ├─► Automatic scaling
               ├─► 100+ server instances available
               ├─► Auto-scales based on traffic
               └─► No server management needed
```

### **Database (Cluster)**
```
MongoDB ──┐
          ├─► Primary database in selected region
          ├─► Automatic failover to replica
          ├─► Secure connection from backend
          └─► Daily backups
```

---

## 📊 Real-Time Communication

### **WebSocket (Socket.IO) Flow**

```
User 1 Browser
    │
    │ Sends: "user:activity-started"
    │
    ▼
Backend (Socket.IO Server)
    │
    │ Broadcasts to all connected users
    │
    ├──► User 2 Browser (receives update)
    ├──► User 3 Browser (receives update)
    └──► User 4 Browser (receives update)

Result: Real-time leaderboard updates!
```

---

## 🔧 Environment Variables

### **Backend Secrets (In Vercel Dashboard)**
```
✓ NODE_ENV = production
✓ MONGO_URI = mongodb+srv://user:pass@cluster...
✓ JWT_SECRET = super_secret_key_here
✓ CLIENT_URL = https://your-frontend.vercel.app
✓ UPLOADS_DIR = /tmp/uploads
✓ FIREBASE_* = (optional)
✓ CLOUDINARY_* = (optional)
```

### **Frontend Secrets (In Vercel Dashboard)**
```
✓ VITE_BACKEND_URL = https://your-backend.vercel.app
✓ VITE_FIREBASE_* = (optional)
```

---

## 📈 Scalability

### **What Happens as You Grow**

```
Phase 1: 0-100 Users (Free Tier)
├─ Vercel: Shared resources ✓
├─ MongoDB: 512 MB storage ✓
└─ Works perfectly!

Phase 2: 100-1,000 Users (Still Free)
├─ Vercel: Auto-scaling kicks in ✓
├─ MongoDB: Approaching 512 MB limit
└─ Upgrade MongoDB to M2 (paid, $57/month)

Phase 3: 1,000+ Users (Growth)
├─ Vercel: Still free, handling thousands ✓
├─ MongoDB: Upgrade to M10+ ($250+/month)
└─ Consider CDN for media files
```

---

## ⚙️ System Requirements

| Component | Requirement | Status |
|-----------|-------------|--------|
| Browser | Modern (Chrome, Firefox, Safari, Edge) | ✅ |
| Internet | Broadband connection | ✅ |
| Node.js | v14+ (Backend) | ✅ Vercel handles |
| MongoDB | v5.0+ | ✅ Vercel handles |
| HTTPS | Required | ✅ Vercel handles |
| DNS | Custom domain (optional) | ✅ Vercel handles |

---

## 🛡️ Fault Tolerance

```
If Frontend goes down:
  └─ Other Vercel projects not affected
  └─ CDN still serves cached content
  └─ Automatic redeploy available

If Backend goes down:
  └─ Automatic failover to new instance
  └─ No manual intervention needed
  └─ Logs available in Vercel dashboard

If Database goes down:
  └─ MongoDB automatic failover to replica
  └─ 99.9% uptime SLA
  └─ Automatic backups restored
```

---

## 📡 Network Architecture

```
                    Internet
                       │
        ┌──────────────┼──────────────┐
        │              │              │
    ┌───▼───┐      ┌───▼───┐    ┌────▼────┐
    │ Europe│      │ USA   │    │ Asia    │
    │ CDN   │      │ CDN   │    │ CDN     │
    └───┬───┘      └───┬───┘    └────┬────┘
        │              │              │
        └──────────────┼──────────────┘
                       │
            ┌──────────▼──────────┐
            │  Vercel Edge Network│
            │  (Global)           │
            └──────────┬──────────┘
                       │
        ┌──────────────┼──────────────┐
        │              │              │
    ┌───▼─────┐    ┌───▼──────┐  ┌───▼──────┐
    │Frontend │    │Backend   │  │Database  │
    │(React)  │    │(Express) │  │(MongoDB) │
    └────────┘    └──────────┘  └──────────┘
```

---

## 🎯 Deployment Goals Met

✅ **Zero Downtime:** Blue-green deployments
✅ **Automatic Scaling:** Handles traffic spikes
✅ **Security:** HTTPS, authentication, encryption
✅ **Performance:** Global CDN, optimized
✅ **Reliability:** Automatic failover, backups
✅ **Monitoring:** Vercel logs and analytics
✅ **Cost Effective:** Free tier sufficient for MVP
✅ **Easy Updates:** Git push = instant deploy

---

## 📞 Monitoring & Support

### **Vercel Dashboard**
- Deployment history
- Build logs
- Runtime logs
- Performance metrics
- Downtime alerts

### **MongoDB Atlas Dashboard**
- Database status
- Connection metrics
- Backup status
- Storage usage
- Database alerts

---

**Your app is now production-ready! 🚀**

For deployment instructions, see [DEPLOYMENT_CHECKLIST.md](./DEPLOYMENT_CHECKLIST.md)
