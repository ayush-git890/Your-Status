# ⚡ Quick Deployment Checklist

Use this checklist to deploy YOUR_STATUS to Vercel quickly!

---

## 🔧 Pre-Deployment (Do Once)

- [ ] Create Vercel account: https://vercel.com/signup
- [ ] Create MongoDB Atlas account: https://www.mongodb.com/cloud/atlas
- [ ] Create GitHub repository and push code
- [ ] Set up MongoDB Atlas cluster and get connection string

---

## 🚀 Deployment Steps

### Step 1: Deploy Backend

```bash
# Option A: Using Vercel CLI
npm install -g vercel
cd Backend
vercel login
vercel --prod

# Option B: Using Vercel Dashboard
# 1. Go to https://vercel.com/dashboard
# 2. Click "Add New" → "Project"
# 3. Import GitHub repository
# 4. Set Root Directory: Backend/
# 5. Add Environment Variables (see Step 3)
# 6. Deploy
```

**After Deploy:**
- [ ] Get Backend URL (e.g., `https://your-backend.vercel.app`)
- [ ] Test: `curl https://your-backend.vercel.app/api/health`

### Step 2: Configure Backend Environment Variables

Go to Vercel Dashboard → Backend Project → Settings → Environment Variables

Add these variables:

```
NODE_ENV = production
MONGO_URI = mongodb+srv://user:password@cluster.mongodb.net/yourstatus?retryWrites=true&w=majority
JWT_SECRET = your_super_secret_key_min_32_characters_long_12345
CLIENT_URL = https://your-frontend.vercel.app
UPLOADS_DIR = /tmp/uploads
```

- [ ] Added all Backend environment variables
- [ ] Redeployed Backend (redeploy needed after env changes)

### Step 3: Deploy Frontend

```bash
# Option A: Using Vercel CLI
cd Frontend
vercel --prod

# Option B: Using Vercel Dashboard
# 1. Go to https://vercel.com/dashboard
# 2. Click "Add New" → "Project"
# 3. Import GitHub repository
# 4. Set Root Directory: Frontend/
# 5. Build Command: npm run build
# 6. Output Directory: dist
# 7. Add Environment Variables (see Step 4)
# 8. Deploy
```

**After Deploy:**
- [ ] Get Frontend URL (e.g., `https://your-frontend.vercel.app`)
- [ ] Visit URL in browser and verify landing page loads

### Step 4: Configure Frontend Environment Variables

Go to Vercel Dashboard → Frontend Project → Settings → Environment Variables

Add:

```
VITE_BACKEND_URL = https://your-backend.vercel.app
```

Optional (if you have Firebase):
```
VITE_FIREBASE_API_KEY = your_key
VITE_FIREBASE_AUTH_DOMAIN = your_domain
VITE_FIREBASE_PROJECT_ID = your_project_id
VITE_FIREBASE_STORAGE_BUCKET = your_bucket
VITE_FIREBASE_MESSAGING_SENDER_ID = your_sender_id
VITE_FIREBASE_APP_ID = your_app_id
```

- [ ] Added Frontend environment variables
- [ ] Redeployed Frontend

---

## ✅ Post-Deployment Testing

### Test Backend API

```bash
# Replace with your actual backend URL
curl https://your-status-backend.vercel.app/api/health
```

Expected Response:
```json
{"status": "ok", "database": "connected"}
```

- [ ] Backend health check passes

### Test User Registration

```bash
curl -X POST https://your-status-backend.vercel.app/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Test User",
    "email": "test@example.com",
    "password": "Password@123"
  }'
```

Expected: 201 Created with user data

- [ ] Registration API works

### Test Frontend

1. Open browser: `https://your-status-frontend.vercel.app`
2. Try signing up with test email
3. Try logging in

- [ ] Frontend loads
- [ ] Can create account
- [ ] Can login
- [ ] Dashboard displays

---

## 🔐 Security Checklist

- [ ] Never commit `.env` file
- [ ] All secrets are in Vercel environment variables only
- [ ] `JWT_SECRET` is strong (32+ characters)
- [ ] `MONGO_URI` password is URL-encoded
- [ ] MongoDB Atlas has IP whitelist configured
- [ ] Frontend `VITE_BACKEND_URL` points to deployed backend
- [ ] CORS is configured correctly in Backend

---

## 📱 Final URLs

After deployment, you'll have:

```
Frontend URL:  https://your-status-frontend.vercel.app
Backend URL:   https://your-status-backend.vercel.app
Database:      MongoDB Atlas (Cloud)
```

Share the **Frontend URL** with users!

---

## 🔄 Updating After Deployment

After deployment, any changes you push to GitHub will auto-deploy:

```bash
# Make changes locally
# ... edit files ...

# Push to GitHub
git add .
git commit -m "Your message"
git push origin main

# Vercel will automatically build and deploy!
```

---

## 🆘 If Something Goes Wrong

### Backend not connecting to database
- Check `MONGO_URI` in Vercel environment variables
- Verify password is URL-encoded in connection string
- Check MongoDB Atlas IP whitelist (add `0.0.0.0/0`)

### Frontend can't reach backend
- Verify `VITE_BACKEND_URL` environment variable
- Check if backend is running: `curl your-backend-url/api/health`
- Check browser console for CORS errors

### Build fails
- Check "Deployments" → "Build Logs" in Vercel dashboard
- Try building locally: `npm run build`
- Ensure all dependencies are installed: `npm install`

### Pages show 404
- Check `vercel.json` configuration
- Ensure framework is correctly set (Vite for Frontend, Node for Backend)

---

## 📞 Support

- Vercel Docs: https://vercel.com/docs
- MongoDB Docs: https://docs.mongodb.com
- Full Deployment Guide: See `DEPLOYMENT.md`

---

**You got this! 🚀**
