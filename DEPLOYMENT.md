# 🚀 Vercel Deployment Guide

This guide will help you deploy YOUR_STATUS application to Vercel (both Frontend and Backend).

---

## 📋 Prerequisites

Before you start, ensure you have:

1. **Vercel Account** - Sign up at [vercel.com](https://vercel.com)
2. **GitHub Account** - Your code repository on GitHub
3. **MongoDB Atlas Account** - Cloud database at [mongodb.com/cloud](https://www.mongodb.com/cloud/atlas)
4. **Git** - Installed and configured on your machine

---

## 🗄️ Step 1: Set Up MongoDB Atlas (Cloud Database)

### Why?
Vercel is serverless - local databases won't work. We need MongoDB Atlas (free tier available).

### How?

1. **Go to MongoDB Atlas:** https://www.mongodb.com/cloud/atlas

2. **Create Account** (or login)

3. **Create a Free Cluster:**
   - Click "Create a Deployment"
   - Choose "Shared" (Free tier)
   - Select your region (closest to you)
   - Click "Create Deployment"

4. **Create Database User:**
   - Go to "Database Access" → "Add New Database User"
   - Username: `yourstatus_user`
   - Password: Generate secure password (save it!)
   - Click "Add User"

5. **Create Database:**
   - Go back to "Databases"
   - In your cluster, click "Browse Collections"
   - Click "Create Database"
   - Database name: `yourstatus`
   - Collection name: `users` (or anything)

6. **Get Connection String:**
   - Go to "Databases" → Click "Connect" on your cluster
   - Choose "Connect your application"
   - Copy the connection string
   - Replace `<password>` with your database user password
   - Save this string for later!

   **Example:** 
   ```
   mongodb+srv://yourstatus_user:your_password@cluster.mongodb.net/yourstatus?retryWrites=true&w=majority
   ```

---

## 🔧 Step 2: Prepare Your GitHub Repository

### Push Your Code to GitHub

```bash
cd YOUR_STATUS

# Initialize git (if not already done)
git init

# Add all files
git add .

# Commit
git commit -m "Initial commit for Vercel deployment"

# Add GitHub remote
git remote add origin https://github.com/YOUR_USERNAME/YOUR_STATUS.git

# Push to GitHub
git branch -M main
git push -u origin main
```

> Replace `YOUR_USERNAME` with your GitHub username!

---

## 📱 Step 3: Deploy Backend to Vercel

### Method 1: Using Vercel CLI (Recommended for Beginners)

```bash
# Install Vercel CLI globally
npm install -g vercel

# Navigate to Backend folder
cd Backend

# Login to Vercel
vercel login

# Deploy
vercel --prod

# When prompted:
# - Link to existing project? → No
# - Set project name? → your-status-backend
# - Set directory? → ./
# - Override? → Yes
```

### Method 2: Using Vercel Dashboard

1. Go to [vercel.com/dashboard](https://vercel.com/dashboard)

2. Click "Add New..." → "Project"

3. Select "Import Git Repository"

4. Connect your GitHub account and select `YOUR_STATUS` repository

5. **Configure:**
   - Framework: `Node.js`
   - Root Directory: `Backend/`
   - Build Command: Leave empty (not needed)
   - Start Command: `npm start`

6. **Add Environment Variables:**
   - Click "Environment Variables"
   - Add these variables:

   ```
   NODE_ENV = production
   MONGO_URI = mongodb+srv://yourstatus_user:your_password@cluster.mongodb.net/yourstatus?retryWrites=true&w=majority
   JWT_SECRET = your_super_secret_key_min_32_characters_long_here_12345678
   CLIENT_URL = https://your-frontend-domain.vercel.app
   UPLOADS_DIR = /tmp/uploads
   ```

7. Click "Deploy"

### After Deployment:

- You'll get a URL like: `https://your-status-backend.vercel.app`
- Save this URL! You'll need it for Frontend

---

## 🎨 Step 4: Deploy Frontend to Vercel

### Method 1: Using Vercel CLI

```bash
# Navigate to Frontend folder
cd Frontend

# Deploy
vercel --prod

# When prompted:
# - Link to existing project? → No
# - Set project name? → your-status-frontend
# - Set directory? → ./
# - Override? → Yes
```

### Method 2: Using Vercel Dashboard

1. Go to [vercel.com/dashboard](https://vercel.com/dashboard)

2. Click "Add New..." → "Project"

3. Select "Import Git Repository"

4. Select `YOUR_STATUS` repository

5. **Configure:**
   - Framework: `Vite`
   - Root Directory: `Frontend/`
   - Build Command: `npm run build`
   - Output Directory: `dist`

6. **Add Environment Variables:**
   - Click "Environment Variables"
   - Add:

   ```
   VITE_BACKEND_URL = https://your-status-backend.vercel.app
   VITE_FIREBASE_API_KEY = your_firebase_api_key (if you have one)
   VITE_FIREBASE_AUTH_DOMAIN = your_firebase_auth_domain
   VITE_FIREBASE_PROJECT_ID = your_firebase_project_id
   VITE_FIREBASE_STORAGE_BUCKET = your_firebase_storage_bucket
   VITE_FIREBASE_MESSAGING_SENDER_ID = your_firebase_messaging_sender_id
   VITE_FIREBASE_APP_ID = your_firebase_app_id
   ```

   > If you don't have Firebase keys, leave them empty for now

7. Click "Deploy"

---

## ✅ Step 5: Verify Deployment

### Backend Health Check

```bash
curl https://your-status-backend.vercel.app/api/health
```

You should see:
```json
{
  "status": "ok",
  "database": "connected"
}
```

### Frontend Check

Visit: `https://your-status-frontend.vercel.app`

You should see the landing page with Sign Up / Login

---

## 🔗 Step 6: Connect Frontend & Backend

The Frontend will automatically use the backend URL from environment variables.

**Frontend will connect to:**
```
https://your-status-backend.vercel.app
```

---

## 🧪 Testing After Deployment

### 1. Test Registration

```bash
curl -X POST https://your-status-backend.vercel.app/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Test User",
    "email": "test@example.com",
    "password": "Password@123"
  }'
```

### 2. Test Login

```bash
curl -X POST https://your-status-backend.vercel.app/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "Password@123"
  }'
```

### 3. Visit Frontend

Open in browser: `https://your-status-frontend.vercel.app`

Try signing up with a new account

---

## 🔧 Troubleshooting

### **Issue: Backend returns 503 (Firebase not configured)**
**Solution:** Add Firebase keys to environment variables or ignore Google OAuth for now

### **Issue: Frontend can't connect to Backend**
**Solution:** 
- Check `VITE_BACKEND_URL` environment variable
- Verify Backend is deployed and running
- Check CORS settings in Backend

### **Issue: MongoDB connection fails**
**Solution:**
- Verify `MONGO_URI` in environment variables
- Check MongoDB Atlas IP whitelist (add `0.0.0.0/0` for all IPs)
- Ensure password in connection string is URL-encoded

### **Issue: Vercel build fails**
**Solution:**
- Check build logs in Vercel dashboard
- Ensure `vercel.json` is correctly configured
- Run `npm run build` locally to debug

### **Issue: "Module not found" error**
**Solution:**
```bash
# In Backend folder
npm install

# In Frontend folder
npm install

# Then push to GitHub
git add package-lock.json
git commit -m "Update dependencies"
git push
```

---

## 🔒 Security Checklist

Before going to production:

- [ ] Never commit `.env` file (use environment variables in Vercel dashboard)
- [ ] Generate strong `JWT_SECRET` (minimum 32 characters)
- [ ] Add `CLIENT_URL` to point to your frontend domain
- [ ] Enable CORS only for your frontend domain
- [ ] MongoDB Atlas: Whitelist only necessary IPs
- [ ] Use HTTPS everywhere (Vercel handles this automatically)
- [ ] Keep dependencies updated: `npm audit`

---

## 📊 Monitoring & Logs

### View Backend Logs

1. Go to Vercel Dashboard → Select Backend Project
2. Click "Deployments" tab
3. Click latest deployment
4. Click "Runtime Logs" or "Build Logs"

### View Frontend Logs

1. Go to Vercel Dashboard → Select Frontend Project
2. Click "Deployments" tab
3. Same as above

---

## 🔄 Updating After Deployment

### Push new changes:

```bash
# Make changes locally
# ... edit files ...

# Commit and push
git add .
git commit -m "Your message"
git push origin main
```

Vercel will automatically:
1. Detect the push
2. Run build
3. Deploy new version

---

## 📱 Custom Domain (Optional)

1. Go to your project in Vercel Dashboard
2. Click "Settings" → "Domains"
3. Add your custom domain
4. Follow DNS setup instructions

---

## 🎓 Summary

| Component | URL | Service |
|-----------|-----|---------|
| Backend | `https://your-status-backend.vercel.app` | Vercel (Serverless) |
| Frontend | `https://your-status-frontend.vercel.app` | Vercel (Static) |
| Database | Cloud Connection | MongoDB Atlas |

---

## 💡 Tips

- **Free Tier:** Vercel and MongoDB Atlas offer generous free tiers
- **Auto-scaling:** Vercel automatically scales your app
- **Fast Deployment:** Typically takes 1-2 minutes
- **Custom Domains:** Add your own domain easily
- **Environment Variables:** Keep sensitive data in Vercel, not in code

---

## 🆘 Need Help?

- **Vercel Docs:** https://vercel.com/docs
- **MongoDB Atlas Docs:** https://docs.mongodb.com/atlas/
- **Express + Vercel:** https://vercel.com/guides/deploying-an-express-app-with-vercel

---

**Happy Deploying! 🚀**
