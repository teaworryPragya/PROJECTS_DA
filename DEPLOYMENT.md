# Deployment Guide

This guide covers deploying the Blinkit Analytics Dashboard to production using Vercel (frontend) and Render (backend).

## Prerequisites

- GitHub account
- Vercel account (free tier available)
- Render account (free tier available)
- Git repository with your code

## Backend Deployment (Render)

### 1. Prepare Your Repository

Ensure your code is pushed to a GitHub repository with the following structure:
```
blinkit-analytics-dashboard/
├── backend/
│   ├── package.json
│   ├── server.js
│   ├── render.yaml
│   └── src/
└── frontend/
    ├── package.json
    ├── vercel.json
    └── src/
```

### 2. Deploy to Render

1. **Connect Repository:**
   - Go to [Render Dashboard](https://dashboard.render.com)
   - Click "New +" → "Web Service"
   - Connect your GitHub repository
   - Select the repository containing your project

2. **Configure Service:**
   - **Name:** `blinkit-analytics-api`
   - **Environment:** `Node`
   - **Region:** Choose closest to your users
   - **Branch:** `main`
   - **Root Directory:** `backend`
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`

3. **Environment Variables:**
   Set the following in Render dashboard:
   ```
   NODE_ENV=production
   PORT=10000
   FRONTEND_URL=https://your-frontend-domain.vercel.app
   API_KEY=your-secure-api-key
   JWT_SECRET=your-jwt-secret
   RATE_LIMIT_WINDOW_MS=900000
   RATE_LIMIT_MAX_REQUESTS=100
   ```

4. **Deploy:**
   - Click "Create Web Service"
   - Wait for deployment to complete
   - Note your backend URL: `https://your-service.onrender.com`

### 3. Health Check

Your backend includes a health check endpoint at `/health`. Render will use this to monitor service health.

## Frontend Deployment (Vercel)

### 1. Prepare Environment Variables

Create a `.env.production` file in the frontend directory:
```env
REACT_APP_API_URL=https://your-backend-service.onrender.com
REACT_APP_NAME=Blinkit Analytics Dashboard
REACT_APP_VERSION=1.0.0
REACT_APP_ENABLE_ANALYTICS=true
REACT_APP_ENABLE_EXPORT=true
```

### 2. Deploy to Vercel

#### Option A: Vercel CLI
```bash
# Install Vercel CLI
npm i -g vercel

# Navigate to frontend directory
cd frontend

# Deploy
vercel

# Follow prompts:
# - Link to existing project? No
# - Project name: blinkit-analytics-dashboard
# - Directory: ./
# - Override settings? No
```

#### Option B: Vercel Dashboard
1. Go to [Vercel Dashboard](https://vercel.com/dashboard)
2. Click "New Project"
3. Import your GitHub repository
4. Configure:
   - **Framework Preset:** Create React App
   - **Root Directory:** `frontend`
   - **Build Command:** `npm run build`
   - **Output Directory:** `build`

### 3. Environment Variables in Vercel

In Vercel dashboard:
1. Go to Project Settings → Environment Variables
2. Add:
   ```
   REACT_APP_API_URL = https://your-backend-service.onrender.com
   REACT_APP_NAME = Blinkit Analytics Dashboard
   REACT_APP_VERSION = 1.0.0
   REACT_APP_ENABLE_ANALYTICS = true
   REACT_APP_ENABLE_EXPORT = true
   ```

### 4. Custom Domain (Optional)

1. In Vercel dashboard, go to Project Settings → Domains
2. Add your custom domain
3. Configure DNS records as instructed

## Post-Deployment Configuration

### 1. Update CORS Settings

Ensure your backend's CORS configuration includes your frontend domain:
```javascript
// In server.js
const corsOptions = {
  origin: [
    'http://localhost:3000',
    'https://your-frontend-domain.vercel.app',
    'https://your-custom-domain.com' // if using custom domain
  ],
  credentials: true
};
```

### 2. Update Frontend API Configuration

Ensure axios is configured to use the production API URL:
```javascript
// In src/context/AuthContext.js or API configuration
const API_BASE_URL = process.env.REACT_APP_API_URL || 'http://localhost:5000';
```

## Monitoring and Maintenance

### Backend Monitoring (Render)
- **Logs:** Available in Render dashboard
- **Metrics:** CPU, Memory, Response times
- **Health Checks:** Automatic via `/health` endpoint
- **Scaling:** Manual scaling available on paid plans

### Frontend Monitoring (Vercel)
- **Analytics:** Built-in Vercel Analytics
- **Performance:** Core Web Vitals tracking
- **Deployments:** Automatic deployments on git push
- **Preview Deployments:** For pull requests

## Troubleshooting

### Common Issues

1. **CORS Errors:**
   - Ensure frontend URL is in backend CORS whitelist
   - Check environment variables are set correctly

2. **API Connection Failed:**
   - Verify `REACT_APP_API_URL` is correct
   - Check backend service is running
   - Verify network connectivity

3. **Build Failures:**
   - Check all dependencies are in package.json
   - Ensure Node.js version compatibility
   - Review build logs for specific errors

4. **Environment Variables Not Working:**
   - Ensure variables start with `REACT_APP_` for frontend
   - Redeploy after adding new variables
   - Check variable names for typos

### Performance Optimization

1. **Backend:**
   - Enable compression (already configured)
   - Use CDN for static assets
   - Implement caching strategies
   - Monitor database queries

2. **Frontend:**
   - Code splitting with React.lazy()
   - Image optimization
   - Bundle analysis with `npm run analyze`
   - Enable Vercel Edge Functions if needed

## Security Considerations

1. **API Security:**
   - Rate limiting (configured)
   - Input validation
   - HTTPS only
   - Secure headers (helmet.js configured)

2. **Frontend Security:**
   - Environment variables for sensitive config
   - Content Security Policy
   - HTTPS enforcement
   - XSS protection

## Backup and Recovery

1. **Code:** Stored in Git repository
2. **Configuration:** Document all environment variables
3. **Data:** Mock data is generated, no persistent storage
4. **Deployment:** Both platforms support rollback to previous deployments

## Cost Optimization

### Render (Backend)
- **Free Tier:** 750 hours/month, sleeps after 15 min inactivity
- **Paid Plans:** Start at $7/month for always-on service
- **Optimization:** Use starter plan for development, upgrade for production

### Vercel (Frontend)
- **Free Tier:** 100GB bandwidth, unlimited deployments
- **Paid Plans:** Start at $20/month for teams
- **Optimization:** Free tier sufficient for most use cases

## Next Steps

1. Set up monitoring and alerting
2. Configure custom domains
3. Implement CI/CD pipelines
4. Set up staging environments
5. Add performance monitoring
6. Implement error tracking (Sentry, LogRocket)

For support, refer to:
- [Render Documentation](https://render.com/docs)
- [Vercel Documentation](https://vercel.com/docs)
- Project README.md for development setup
