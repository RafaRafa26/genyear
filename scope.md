# 📋 Detailed Implementation Plan for Genyear

## 🚀 Project Overview

**Genyear** is a micro SaaS that allows users to discover the main events of a specific year in any country, in a fast, visual and fun way.

The goal is to **evoke emotions and nostalgia**, delivering information in a simple and shareable way — perfect for going viral on social networks like TikTok, Instagram and YouTube Shorts.

### Value Proposition

- Search for **events, music, movies, prices and relevant facts** from any country and year
- **Gamified and visual** experience, including an avatar called **Gen**
- Each result can be **downloaded as an image** or **shared on social networks**

### Monetization

**Free Plan**: 5 free searches per month, recent years only (up to 10 years ago), AdSense ads
**Pro Plan**: $7-8/month or $4-5/month annually, unlimited searches, all years, no ads
**Voluntary Support**: "☕ Support Genyear" button with symbolic benefits

### Tech Stack

- **Frontend**: Next.js 15 + TypeScript + TailwindCSS + shadcn/ui
- **Backend/Data**: Route Handlers, Prisma ORM, Supabase (Postgres), local JSON
- **Cache**: Upstash Redis (optional)
- **Auth**: Auth.js (NextAuth)
- **Payments**: Stripe
- **Deploy**: Vercel + Supabase
- **Analytics**: Plausible or PostHog
- **Ads**: Google AdSense
- **Images**: Satori (SVG) + Resvg (PNG)

---

## 🎨 **Phase 1: UI Foundation & Dark Theme Setup** - [ ]

### 1.1 Install Required Dependencies - [x]

```bash
# UI Components & Styling ✓
npm install @radix-ui/react-slot class-variance-authority clsx tailwind-merge
npm install lucide-react @radix-ui/react-icons

# Form Handling ✓
npm install react-hook-form @hookform/resolvers zod

# Database & Auth ✓
npm install prisma @prisma/client
npm install next-auth @auth/prisma-adapter
npm install bcryptjs @types/bcryptjs

# Payments ✓
npm install stripe @stripe/stripe-js

# Image Generation ✓
npm install satori @resvg/resvg-js

# Utils ✓
npm install date-fns
```

### 1.2 Setup shadcn/ui Components - [x]

- ✓ Initialize shadcn/ui with dark theme as default
- ✓ Create custom dark theme color palette for Genyear brand (Gen character green)
- ✓ Install core components: Button, Input, Card, Dialog, Sonner (Toast replacement)

### 1.3 Create Design System - [x]

