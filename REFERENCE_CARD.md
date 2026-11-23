# 📋 Deployment Reference Card

A quick reference guide with all important information at a glance.

---

## 🔗 Quick Links

| Resource | URL |
|----------|-----|
| Vercel Dashboard | https://vercel.com/dashboard |
| MongoDB Atlas | https://www.mongodb.com/cloud/atlas |
| GitHub | https://github.com |
| Vercel Docs | https://vercel.com/docs |
| MongoDB Docs | https://docs.mongodb.com |

---

## 🔐 Credentials to Keep Safe

```
❌ NEVER COMMIT THESE TO GITHUB ❌

Backend Environment Variables:
  - MONGO_URI (database connection)
  - JWT_SECRET (authentication)

Frontend Environment Variables:
  - VITE_BACKEND_URL (API endpoint)

Store ONLY in Vercel dashboard!
```

---

## 📝 Configuration Checklist

### Backend Environment Variables
```
NODE_ENV=production
PORT=3000
MONGO_URI=mongodb+srv://user:pass@cluster.mongodb.net/yourstatus?retryWrites=true&w=majority
JWT_SECRET=your_super_secret_key_min_32_characters_long_here
CLIENT_URL=https://your-frontend.vercel.app
UPLOADS_DIR=/tmp/uploads
```

### Frontend Environment Variables
```
VITE_BACKEND_URL=https://your-backend.vercel.app
```

---

## 🚀 Deployment Commands

### Push to GitHub
```bash
cd YOUR_STATUS
git add .
git commit -m "Prepare for Vercel deployment"
git push origin main
```

### Using Vercel CLI
```bash
# Install
npm install -g vercel

# Backend
cd Backend
vercel login
vercel --prod

# Frontend (in another terminal)
cd Frontend
vercel --prod
```

---

## 🧪 Testing Commands

### Backend Health Check
```bash
curl https://your-status-backend.vercel.app/api/health
```

### Create Account
```bash
curl -X POST https://your-status-backend.vercel.app/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Test User",
    "email": "test@example.com",
    "password": "Password@123"
  }'
```

### Login
```bash
curl -X POST https://your-status-backend.vercel.app/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "Password@123"
  }'
```

---

## 📂 Project Structure

```
YOUR_STATUS/
├── Backend/
│   ├── vercel.json ✅ (Updated)
│   ├── .env.example ✅ (Updated)
│   ├── package.json
│   ├── index.js
│   └── src/
│       ├── routes/
│       ├── models/
│       ├── middleware/
│       └── config/
│
├── Frontend/
│   ├── vercel.json ✅ (Updated)
│   ├── .env.example ✅ (Updated)
│   ├── package.json
│   ├── vite.config.js
│   └── src/
│       ├── pages/
│       ├── components/
│       ├── api/
│       └── config/
│
└── Documentation/
    ├── README.md
    ├── INDEX.md
    ├── DEPLOYMENT.md
    ├── DEPLOYMENT_CHECKLIST.md
    ├── MONGODB_SETUP.md
    ├── VERCEL_SUMMARY.md
    ├── ARCHITECTURE.md
    ├── VISUAL_GUIDE.md
    └── REFERENCE_CARD.md (this file)
```

---

## 🎯 Deployment Steps Summary

### Step 1: Prepare (5 min)
- [ ] Code ready on GitHub
- [ ] No sensitive data in commits
- [ ] `.env` file excluded from git

### Step 2: MongoDB (10 min)
- [ ] Create MongoDB Atlas account
- [ ] Create cluster (free tier)
- [ ] Create database user
- [ ] Whitelist IP (0.0.0.0/0 for Vercel)
- [ ] Get connection string

### Step 3: Backend (5 min)
- [ ] Deploy to Vercel
- [ ] Add environment variables
- [ ] Test health check

### Step 4: Frontend (5 min)
- [ ] Deploy to Vercel
- [ ] Add environment variables
- [ ] Test page loads

### Step 5: Testing (5 min)
- [ ] Backend responds ✅
- [ ] Frontend loads ✅
- [ ] Can create account ✅
- [ ] Can login ✅

**Total Time: ~30 minutes**

---

