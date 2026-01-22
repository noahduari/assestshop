# 🚀 Deployment Guide

## Vercel Deployment (Recommended)

### Step 1: Prepare Your Repository
```bash
# Initialize Git (if not already done)
git init
git add .
git commit -m "Initial commit: QuickShop Marketplace"

# Create GitHub repository
# Go to github.com → New repository → "quickshop-marketplace"
# Copy the repository URL
```

### Step 2: Push to GitHub
```bash
git remote add origin https://github.com/YOUR_USERNAME/quickshop-marketplace.git
git push -u origin main
```

### Step 3: Deploy to Vercel
1. **Go to** [vercel.com](https://vercel.com)
2. **Sign in** with GitHub
3. **Click** "New Project"
4. **Import** your GitHub repository
5. **Configure**:
   - **Framework Preset**: Other
   - **Root Directory**: `./` (leave default)
   - **Build Command**: Leave empty
   - **Output Directory**: `./` (leave default)
6. **Click** "Deploy"

### Step 4: Update Firebase Authorized Domains
After deployment, add your Vercel domain to Firebase:
1. **Firebase Console** → **Authentication** → **Sign-in method**
2. **Authorized domains** → **Add domain**
3. **Add**: `your-project.vercel.app`

## Firebase Hosting (Alternative)

### Step 1: Install Firebase CLI
```bash
npm install -g firebase-tools
firebase login
```

### Step 2: Initialize Firebase Hosting
```bash
firebase init hosting
# Select your existing Firebase project
# Choose "index.html" as your public directory
```

### Step 3: Deploy
```bash
firebase deploy
```

## Post-Deployment Checklist

- ✅ **Test authentication** (Google & Email/Password)
- ✅ **Test shopping cart** functionality
- ✅ **Check responsive design** on mobile
- ✅ **Verify Firebase Firestore** is working
- ✅ **Test contact form** (if implemented)
- ✅ **Update social media links** with your domain

## Custom Domain (Optional)

### With Vercel:
1. **Vercel Dashboard** → Your project → **Settings** → **Domains**
2. **Add** your custom domain
3. **Update DNS** records as instructed

### Update Firebase:
- Add your custom domain to **Authorized domains**
- Update any hardcoded URLs in your code

## Environment Variables

If you need API keys or sensitive data:
- **Vercel**: Project Settings → Environment Variables
- **Firebase**: Use Firebase config (already included)

## Performance Optimization

- ✅ **Enable compression** (Vercel does this automatically)
- ✅ **Optimize images** (use WebP format)
- ✅ **Minify HTML/CSS/JS** (consider adding build step)
- ✅ **Enable CDN** (Vercel provides this)

## Monitoring

- **Vercel Analytics**: Built-in analytics
- **Firebase Analytics**: Already configured
- **Error tracking**: Check browser console and Vercel logs

Your marketplace is now ready for production! 🎉