- ✓ **Typography**: Clean, readable fonts (Geist Sans & Mono)
- ✓ **Colors**: Deep blacks, subtle grays, Gen character green accent (#65a548)
- ✓ **Spacing**: Generous whitespace for minimalist feel
- ✓ **Components**: Rounded corners, subtle shadows, smooth animations
- ✓ **Directory Structure**: Created component folders (gen/, search/, layout/, data/)

---

## 🏗️ **Phase 2: Core Application Structure** - [ ]

### 2.1 Database Schema (Prisma)

```prisma
// User management & subscription tracking
model User {
  id            String    @id @default(cuid())
  email         String    @unique
  name          String?
  searchCount   Int       @default(0)
  isPro         Boolean   @default(false)
  stripeId      String?   @unique
  searches      Search[]
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
}

// Cache search results
model Search {
  id          String   @id @default(cuid())
  userId      String?
  year        Int
  country     String
  data        Json     // Store the complete result
  imageUrl    String?  // Generated share image
  user        User?    @relation(fields: [userId], references: [id])
  createdAt   DateTime @default(now())
}
```

### 2.2 Authentication Setup

- Configure NextAuth with email/password
- Create sign-up/sign-in pages with dark theme
- Implement session management
- Add middleware for protected routes

### 2.3 File Structure

```
src/
├── app/
│   ├── (auth)/
│   │   ├── signin/
│   │   └── signup/
│   ├── api/
│   │   ├── auth/
│   │   ├── search/
│   │   ├── stripe/
│   │   └── generate-image/
│   ├── dashboard/
│   └── search/
├── components/
│   ├── ui/           # shadcn components
│   ├── gen/          # Gen character components
│   ├── search/       # Search-related components
│   └── layout/       # Layout components
├── lib/
│   ├── auth.ts
│   ├── prisma.ts
│   ├── stripe.ts
│   ├── data/         # JSON data files
│   └── utils.ts
└── types/
    └── index.ts
```

---

## 🎯 **Phase 3: Landing Page & Branding** - [ ]

### 3.1 Gen Character Design

- Create simple, friendly avatar using CSS/SVG
- Dark theme variant with glowing effects
- Animated states (thinking, celebrating, etc.)
- Responsive design for different screen sizes

### 3.2 Hero Section

- Minimalist layout with Gen character
- Compelling headline: "Descubra o que marcou cada ano da história"
- Quick search preview (year + country input)
- Smooth scroll animations

### 3.3 Features Showcase

- Visual cards showing example searches
- Screenshots of generated content
- Social proof elements
- Pricing comparison table

---

## 🔍 **Phase 4: Search Functionality** - [ ]

### 4.1 Search Interface

- Clean year input (numeric, with validation)
- Country dropdown with search/autocomplete
- Loading states with Gen character animations
- Error handling with friendly messages

### 4.2 Data Management

- Create JSON files for different countries/years
- Structure data categories:
  - Major events
  - Popular music
  - Movies/TV shows
  - Technology milestones
  - Prices/economy
  - Sports achievements

### 4.3 Results Display

- Card-based layout for different categories
- Expandable sections
- Visual icons for each category
- Share buttons for individual items

---

## 💳 **Phase 5: Subscription & Monetization** - [ ]

### 5.1 Usage Tracking

- Implement search counter for free users
- Session-based tracking for non-authenticated users
- Clear usage indicators in UI
- Upgrade prompts when limit reached

### 5.2 Stripe Integration

- Create subscription plans ($7-8/month, $4-5/month annual)
- Stripe Checkout integration
- Webhook handlers for subscription events
- Customer portal for subscription management

### 5.3 AdSense Integration (Free Users)

- Strategic ad placement that doesn't disrupt UX
- Dark theme compatible ad styles
- A/B testing for optimal placement

---

## 🖼️ **Phase 6: Image Generation & Sharing** - [ ]

### 6.1 Satori Setup

- Create template designs for different content types
- Include Gen character branding
- Responsive design for social media formats
- Dark theme optimized colors

### 6.2 Share Cards

- Movie posters with year/country info
- Music charts with album covers
- Event timelines with key facts
- Price comparison charts
- Custom Gen character watermark

### 6.3 Social Integration

- One-click sharing to major platforms
- Optimized image sizes for each platform
- Custom share texts with hashtags
- Analytics tracking for viral content

---

## 🚀 **Phase 7: Performance & User Experience** - [ ]

### 7.1 Optimization

- Image optimization and lazy loading
- API response caching (Redis for production)
- Search result pagination
- Progressive loading for large datasets

### 7.2 Mobile-First Design

- Touch-friendly interface
- Swipe gestures for navigation
- Optimized image generation for mobile sharing
- Fast loading times on slower connections

### 7.3 Analytics & Feedback

- Search pattern analysis
- Popular years/countries tracking
- User behavior insights
- A/B testing framework

---

## 📱 **Phase 8: Dark Mode UI Details** - [ ]

### 8.1 Color Palette

```css
:root {
  --background: 0 0% 3.9%;
  --foreground: 0 0% 98%;
  --primary: 142 70% 45%; /* Gen character accent */
  --secondary: 0 0% 14.9%;
  --accent: 142 70% 45%;
  --muted: 0 0% 14.9%;
  --border: 0 0% 14.9%;
  --card: 0 0% 6%;
}
```

### 8.2 Component Styling

- Subtle gradients and glows
- Smooth transitions and micro-interactions
- High contrast for accessibility
- Consistent spacing and typography
- Minimal shadows with dark theme optimization

### 8.3 Gen Character Integration

- Contextual animations based on user actions
- Personality traits reflected in micro-interactions
- Consistent branding across all touchpoints
- Memorable and shareable visual identity

---

## ⚡ **Implementation Priority Order**

1. **Week 1**: Setup infrastructure (dependencies, database, auth)
2. **Week 2**: Build landing page with Gen branding and dark theme
3. **Week 3**: Implement basic search functionality with sample data
4. **Week 4**: Add user management and search limits
5. **Week 5**: Integrate Stripe payments and subscription logic
6. **Week 6**: Build image generation and sharing features
7. **Week 7**: Add AdSense integration and mobile optimization
8. **Week 8**: Testing, polish, and deployment

---

## 📅 MVP Progress Tracker

- [x] Criar projeto Next.js
- [ ] Setup UI foundation & dark theme
- [ ] Create database schema and authentication
- [ ] Build landing page with Gen branding
- [ ] Implement search functionality with JSON data
- [ ] Add user management and search limits
- [ ] Integrate Stripe payments
- [ ] Build image generation and sharing
- [ ] Add AdSense integration
- [ ] Testing and deployment

---

## 🤖 Development Guidelines

- Work **one task at a time**, always following MVP order
- Ask before assuming anything not specified
- Don't implement future roadmap features now
- If error occurs → fix first, then continue
- Always update this file marking progress
- Focus on minimalist dark mode UI throughout

---

## 🛣️ Future Roadmap

- Card customization (colors, styles)
- **Nostalgia mode** (old TV, printed newspaper)
- Global ranking of most searched years
- Weekly popular years feed
- Direct TikTok/Instagram sharing

---

## 🎨 Slogan

_"Genyear — Descubra o que marcou cada ano da história."_
