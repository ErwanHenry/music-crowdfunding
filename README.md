# Music Crowdfunding Platform

A modern crowdfunding platform specifically designed for musicians to fund their creative projects - albums, tours, music videos, equipment, and more.

## Features

### For Musicians
- 🎵 **Create Campaigns** - Set funding goals for albums, tours, equipment, or videos
- 🎁 **Offer Rewards** - Create tiered rewards for backers (merch, exclusive content, meet & greets)
- 📊 **Track Progress** - Real-time dashboard with funding stats and backer management
- 💰 **Flexible Funding** - Keep what you raise, even if you don't reach your goal
- 📱 **Updates & Engagement** - Post updates and interact with supporters

### For Backers
- 🔍 **Discover Music** - Explore campaigns by genre, project type, or popularity
- 💳 **Secure Payments** - Safe transactions via Stripe
- 🎉 **Exclusive Rewards** - Get unique perks for supporting artists
- 💬 **Community** - Comment and engage with musicians and other backers
- 📧 **Stay Updated** - Receive project updates and milestone notifications

### Platform Features
- 🔐 **Authentication** - Secure login with NextAuth
- 👤 **User Profiles** - Dedicated profiles for musicians and backers
- 🎨 **Modern UI** - Clean, responsive design with Tailwind CSS
- 📈 **Analytics** - Track campaign performance and engagement
- 🌍 **Multi-currency** - Support for EUR, USD, and more
- ⚡ **Fast & Scalable** - Built on Next.js 15 with React 19

## Tech Stack

- **Framework**: Next.js 15 with App Router
- **Language**: TypeScript (strict mode)
- **Database**: PostgreSQL with Prisma ORM
- **Authentication**: NextAuth.js
- **Payments**: Stripe
- **Styling**: Tailwind CSS
- **Forms**: React Hook Form + Zod validation
- **Deployment**: Vercel

## Database Schema

### Core Models

**User**
- Roles: MUSICIAN, BACKER, ADMIN
- Musician profiles with artist info, social links
- Stripe customer integration

**Campaign**
- Status: DRAFT, ACTIVE, FUNDED, CLOSED, CANCELLED
- Funding goals, current amount, backer count
- Rich content: description, story, cover image, video
- Project types: Album, EP, Tour, Video, Equipment

**Reward**
- Tiered rewards for backers
- Limited availability options
- Estimated delivery dates

**Pledge**
- Stripe payment integration
- Status tracking: PENDING, COMPLETED, REFUNDED, FAILED
- Optional anonymous pledges
- Personal messages to musicians

**Update** & **Comment**
- Campaign updates from musicians
- Community engagement through comments

## Getting Started

### Prerequisites

- Node.js 18+
- PostgreSQL database
- Stripe account (for payments)

### Installation

```bash
# Clone the repository
cd music-crowdfunding

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env
# Edit .env with your configuration

# Generate Prisma client
npm run db:generate

# Push database schema
npm run db:push

# Run development server
npm run dev
```

Visit http://localhost:3000

### Environment Variables

Create a `.env` file with:

```bash
# Database
DATABASE_URL="postgresql://user:password@localhost:5432/music_crowdfunding"

# NextAuth
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="generate-with-openssl-rand-base64-32"

# Stripe
STRIPE_SECRET_KEY="sk_test_..."
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY="pk_test_..."
STRIPE_WEBHOOK_SECRET="whsec_..."

# App
NEXT_PUBLIC_APP_URL="http://localhost:3000"
```

## Available Scripts

```bash
# Development
npm run dev              # Start Next.js dev server

# Database
npm run db:generate      # Generate Prisma client
npm run db:push          # Push schema to database
npm run db:studio        # Open Prisma Studio GUI
npm run db:migrate       # Run migrations
npm run db:seed          # Seed database with sample data

# Production
npm run build            # Build for production
npm start                # Start production server
npm run prepare-prod     # Prepare production environment

# Code Quality
npm run lint             # Run ESLint
npm run type-check       # TypeScript type checking
```

