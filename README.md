# 🥞 Milky Amsterdam — Full-Stack Website

A production-ready website for **Milky Amsterdam**, a modern crêpe & pancake shop at Barndesteeg 13H, Amsterdam.

## ✨ Features

- **Homepage** with hero, popular items, how-it-works, reviews, location & hours
- **Full digital menu** with category filters and dietary tags
- **Step-by-step crêpe builder** with live price updates
- **Shopping cart** with localStorage persistence
- **Checkout** with pickup time slots, dine-in & delivery options
- **Stripe payments** (cards, iDEAL, Apple Pay) + pay-at-counter
- **Email confirmations** via Nodemailer
- **Order status tracking** with live updates
- **Optional reservations system** (admin-toggled)
- **Full admin dashboard**: orders, menu, toppings, hours, settings
- **Protected admin routes** with NextAuth credentials login
- **Fully responsive** mobile-first design
- **SEO optimized** with metadata on every page

## 🛠 Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 14, TypeScript, Tailwind CSS |
| State | Zustand (cart), React Hook Form |
| Database | PostgreSQL + Prisma ORM |
| Auth | NextAuth v5 (credentials) |
| Payments | Stripe Checkout |
| Email | Nodemailer (SMTP) |
| Animations | Framer Motion |
| Fonts | Playfair Display + Nunito |

## 📁 Project Structure

```
milky-amsterdam/
├── prisma/
│   ├── schema.prisma          # Full DB schema
│   └── seed.ts               # Realistic seed data
├── src/
│   ├── app/
│   │   ├── (public)/         # Public-facing pages
│   │   │   ├── page.tsx      # Homepage
│   │   │   ├── menu/         # Menu + product detail
│   │   │   ├── cart/         # Shopping cart
│   │   │   ├── checkout/     # Checkout flow
│   │   │   ├── order-confirmation/
│   │   │   ├── track-order/
│   │   │   ├── reservations/
│   │   │   ├── about/
│   │   │   ├── contact/
│   │   │   ├── privacy-policy/
│   │   │   └── terms/
│   │   ├── admin/            # Admin dashboard
│   │   │   ├── page.tsx      # Dashboard overview
│   │   │   ├── orders/       # Order management
│   │   │   ├── menu/         # Menu management
│   │   │   ├── toppings/     # Toppings management
│   │   │   ├── reservations/ # Reservations
│   │   │   ├── opening-hours/
│   │   │   ├── settings/
│   │   │   └── login/
│   │   └── api/              # API routes
│   │       ├── auth/         # NextAuth handler
│   │       ├── orders/       # Order creation + tracking
│   │       ├── reservations/ # Reservation booking
│   │       ├── stripe/       # Webhook handler
│   │       └── admin/        # Protected admin APIs
│   ├── components/
│   │   ├── layout/           # Navbar, Footer
│   │   ├── home/             # Hero, PopularItems, HowItWorks, etc.
│   │   └── admin/            # AdminSidebar
│   ├── lib/
│   │   ├── prisma.ts         # Prisma client singleton
│   │   ├── auth.ts           # NextAuth config
│   │   ├── email.ts          # Nodemailer helpers
│   │   └── utils.ts          # Utilities, formatPrice, etc.
│   ├── store/
│   │   └── cart.ts           # Zustand cart store
│   ├── types/
│   │   ├── index.ts          # Shared types
│   │   └── next-auth.d.ts    # Auth session types
│   └── middleware.ts         # Admin route protection
├── .env.example
├── package.json
├── tailwind.config.ts
└── next.config.js
```

## 🚀 Quick Start (Local Development)

### 1. Clone and install

```bash
git clone <your-repo>
cd milky-amsterdam
npm install
```

### 2. Set up environment variables

```bash
cp .env.example .env
```

Edit `.env` with your values (see below).

### 3. Set up the database

```bash
# Start PostgreSQL (or use a cloud provider)
# Then run:
npx prisma migrate dev --name init
npm run db:seed
```

### 4. Generate Prisma client

```bash
npm run db:generate
```

### 5. Start development server

```bash
npm run dev
```

Visit:
- **Public site**: http://localhost:3000
- **Admin panel**: http://localhost:3000/admin
  - Email: `admin@milkyamsterdam.nl`
  - Password: `admin123`

---

## ⚙️ Environment Variables

Create `.env` based on `.env.example`:

```env
# ---- DATABASE ----
DATABASE_URL="postgresql://user:password@localhost:5432/milky_amsterdam"

# ---- NEXTAUTH ----
NEXTAUTH_SECRET="generate-with: openssl rand -base64 32"
NEXTAUTH_URL="http://localhost:3000"

# ---- STRIPE ----
STRIPE_SECRET_KEY="sk_test_..."
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY="pk_test_..."
STRIPE_WEBHOOK_SECRET="whsec_..."

# ---- EMAIL (Gmail SMTP example) ----
SMTP_HOST="smtp.gmail.com"
SMTP_PORT="587"
SMTP_USER="your-gmail@gmail.com"
SMTP_PASS="your-app-password"       # Use Gmail App Password
EMAIL_FROM="Milky Amsterdam <hello@milkyamsterdam.nl>"

# ---- APP ----
NEXT_PUBLIC_APP_URL="http://localhost:3000"
```

