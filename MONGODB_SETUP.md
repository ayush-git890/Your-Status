# 🗄️ MongoDB Atlas Setup Guide

This guide walks you through setting up MongoDB Atlas for YOUR_STATUS deployment.

---

## 🎯 Why MongoDB Atlas?

- ✅ **Free Tier:** 512 MB storage (perfect for learning/testing)
- ✅ **Cloud Hosted:** No local database needed
- ✅ **Easy Setup:** 5 minutes to get running
- ✅ **Scalable:** Upgrade anytime
- ✅ **Secure:** Built-in authentication

---

## 🔧 Step-by-Step Setup

### **Step 1: Create MongoDB Atlas Account**

1. Go to: https://www.mongodb.com/cloud/atlas
2. Click "Start Free"
3. Sign up with email/password or Google
4. Verify your email

### **Step 2: Create Your First Cluster**

1. After login, you'll see "Create a Deployment"
2. Click "Create"
3. Choose **Shared** (Free tier)
4. Select your region (choose closest to you)
5. Click **Create Deployment**
6. Wait 1-2 minutes for cluster to initialize

### **Step 3: Create Database User**

1. In MongoDB Atlas dashboard, go to **"Database Access"**
2. Click **"Add New Database User"**
3. Fill in details:
   - **Username:** `yourstatus_user`
   - **Password:** Click "Auto-generate secure password" (save this!)
   - **User Privileges:** Select "Built-in Role" → "Read and write to any database"
4. Click **"Add User"**
5. Copy and save the password somewhere safe!

### **Step 4: Whitelist Your IP Address**

1. Go to **"Network Access"**
2. Click **"Add IP Address"**
3. Click **"Allow Access from Anywhere"** (for Vercel: use `0.0.0.0/0`)
   - This allows Vercel to connect to your database
   - Alternative: Whitelist Vercel IPs (more complex, but more secure)
4. Click **"Confirm"**

### **Step 5: Get Connection String**

1. Go to **"Databases"** tab
2. Click **"Connect"** on your cluster
3. Choose **"Connect your application"**
4. Select **"Node.js"** driver and version **5.9**
5. Copy the connection string

**Example:**
```
mongodb+srv://yourstatus_user:password123@cluster0.abc123.mongodb.net/yourstatus?retryWrites=true&w=majority
```

### **Step 6: Prepare Connection String**

The connection string has a placeholder `<password>`:

```
mongodb+srv://yourstatus_user:<password>@cluster0.abc123.mongodb.net/yourstatus?retryWrites=true&w=majority
```

Replace `<password>` with your actual database user password.

**⚠️ Important:** If your password contains special characters, URL-encode them:

```
Character | Encoded
==========|==========
!         | %21
@         | %40
#         | %23
$         | %24
%         | %25
&         | %26
=         | %3D
```

**Example:** 
- Password: `Pass@123#`
- Encoded: `Pass%40123%23`
- Final: `mongodb+srv://yourstatus_user:Pass%40123%23@cluster0.abc123.mongodb.net/yourstatus?retryWrites=true&w=majority`

---

## 📝 Adding to Vercel

Once you have your connection string:

1. Go to Vercel Dashboard
2. Select Backend project
3. Go to **Settings** → **Environment Variables**
4. Add variable:
   - **Name:** `MONGO_URI`
   - **Value:** Your complete connection string with password
5. Click **Save**
6. Redeploy Backend

---

## 🧪 Test Connection

### Using MongoDB Compass (GUI)

1. Download MongoDB Compass: https://www.mongodb.com/products/compass
2. Click "New Connection"
3. Paste your connection string
4. Click "Connect"
5. You should see your database!

### Using Node.js

```javascript
const mongoose = require('mongoose');

const mongoUri = 'mongodb+srv://yourstatus_user:password@cluster0.abc123.mongodb.net/yourstatus?retryWrites=true&w=majority';

mongoose.connect(mongoUri)
  .then(() => console.log('Connected to MongoDB!'))
  .catch(err => console.error('Connection failed:', err));
```

---

## 📊 Database Structure

MongoDB will automatically create collections as you use them:

```
yourstatus (database)
├── users (collection)
├── activities (collection)
├── groups (collection)
├── feeditems (collection)
├── leaderboards (collection)
└── notifications (collection)
```

---

## 🔐 Security Best Practices

✅ **Do:**
- Use strong passwords (12+ characters, mixed case, numbers, symbols)
- Use `0.0.0.0/0` only for testing (Vercel requires it)
- Regularly rotate passwords
- Keep connection string secret
- Never commit connection string in code
- Use environment variables only

❌ **Don't:**
- Use simple passwords like "123456"
- Share connection string publicly
- Commit `.env` file to GitHub
- Use same password for multiple services
- Whitelist `0.0.0.0/0` for production (unless you know what you're doing)

---

## 🆘 Troubleshooting

### **"Authentication failed"**
- Check username and password are correct
- Ensure password is URL-encoded if it has special characters
- Verify database user exists in "Database Access"

### **"Connection timeout"**
- Check IP address is whitelisted in "Network Access"
- Try using `0.0.0.0/0` temporarily
- Verify cluster is running (not paused)
- Check Vercel location is in list of whitelisted IPs

### **"Database not found"**
- MongoDB automatically creates databases on first write
- Just start using it - it will create automatically
- Or manually create in "Databases" tab

### **"Too many connections"**
- MongoDB Atlas has connection limits on free tier
- Reduce concurrent connections
- Upgrade to paid plan if needed

---

## 💾 Backup & Recovery

### **Export Data**

1. Go to your cluster → **Collections**
2. Right-click collection → **Export Collection**
3. Choose CSV or JSON format
4. Download file

### **Restore Data**

1. Go to your cluster → **Collections**
2. Click **+ Insert Document**
3. Upload your backup file
4. Data is restored!

---

## 🚀 Upgrading from Free Tier

If you outgrow free tier:

1. Go to **Billing**
2. Click **Change Cluster Tier**
3. Select paid tier (Dedicated M10 or higher)
4. Update payment info
5. Click **Confirm Upgrade**

Upgrade takes ~10 minutes. No data loss!

---

## 📱 Free Tier Limits

| Feature | Limit |
|---------|-------|
| Storage | 512 MB |
| Connections | 100 concurrent |
| Memory | Shared with other users |
| Automated Backups | 7 days |
| Snapshots | 2 per day |

Perfect for:
- ✅ Development
- ✅ Testing
- ✅ Small hobby projects
- ✅ Learning

Not suitable for:
- ❌ High-traffic production
- ❌ Large data volumes
- ❌ Critical business apps

---

## 📞 Support

- **MongoDB Docs:** https://docs.mongodb.com
- **Atlas Docs:** https://docs.atlas.mongodb.com
- **Connection String Help:** https://docs.mongodb.com/manual/reference/connection-string

---

## ✅ Checklist

Before moving to Vercel deployment:

- [ ] MongoDB Atlas account created
- [ ] Cluster deployed and running
- [ ] Database user created
- [ ] IP address whitelisted (0.0.0.0/0)
- [ ] Connection string copied
- [ ] Password URL-encoded (if needed)
- [ ] Connection tested locally
- [ ] Connection string saved securely

---

**You're ready to deploy! 🚀**

Next: Follow [DEPLOYMENT_CHECKLIST.md](./DEPLOYMENT_CHECKLIST.md) to deploy to Vercel.