## Project Structure

```
music-crowdfunding/
├── prisma/
│   └── schema.prisma          # Database schema
├── src/
│   ├── app/                   # Next.js App Router
│   │   ├── (auth)/           # Auth pages (login, signup)
│   │   ├── campaigns/        # Campaign pages
│   │   ├── dashboard/        # User dashboard
│   │   ├── api/              # API routes
│   │   ├── layout.tsx        # Root layout
│   │   └── page.tsx          # Homepage
│   ├── components/           # React components
│   │   ├── ui/              # Reusable UI components
│   │   ├── campaigns/       # Campaign-specific components
│   │   └── layout/          # Layout components
│   ├── lib/                  # Utilities and configurations
│   │   ├── prisma.ts        # Prisma client
│   │   ├── stripe.ts        # Stripe configuration
│   │   └── auth.ts          # NextAuth configuration
│   └── types/                # TypeScript types
├── public/                   # Static assets
├── .env.example             # Environment variables template
├── package.json
├── tsconfig.json
└── README.md
```

## Key Features Implementation

### Campaign Creation Flow
1. Musician creates account and profile
2. Fills out campaign details (title, description, goal)
3. Adds reward tiers
4. Uploads cover image and optional video
5. Submits for review (optional) or publishes immediately

### Pledge Flow
1. Backer browses campaigns
2. Selects campaign and reward tier
3. Enters pledge amount (minimum: reward tier amount)
4. Completes Stripe payment
5. Receives confirmation and updates

### Payout Flow
1. Campaign ends successfully
2. Funds held in Stripe Connect account
3. Musician requests payout
4. Stripe transfers funds (minus platform fee)
5. Musician receives funds in 2-7 business days

## Payment Processing

Uses **Stripe Connect** for marketplace payments:
- Platform collects payments on behalf of musicians
- Automatic fee calculation (e.g., 5% platform fee + Stripe fees)
- Direct payouts to musician bank accounts
- Refund handling for cancelled campaigns

## Security

- Password hashing with bcrypt
- CSRF protection via NextAuth
- SQL injection prevention via Prisma
- Rate limiting on API routes
- Input validation with Zod schemas
- Secure payment processing via Stripe

## Deployment

### Vercel (Recommended)

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel
```

Configuration:
1. Add environment variables in Vercel dashboard
2. Connect PostgreSQL database (Vercel Postgres recommended)
3. Set up Stripe webhooks with production URL
4. Configure custom domain (optional)

### Environment Setup

**Production Environment Variables:**
- Update `DATABASE_URL` with production database
- Use production Stripe keys
- Set `NEXTAUTH_URL` to production domain
- Generate new `NEXTAUTH_SECRET`

## Roadmap

### Phase 1 (MVP) - Current
- ✅ User authentication
- ✅ Campaign creation and management
- ✅ Pledge system with Stripe
- ✅ Basic rewards system
- ✅ Homepage and campaign listing

### Phase 2 (Coming Soon)
- [ ] Campaign search and filtering
- [ ] Email notifications
- [ ] Social sharing
- [ ] Admin dashboard
- [ ] Campaign categories and tags

### Phase 3 (Future)
- [ ] Mobile app (React Native)
- [ ] Video streaming integration
- [ ] Musician verification badges
- [ ] Advanced analytics
- [ ] Multi-language support
- [ ] Spotify/Apple Music integration

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License.

## Support

For questions or issues:
- Open an issue on GitHub
- Email: support@musicfund.com
- Discord: [Join our community]

## Acknowledgments

- Built with [Next.js](https://nextjs.org/)
- Payments by [Stripe](https://stripe.com/)
- Styled with [Tailwind CSS](https://tailwindcss.com/)
- Database by [Prisma](https://www.prisma.io/)

---

Made with ♥ for the music community