### Getting a Gmail App Password

1. Go to Google Account → Security → 2-Step Verification
2. Scroll to **App passwords**
3. Generate for "Mail" → "Other (Milky Amsterdam)"
4. Use that 16-char password as `SMTP_PASS`

---

## 💳 Stripe Setup

1. Create account at [stripe.com](https://stripe.com)
2. Get API keys from Dashboard → Developers → API Keys
3. For webhooks (local): install [Stripe CLI](https://stripe.com/docs/stripe-cli)

```bash
stripe listen --forward-to localhost:3000/api/stripe/webhook
```

4. Copy the webhook secret to `STRIPE_WEBHOOK_SECRET`

Supported payment methods (Netherlands): Mastercard, Visa, iDEAL, Apple Pay, Google Pay

---

## 🗄️ Database: PostgreSQL Setup Options

### Option A: Local (macOS with Homebrew)
```bash
brew install postgresql@16
brew services start postgresql@16
createdb milky_amsterdam
DATABASE_URL="postgresql://localhost:5432/milky_amsterdam"
```

### Option B: Docker
```bash
docker run -d \
  --name milky-postgres \
  -e POSTGRES_DB=milky_amsterdam \
  -e POSTGRES_PASSWORD=password \
  -p 5432:5432 \
  postgres:16
DATABASE_URL="postgresql://postgres:password@localhost:5432/milky_amsterdam"
```

### Option C: Neon (free cloud, recommended for Vercel)
1. Sign up at [neon.tech](https://neon.tech)
2. Create a project → Get connection string
3. Use as `DATABASE_URL`

---

## 🌐 Deployment (Vercel + Neon PostgreSQL)

### 1. Push to GitHub

```bash
git init && git add . && git commit -m "initial"
git remote add origin https://github.com/yourusername/milky-amsterdam.git
git push -u origin main
```

### 2. Deploy on Vercel

1. Go to [vercel.com](https://vercel.com) → New Project → Import from GitHub
2. Add all environment variables in Vercel Dashboard → Settings → Environment Variables
3. For `DATABASE_URL`: use your Neon connection string (add `?sslmode=require`)
4. Deploy!

### 3. Run migrations on production

```bash
# After first deployment:
npx prisma migrate deploy
npm run db:seed
```

### 4. Set up Stripe webhooks for production

In Stripe Dashboard → Webhooks → Add endpoint:
- URL: `https://your-domain.vercel.app/api/stripe/webhook`
- Events: `checkout.session.completed`, `checkout.session.expired`

---

## 📱 Admin Dashboard Guide

| Section | URL | What you can do |
|---------|-----|-----------------|
| Overview | /admin | Stats, active orders, reservations |
| Orders | /admin/orders | View, filter, update status, print tickets |
| Menu | /admin/menu | Add/edit/delete products, toggle availability |
| Toppings | /admin/toppings | Manage topping options & prices |
| Reservations | /admin/reservations | Enable/disable system, manage bookings |
| Opening Hours | /admin/opening-hours | Set weekly hours per day |
| Settings | /admin/settings | All shop config, delivery, fees, VAT |

### First login credentials (change after setup!)
- Email: `admin@milkyamsterdam.nl`
- Password: `admin123`

---

## 🎨 Design System

| Color | Tailwind Class | Usage |
|-------|---------------|-------|
| Cream | `cream-50/100/200` | Backgrounds |
| Chocolate | `choco-600/700/800` | Text, buttons |
| Caramel | `caramel-400/500` | Accents |
| Blush pink | `blush-300/400/500` | Tags, highlights |

Fonts:
- **Display**: Playfair Display (headings)
- **Body**: Nunito (body text)

---

## 🔒 Security Notes

- Admin routes protected by NextAuth session + middleware
- All admin API routes verify session role = ADMIN
- Stripe webhook signature verified
- Input validated with Zod on all API routes
- Passwords hashed with bcrypt (12 rounds)
- SQL injection prevented by Prisma ORM

---

## 📧 Email Flows

| Trigger | Recipient | Content |
|---------|-----------|---------|
| Order placed | Customer | Order confirmation + items |
| Status update | Customer | New status notification |
| Reservation request | Customer | Booking details + cancel link |

---

## 🧪 Seed Data

The seed includes:
- **10 products**: Signature crêpes, pancakes, savory, coffee, drinks
- **22 toppings**: Spreads, fruits, crunch extras, sauces
- **Opening hours**: Mon–Sun 09:00–22:00
- **1 admin user**: admin@milkyamsterdam.nl / admin123
- **All default settings**

```bash
npm run db:seed
```

---

## 📞 Support

- Address: Barndesteeg 13H, 1012 BV Amsterdam
- Email: hello@milkyamsterdam.nl
- Maps: https://maps.app.goo.gl/FvHzLPAeVPuPBYqm6
