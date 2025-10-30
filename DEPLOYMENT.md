# Deployment Guide

## Quick Deployment to Vercel

### Prerequisites
- GitHub account
- Vercel account (free tier works)
- PostgreSQL database (Vercel Postgres recommended)
- Stripe account

### Step 1: Push to GitHub

```bash
# Create a new repository on GitHub
# Then push your code
git remote add origin https://github.com/YOUR_USERNAME/music-crowdfunding.git
git push -u origin main
```

### Step 2: Deploy to Vercel

1. Visit [vercel.com](https://vercel.com)
2. Click "New Project"
3. Import your GitHub repository
4. Configure project:
   - Framework Preset: **Next.js**
   - Build Command: `prisma generate && next build`
   - Output Directory: `.next`

### Step 3: Set Up Database

**Option A: Vercel Postgres (Recommended)**

1. In Vercel dashboard, go to Storage tab
2. Create a new Postgres database
3. Copy the `DATABASE_URL` connection string
4. Add to environment variables

**Option B: External PostgreSQL**

Use any PostgreSQL provider:
- [Neon](https://neon.tech/) (generous free tier)
- [Supabase](https://supabase.com/) (free tier with auth included)
- [Railway](https://railway.app/)
- [Heroku Postgres](https://www.heroku.com/postgres)

### Step 4: Configure Environment Variables

In Vercel dashboard → Settings → Environment Variables, add:

```bash
# Database
DATABASE_URL="postgresql://user:password@host:5432/database"

# NextAuth
NEXTAUTH_URL="https://your-app.vercel.app"
NEXTAUTH_SECRET="generate-with-command-below"

# Stripe
STRIPE_SECRET_KEY="sk_live_..."
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY="pk_live_..."
STRIPE_WEBHOOK_SECRET="whsec_..."

# App
NEXT_PUBLIC_APP_URL="https://your-app.vercel.app"
```

**Generate NEXTAUTH_SECRET:**
```bash
openssl rand -base64 32
```

### Step 5: Set Up Stripe

1. **Get API Keys**
   - Login to [Stripe Dashboard](https://dashboard.stripe.com/)
   - Get your API keys from Developers → API keys
   - Use test keys for staging, live keys for production

2. **Configure Webhooks**
   - Go to Developers → Webhooks
   - Add endpoint: `https://your-app.vercel.app/api/webhooks/stripe`
   - Select events:
     - `payment_intent.succeeded`
     - `payment_intent.payment_failed`
     - `checkout.session.completed`
   - Copy webhook signing secret to `STRIPE_WEBHOOK_SECRET`

3. **Enable Stripe Connect** (for marketplace)
   - Go to Connect settings
   - Complete business profile
   - Enable Connect onboarding

### Step 6: Initialize Database

After first deployment:

```bash
# Install Vercel CLI
npm i -g vercel

# Link to your project
vercel link

# Run Prisma migrations
vercel env pull .env.local
npx prisma migrate deploy
```

Or use Prisma Studio remotely:
```bash
npx prisma studio
```

### Step 7: Seed Database (Optional)

Create sample data for testing:

```bash
npm run db:seed
```

## Environment-Specific Configuration

### Development
```bash
DATABASE_URL="postgresql://localhost:5432/music_crowdfunding_dev"
NEXTAUTH_URL="http://localhost:3000"
STRIPE_SECRET_KEY="sk_test_..."
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY="pk_test_..."
```

### Staging
```bash
DATABASE_URL="postgresql://staging-db-url"
NEXTAUTH_URL="https://staging.your-app.vercel.app"
STRIPE_SECRET_KEY="sk_test_..."  # Use test keys
```

### Production
```bash
DATABASE_URL="postgresql://production-db-url"
NEXTAUTH_URL="https://your-app.vercel.app"
STRIPE_SECRET_KEY="sk_live_..."  # Use live keys
```

## Custom Domain

1. Go to Vercel → Settings → Domains
2. Add your custom domain
3. Configure DNS records:
   - Type: **CNAME**
   - Name: **@** (or subdomain)
   - Value: **cname.vercel-dns.com**
4. Update `NEXTAUTH_URL` environment variable

## Post-Deployment Checklist

- [ ] Database connected and migrated
- [ ] All environment variables set
- [ ] Stripe webhooks configured
- [ ] Test user registration
- [ ] Test campaign creation
- [ ] Test payment flow
- [ ] Verify email sending (if configured)
- [ ] Check error logging
- [ ] Set up monitoring (Vercel Analytics)
- [ ] Configure custom domain (optional)

## Monitoring & Maintenance

### Vercel Analytics
Enable in Vercel dashboard → Analytics tab

### Error Tracking
Consider adding:
- [Sentry](https://sentry.io/) for error tracking
- [LogRocket](https://logrocket.com/) for session replay

### Performance Monitoring
- Use Vercel Speed Insights
- Monitor Core Web Vitals
- Set up Lighthouse CI

## Troubleshooting

### Build Fails with "Prisma Client Not Generated"
**Solution:** Ensure build command includes `prisma generate`:
```bash
prisma generate && next build
```

### Database Connection Issues
**Solution:**
1. Check `DATABASE_URL` format
2. Verify database is accessible from Vercel
3. Check SSL requirements (add `?sslmode=require` if needed)

### Stripe Webhook Not Working
**Solution:**
1. Verify webhook URL is correct
2. Check endpoint is accessible (return 200 OK)
3. Verify webhook secret matches
4. Check Stripe dashboard for failed deliveries

### NextAuth Session Issues
**Solution:**
1. Verify `NEXTAUTH_SECRET` is set
2. Check `NEXTAUTH_URL` matches your domain
3. Clear cookies and try again

## Scaling Considerations

### Database
- Start with Vercel Postgres Hobby tier (60 hours compute/month)
- Upgrade to Pro for production workloads
- Consider read replicas for heavy traffic
- Implement connection pooling (PgBouncer)

### Caching
- Use Vercel Edge Caching for static content
- Implement Redis for session storage
- Cache campaign data with revalidation

### File Storage
- Use Vercel Blob for user uploads
- Consider Cloudinary for image optimization
- S3 for large file storage

### Performance
- Enable Next.js image optimization
- Use ISR (Incremental Static Regeneration) for campaign pages
- Implement CDN for static assets

## Security Best Practices

- [ ] Enable HTTPS (automatic with Vercel)
- [ ] Set secure cookie flags in NextAuth
- [ ] Implement rate limiting on API routes
- [ ] Validate all user inputs with Zod
- [ ] Use environment variables for secrets
- [ ] Enable CORS only for trusted domains
- [ ] Implement CSP headers
- [ ] Regular dependency updates
- [ ] Enable Vercel firewall rules

## Backup Strategy

### Database Backups
- Vercel Postgres: Automatic daily backups
- External: Configure automated backups
- Test restore process monthly

### Code Backups
- GitHub provides version control
- Consider GitHub Actions for automated backups

## Support

For deployment issues:
- [Vercel Documentation](https://vercel.com/docs)
- [Next.js Deployment Docs](https://nextjs.org/docs/deployment)
- [Prisma Deployment Guide](https://www.prisma.io/docs/guides/deployment)

---

**Deployment Region:** Paris (cdg1) for EU compliance and low latency

**Estimated Setup Time:** 15-30 minutes

**Cost Estimate:**
- Vercel Hobby: Free (includes 100GB bandwidth)
- Vercel Postgres: $20/month (Hobby tier)
- Stripe: 1.4% + €0.25 per transaction (EU cards)

Total: ~$20-30/month for small-medium traffic