## 🌐 API Endpoints Reference

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/health` | Health check |
| POST | `/api/auth/register` | Create account |
| POST | `/api/auth/login` | User login |
| GET | `/api/auth/me` | Get current user |
| POST | `/api/activities` | Create activity |
| GET | `/api/activities` | Get activities |
| POST | `/api/activities/:id/start` | Start timer |
| POST | `/api/activities/:id/stop` | Stop timer |
| GET | `/api/leaderboard` | Get rankings |
| GET | `/api/analytics` | Get stats |

---

## 📊 Deployment Comparison

| Feature | Local | Vercel |
|---------|-------|--------|
| Cost | $0 | $0 (free tier) |
| Uptime | When laptop on | 99.9% |
| Speed | Localhost | Global CDN |
| Sharing | Only local network | Worldwide internet |
| Database | Local MongoDB | MongoDB Atlas |
| Backups | Manual | Automatic |
| Scaling | Limited | Unlimited |

---

## 🔒 Security Checklist

- [ ] No `.env` file in git
- [ ] All secrets in Vercel env vars
- [ ] JWT_SECRET is 32+ characters
- [ ] MONGO_URI has password
- [ ] MongoDB IP whitelist configured
- [ ] HTTPS enabled (Vercel handles)
- [ ] CORS configured
- [ ] No hardcoded secrets in code

---

## 🆘 Quick Troubleshooting

| Problem | Solution |
|---------|----------|
| 503 Backend error | Add Firebase keys to env vars |
| Can't connect to backend | Check VITE_BACKEND_URL env var |
| MongoDB timeout | Whitelist IP 0.0.0.0/0 in Atlas |
| Build fails | Check vercel.json configuration |
| 404 on routes | Verify React Router rewrites in vercel.json |

---

## 📱 After Deployment URLs

```
Bookmarks to save:

Frontend: https://your-status-frontend.vercel.app
Backend:  https://your-status-backend.vercel.app
Vercel:   https://vercel.com/dashboard
MongoDB:  https://cloud.mongodb.com
```

---

## 🔄 Git Workflow After Deployment

```bash
# Make changes
# ... edit files ...

# Commit
git add .
git commit -m "Describe your changes"

# Push (this triggers Vercel deployment)
git push origin main

# Vercel automatically:
# - Builds
# - Tests
# - Deploys
# ✅ Done in ~2-3 minutes
```

---

## 📞 Support Channels

| Issue | Solution |
|-------|----------|
| Vercel problems | Vercel Docs / Dashboard Logs |
| MongoDB issues | MongoDB Docs / Atlas Dashboard |
| Code bugs | Check local dev env first |
| Deployment stuck | Check Vercel build logs |

---

## 💾 Important Files Reference

### Backend Files
- `index.js` - Main server entry
- `src/routes/auth.js` - Authentication endpoints
- `vercel.json` - Deployment configuration
- `.env.example` - Environment template

### Frontend Files
- `src/pages/Landing.jsx` - Login/signup page
- `src/api/index.js` - API client
- `vite.config.js` - Build configuration
- `vercel.json` - Deployment configuration

---

## 📈 Metrics to Monitor

After deployment, check:

```
Vercel Dashboard:
  - Deployment status
  - Build time
  - Runtime logs
  - Performance metrics

MongoDB Atlas:
  - Database size (keep under 512 MB)
  - Connection count
  - Query performance
  - Backups status
```

---

## 🎓 Key Concepts

| Term | Meaning |
|------|---------|
| **Vercel** | Hosting platform for frontend & backend |
| **Serverless** | No servers to manage; auto-scaling |
| **MongoDB Atlas** | Cloud database service |
| **Environment Variables** | Secrets stored securely in Vercel |
| **JWT** | Secure authentication token |
| **CORS** | Cross-Origin Resource Sharing security |
| **CDN** | Content Delivery Network (global speed) |

---

## ⚡ Performance Tips

```
Frontend (Vite):
  - Build output: dist/
  - Served from global CDN
  - Automatically optimized
  - Cache headers configured

Backend (Express):
  - Serverless functions on-demand
  - Auto-scales with traffic
  - Cold start: ~500ms first request
  - Warm: <100ms subsequent

Database (MongoDB):
  - Free tier: 512 MB
  - Upgrade if needed
  - Auto-replication
  - Daily backups
```

---

## 🔐 Sensitive Data Locations

✅ **Correctly stored:**
- Vercel environment variables dashboard
- MongoDB Atlas UI

❌ **NEVER store in:**
- Source code
- .env files committed to git
- Comments
- Logs (use secure logging)

---

## 📋 Final Pre-Launch Checklist

- [ ] Code on GitHub
- [ ] MongoDB Atlas running
- [ ] Backend deployed to Vercel
- [ ] Frontend deployed to Vercel
- [ ] All env vars configured
- [ ] Backend health check working
- [ ] Frontend loads in browser
- [ ] Can create account
- [ ] Can login successfully
- [ ] Dashboard displays correctly
- [ ] No sensitive data exposed
- [ ] Ready to share! 🎉

---

## 🚀 Launch Announcement Template

```
🎉 YOUR_STATUS is now LIVE!

📱 Visit: https://your-status-frontend.vercel.app

Features:
✓ Track your study sessions
✓ Compete on leaderboards
✓ Join study groups
✓ Get real-time notifications
✓ Analyze your progress

👉 Sign up now and start studying!
```

---

## 📞 Quick Contact Info

| Need Help With | Contact |
|----------------|---------|
| Vercel hosting | https://vercel.com/support |
| MongoDB database | https://www.mongodb.com/support |
| General coding | Stack Overflow / GitHub Issues |
| This project | Check documentation files |

---

**Bookmark this page! 📌**

Print this or save as PDF for quick reference during deployment.

---

**Happy Deployment! 🚀**
