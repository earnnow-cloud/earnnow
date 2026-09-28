# EarnNow — Viral Link Engine

A serverless Next.js application for turning public URLs into trackable sharing links.

## Architecture

- **Frontend + serverless API:** Next.js App Router on Vercel
- **Database:** Supabase Postgres
- **No traditional server:** Vercel handles serverless route execution
- **Anonymous-first:** campaign creation does not require signup
- **Private dashboard:** each campaign receives a random owner token
- **Social sharing:** Web Share API, Facebook share URL, WhatsApp share URL, and copy link
- **Analytics:** visits, shares, content opens, unique visitor hashes
- **Monetization slot:** optional Adsterra/Monetag Direct/SmartLink shown as a clearly labeled sponsored offer
- **Social preview:** generated Open Graph images for the homepage and campaign pages

Vercel provides first-class Next.js support and deploys dynamic routes/APIs as serverless infrastructure. Supabase provides managed Postgres and can be used without running your own database server.

## 1. Create Supabase project

Create a project at Supabase, then open **SQL Editor** and run:

`supabase/schema.sql`

The API uses the Supabase service-role key on the server. Never put this key in `NEXT_PUBLIC_*` variables.

## 2. Configure environment

Copy `.env.example` to `.env.local`:

```env
NEXT_PUBLIC_SITE_URL=http://localhost:3000
NEXT_PUBLIC_SUPABASE_URL=https://YOUR_PROJECT.supabase.co
NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=YOUR_PUBLISHABLE_KEY
SUPABASE_SERVICE_ROLE_KEY=YOUR_SERVICE_ROLE_KEY

NEXT_PUBLIC_AD_PROVIDER=none
NEXT_PUBLIC_AD_DIRECT_LINK=
```

The publishable key is included for future client-side Supabase features; the current MVP uses the server-side service-role route for its database operations.

## 3. Install and run

```bash
npm install
npm run dev
```

Open `http://localhost:3000`.

## 4. Vercel deployment

1. Push this folder to a GitHub repository.
2. Import the repository into Vercel.
3. Add the environment variables from `.env.local` in Vercel Project Settings → Environment Variables.
4. Set `NEXT_PUBLIC_SITE_URL` to `https://earnnow.biz` after the domain is connected.
5. Deploy.

No VPS, cPanel, Apache, Node server, or traditional hosting is required.

## 5. Domain

Point `earnnow.biz` to the Vercel project and set it as the production domain.

## 6. Adsterra / Monetag

Do not hard-code a network URL into source code. Add the exact Direct/SmartLink supplied by the network to:

```env
NEXT_PUBLIC_AD_PROVIDER=adsterra
NEXT_PUBLIC_AD_DIRECT_LINK=https://...
```

or:

```env
NEXT_PUBLIC_AD_PROVIDER=monetag
NEXT_PUBLIC_AD_DIRECT_LINK=https://...
```

The production UI should present this as a clearly labeled sponsored/advertising action. Do not disguise an ad as the original-content navigation button, do not auto-click it, and do not incentivize ad clicks.

## Product flow

`Facebook Ad → earnnow.biz → paste URL → create campaign → share → friend opens → analytics → sponsored monetization`

## Important production hardening before paid traffic

1. Add rate limiting at the API edge.
2. Add bot/abuse detection and campaign quotas.
3. Add URL reputation/safe-browsing checks if you will allow arbitrary public URLs.
4. Add privacy policy, terms, cookie/analytics disclosure, and abuse reporting.
5. Verify each ad network's current traffic/placement rules before enabling paid traffic.
6. Add server-side event aggregation instead of retaining raw user-agent/referrer data longer than needed.
7. Add authentication if users need long-term account-based campaign management.

## What this MVP intentionally does not do

It never reads a user's private friend list or automatically sends messages. Sharing uses voluntary platform share actions.
