# 🚀 Railway Deployment Guide - TDS Virtual TA

Deploy your complete RAG Query system to Railway - the easiest all-in-one platform!

## 📋 Prerequisites

1. **GitHub Account** (free)
2. **Railway Account** (free tier: $5 credit/month)
3. **Your API Keys:**
   - `AIPROXY_TOKEN` (for AIproxy)
   - `PINECONE_API_KEY` (for Pinecone)
   - `API_KEY` (same as AIPROXY_TOKEN)

## 🏗️ Railway Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Railway Project                          │
├─────────────────┬─────────────────┬─────────────────────────┤
│   Frontend      │   Backend       │   Environment Variables │
│   Service       │   Service       │   (Shared)              │
│   - HTML/CSS/JS │   - FastAPI     │   - AIPROXY_TOKEN       │
│   - Static      │   - Python      │   - PINECONE_API_KEY    │
│   - Node.js     │   - Uvicorn     │   - API_KEY             │
└─────────────────┴─────────────────┴─────────────────────────┘
```

## 🎯 Step-by-Step Railway Deployment

### Step 1: Prepare Your Repository

1. **Push your code to GitHub** (if not already done)
2. **Make sure these files are in your repo:**
   - `app.py` (FastAPI backend)
   - `requirements.txt`
   - `railway.json` (Backend config)
   - `frontend/index.html` (Frontend)
   - `frontend/package.json` (Frontend config)
   - `railway-frontend.json` (Frontend Railway config)

### Step 2: Create Railway Project

1. **Go to [Railway Dashboard](https://railway.app/dashboard)**
2. **Click "New Project"**
3. **Select "Deploy from GitHub repo"**
4. **Connect your GitHub repository**
5. **Click "Deploy Now"**

### Step 3: Deploy Backend Service

1. **In your Railway project, click "New Service"**
2. **Select "GitHub Repo"**
3. **Choose your repository**
4. **Configure the service:**
   - **Name:** `tds-backend` (or your preferred name)
   - **Root Directory:** `/` (root of repo)
   - **Railway will auto-detect Python and use `railway.json`**

5. **Set Environment Variables:**
   - Go to the service settings
   - Click "Variables" tab
   - Add these variables:
     ```
     AIPROXY_TOKEN=your_aiproxy_token_here
     PINECONE_API_KEY=your_pinecone_api_key_here
     API_KEY=your_aiproxy_token_here
     ```

6. **Deploy:**
   - Railway will automatically build and deploy
   - Wait for deployment to complete (2-3 minutes)
   - Note your backend URL: `https://your-backend-service-name-production.up.railway.app`

### Step 4: Deploy Frontend Service

1. **In the same Railway project, click "New Service" again**
2. **Select "GitHub Repo"**
3. **Choose the same repository**
4. **Configure the service:**
   - **Name:** `tds-frontend` (or your preferred name)
   - **Root Directory:** `frontend`
   - **Railway will auto-detect Node.js and use `railway-frontend.json`**

5. **Deploy:**
   - Railway will automatically build and deploy
   - Wait for deployment to complete (1-2 minutes)
   - Note your frontend URL: `https://your-frontend-service-name-production.up.railway.app`

### Step 5: Connect Frontend to Backend

1. **Go to your frontend service**
2. **Click "Settings" → "Variables"**
3. **Add a new variable:**
   ```
   BACKEND_URL=https://your-backend-service-name-production.up.railway.app
   ```

4. **Update the frontend code:**
   - Go to your frontend service
   - Click "Deployments" → "Latest" → "View Files"
   - Edit `index.html`
   - Find this line:
     ```javascript
     API_BASE_URL = 'https://your-backend-service-name-production.up.railway.app';
     ```
   - Replace with your actual backend URL
   - Save and redeploy

## 🧪 Testing Your Deployment

### Test Backend API:
```bash
curl https://your-backend-service-name-production.up.railway.app/health
```

### Test Frontend:
1. **Visit your frontend Railway URL**
2. **Try asking a question**
3. **Check if the answer appears**

### Test with Promptfoo:
```bash
# Update 15q.yaml with your Railway backend URL
promptfoo eval --config 15q.yaml
```

## 🔧 Configuration Files

### `railway.json` (Backend)
```json
{
  "$schema": "https://railway.app/railway.schema.json",
  "build": {
    "builder": "NIXPACKS"
  },
  "deploy": {
    "startCommand": "uvicorn app:app --host 0.0.0.0 --port $PORT",
    "healthcheckPath": "/health",
    "healthcheckTimeout": 100,
    "restartPolicyType": "ON_FAILURE",
    "restartPolicyMaxRetries": 10
  }
}
```

### `railway-frontend.json` (Frontend)
```json
{
  "$schema": "https://railway.app/railway.schema.json",
  "build": {
    "builder": "NIXPACKS"
  },
  "deploy": {
    "startCommand": "npx serve -s . -l $PORT",
    "healthcheckPath": "/",
    "healthcheckTimeout": 100,
    "restartPolicyType": "ON_FAILURE",
    "restartPolicyMaxRetries": 10
  }
}
```

## 🌟 Railway Advantages

### ✅ **All-in-One Platform:**
- Backend and frontend in one project
- Shared environment variables
- Easy service communication
- Built-in monitoring

### ✅ **Free Tier Benefits:**
- **$5 credit/month** (more than enough for this project)
- **Always-on services** (no sleeping)
- **Global CDN**
- **Automatic HTTPS**
- **Custom domains**

### ✅ **Developer Experience:**
- **GitHub integration** (auto-deploy on push)
- **Real-time logs**
- **Easy environment variable management**
- **One-click rollbacks**

## 📊 Railway Free Tier Limits

- **$5 credit/month** (approximately 500 hours of runtime)
- **Always-on services** (no cold starts)
- **Unlimited deployments**
- **Global edge network**

## 💰 Cost Breakdown

- **Railway Backend:** ~$2-3/month (within free tier)
- **Railway Frontend:** ~$1-2/month (within free tier)
- **Pinecone Database:** $0/month (free tier)
- **AIproxy API:** $0/month (free tier)
- **Total:** $0-5/month (within free tier) 🎉

Your complete RAG Query system is now ready for Railway deployment! 