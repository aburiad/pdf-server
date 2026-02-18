# Deployment Guide - Render

This guide will help you deploy the PDF Server to Render.

## Overview

Your setup:
- **Frontend**: https://qgen-mu.vercel.app
- **Backend API**: Will be deployed on Render (https://pdf-server-xxxx.onrender.com)

## Quick Start

### Option 1: Automatic Deployment with render.yaml (Recommended)

1. **Push your code to GitHub**
   ```bash
   git add .
   git commit -m "Setup for Render deployment"
   git push
   ```

2. **Go to Render Dashboard** (https://render.com)
   - Sign in with GitHub

3. **Create Web Service**
   - Click "New +" → "Web Service"
   - Select your GitHub repository
   - Render will auto-detect the `render.yaml` configuration

4. **That's it!**
   - Render will automatically build and deploy
   - Your backend URL will appear in the dashboard

### Option 2: Manual Setup on Render

1. **Connect Repository**
   - Go to render.com → "New Web Service"
   - Connect your GitHub account and select this repository

2. **Configure Service**
   - **Name**: pdf-server
   - **Environment**: Node
   - **Region**: Choose closest to you
   - **Branch**: main (or your default branch)
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Plan**: Free (or upgrade as needed)

3. **Set Environment Variables**
   - Click "Advanced"
   - Add these environment variables:
     ```
     FRONTEND_URL=https://qgen-mu.vercel.app
     NODE_ENV=production
     PUPPETEER_HEADLESS=true
     PUPPETEER_ARGS=--no-sandbox
     ```

4. **Deploy**
   - Click "Create Web Service"
   - Wait for build to complete (3-5 minutes)
   - Copy your service URL (e.g., `https://pdf-server-abc123.onrender.com`)

## Update Frontend

Once deployment is complete:

1. **Get Your Backend URL**
   - From Render dashboard, copy the service URL
   - Example: `https://pdf-server-abc123.onrender.com`

2. **Update Frontend API Configuration**
   ```javascript
   // In your frontend code
   const API_BASE_URL = 'https://pdf-server-abc123.onrender.com/api/pdf';
   
   // Or if using environment variables
   const API_BASE_URL = process.env.REACT_APP_PDF_API_URL;
   ```

3. **Deploy Frontend**
   - Push changes to your frontend repository
   - Vercel will auto-deploy

## Verify Deployment

Test your backend directly:

```bash
# Health check
curl https://pdf-server-abc123.onrender.com/

# You should get a JSON response with API endpoints
```

## Troubleshooting

### Service keeps crashing
- Check Render logs: Dashboard → Logs tab
- Most common: Puppeteer can't run without `--no-sandbox`
- Verify `PUPPETEER_ARGS=--no-sandbox` is set

### CORS errors in frontend
- Check that `FRONTEND_URL` exactly matches your frontend URL
- No trailing slash needed: `https://qgen-mu.vercel.app`
- Both http/https must match

### Slow PDF generation
- Puppet takes time to render PDFs (5-15 seconds is normal)
- For high volume, upgrade from Free plan to Starter plan

### Changes not deploying
- Render auto-deploys on push to your branch
- Check "Auto-Deploy" is enabled in Render settings
- Force redeploy: Dashboard → "Deploy" → "Trigger Deploy"

## Updating Backend

After deployment, to push updates:

1. Make changes locally
2. Git commit and push: `git push`
3. Render automatically redeploys (takes 2-3 minutes)
4. No manual action needed!

## Environment Variables Summary

| Variable | Value | Purpose |
|----------|-------|---------|
| `FRONTEND_URL` | `https://qgen-mu.vercel.app` | Allow CORS from this domain |
| `NODE_ENV` | `production` | Optimize for production |
| `PORT` | (auto-assigned by Render) | Server port |
| `PUPPETEER_HEADLESS` | `true` | Run browser in headless mode |
| `PUPPETEER_ARGS` | `--no-sandbox` | Disable sandbox (required on Linux) |

## Cost & Limits (Render Free Plan)

- ✅ Free tier available
- ⏱ Services sleep after 15 minutes of inactivity (cold start ~30s)
- 📊 Limited resources
- 💾 Sufficient for development/testing

**For Production**: Consider upgrading to Starter plan ($7/month) for:
- Always-on service (no sleeping)
- Better resources
- More reliable performance

---

Need help? Check:
- [Render Documentation](https://render.com/docs)
- [Puppeteer Issues](https://github.com/puppeteer/puppeteer/issues)
- Project README.md for API documentation
