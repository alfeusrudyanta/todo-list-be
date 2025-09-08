# 🚀 Deploy to Vercel Instructions

## Prerequisites
1. Install Vercel CLI: `npm i -g vercel`
2. Login to Vercel: `vercel login`

## Step-by-step Deployment

### 1. **Initial Setup & First Deploy**
```bash
# Make sure you're in the project directory
cd /path/to/your/project

# Run vercel command (first time)
vercel

# This will prompt you with questions:
# ? Set up and deploy "~/be_todolist"? [Y/n] y
# ? Which scope do you want to deploy to? [Use arrows] Your Account
# ? Link to existing project? [y/N] n
# ? What's your project's name? be-todolist
# ? In which directory is your code located? ./
```

### 2. **Configure Build Settings**
Vercel will automatically detect your Node.js project and use these settings:
- **Build Command**: `npm run vercel-build` (which runs `npm run build`)
- **Output Directory**: `dist`
- **Install Command**: `npm install`

### 3. **Environment Variables Setup**
⚠️ **CRITICAL**: Set these in Vercel Dashboard before deployment works

Go to: [Vercel Dashboard](https://vercel.com/dashboard) → Your Project → Settings → Environment Variables

**Required Variables:**
```
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=your-aws-access-key-here
AWS_SECRET_ACCESS_KEY=your-secret-access-key-here
CLERK_SECRET_KEY=your-clerk-secret-key-here
API_KEY=0ICVyrNhPL56Oss58qv-_y42PhSQvYcPm6Vz26j4bNw
```

**Set for All Environments**: Production, Preview, Development

### 4. **Deploy Commands**

#### **Preview Deployment** (for testing):
```bash
vercel
```
- Creates a unique preview URL
- Perfect for testing before production

#### **Production Deployment**:
```bash
vercel --prod
```
- Deploys to your production domain
- Only do this after testing preview deployment

#### **Force Rebuild** (if needed):
```bash
vercel --force --prod
```

#### **Deploy with Logs** (for debugging):
```bash
vercel --logs --prod
```

### 5. **Continuous Deployment via Git**
After initial setup, you can enable auto-deploy:

1. Push code to GitHub:
```bash
git add .
git commit -m "Deploy to Vercel"
git push origin main
```

2. Vercel will auto-deploy on every push to main branch

## ⚙️ Vercel Configuration Explained

### `vercel.json` Configuration:
```json
{
  "version": 2,
  "builds": [
    {
      "src": "dist/app.js",           // Entry point after build
      "use": "@vercel/node"           // Node.js runtime
    }
  ],
  "routes": [
    {
      "src": "/(.*)",                 // Catch all routes
      "dest": "dist/app.js"           // Route to Express app
    }
  ],
  "functions": {
    "dist/app.js": {
      "maxDuration": 30               // 30 second timeout
    }
  }
}
```

## 🌐 Your API Endpoints

After deployment, your API will be available at:
- **Production**: `https://be-todolist.vercel.app` (or your custom domain)
- **Preview**: `https://be-todolist-xyz123.vercel.app` (unique per deployment)

### Available Routes:
- **API Docs**: `https://your-domain.vercel.app/api-docs`
- **Todos API**: `https://your-domain.vercel.app/todos`

## 🔧 Advanced Options

### Deploy with Custom Target:
```bash
vercel --target=production    # Explicit production
vercel --target=preview      # Explicit preview
```

### Deploy with Custom Environment:
```bash
vercel --env KEY1=value1 --env KEY2=value2
```

### Deploy Specific Directory:
```bash
vercel --cwd /path/to/project
```

## 🐛 Troubleshooting

### 1. **Build Failures**
```bash
# Test build locally first
npm run build

# Check for TypeScript errors
npx tsc --noEmit
```

### 2. **Runtime Errors**
- Check Vercel Function logs in dashboard
- Verify all environment variables are set
- Test API endpoints with Postman/curl

### 3. **Environment Variable Issues**
```bash
# Check if vars are properly set
vercel env ls
```

### 4. **Port Issues**
Your app automatically uses `process.env.PORT` (Vercel provides this)

### 5. **CORS Issues**
The app already includes CORS for all origins:
```javascript
app.use(cors());
```

## 📊 Monitoring & Logs

### View Deployment Logs:
1. Go to Vercel Dashboard
2. Select your project
3. Click on a deployment
4. View "Build Logs" and "Function Logs"

### Real-time Function Logs:
```bash
vercel logs --follow
```

## 🚀 Production Checklist

Before going production:
- [ ] ✅ Environment variables configured
- [ ] ✅ AWS DynamoDB tables created
- [ ] ✅ Clerk authentication setup
- [ ] ✅ API endpoints tested
- [ ] ✅ Swagger documentation accessible
- [ ] ✅ CORS properly configured
- [ ] ✅ Error handling implemented

## 📝 Useful Commands Summary

```bash
# Initial setup
vercel login
vercel

# Development
npm run dev                    # Local development
npm run build                  # Test build locally

# Deployment
vercel                         # Preview deployment
vercel --prod                  # Production deployment  
vercel --force --prod          # Force rebuild & deploy
vercel --logs --prod           # Deploy with build logs

# Management
vercel ls                      # List deployments
vercel rm [deployment-url]     # Remove deployment
vercel domains                 # Manage domains
vercel env ls                  # List environment variables
```

## 🔗 Quick Links
- [Vercel Dashboard](https://vercel.com/dashboard)
- [Vercel Documentation](https://vercel.com/docs)
- [Your Project Settings](https://vercel.com/dashboard) → Select Project → Settings
