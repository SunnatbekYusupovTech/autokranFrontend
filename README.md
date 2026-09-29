<div align="center">

# AUTOKRAN.UZ — Frontend

**Crane-rental website and admin console for a Tashkent heavy-equipment company**

![Next.js](https://img.shields.io/badge/Next.js-16-000000?logo=nextdotjs&logoColor=white)
![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind%20CSS-4-06B6D4?logo=tailwindcss&logoColor=white)
![next-intl](https://img.shields.io/badge/i18n-uz%20·%20ru%20·%20en-blue)
![Vercel](https://img.shields.io/badge/Deploy-Vercel-000000?logo=vercel&logoColor=white)

**[autokran.uz](https://www.autokran.uz)**

</div>

---

## Overview

The customer-facing website and internal admin panel for AUTOKRAN.UZ, a crane-rental company in Uzbekistan. Visitors browse the fleet, estimate a rental price, and book a crane with a map-picked location. Staff manage orders, the fleet and site content from a role-based admin console.

This repository is **UI only**. All data comes from a separate Express API ([`autokran`](https://github.com/SUNNATBEE/autokran)). The two-repo workflow is described in [`WORKFLOW.md`](./WORKFLOW.md).

## Features

### Public site (uz · ru · en)
- Landing page with hero, company overview, **price calculator**, fleet, completed projects, reviews, FAQ and partners
- Fleet, About, FAQ and Contact pages
- **Booking modal** with a Leaflet map location picker and reverse geocoding (OpenStreetMap Nominatim); orders are delivered to the company's Telegram
- Contact form, floating quick-contact button, visit tracking
- Light/dark theme without flash of incorrect theme

### Admin console (`/admin`)
- Dashboard with business statistics
- Orders and contact requests
- Fleet management (CRUD with image upload), media library, sponsors, site settings
- **Role-based access**: `super_admin` (full access) and `order_manager` (orders and contacts only)
- JWT-guarded routes, verified at the edge in `src/proxy.ts`

### SEO & performance
- Server-rendered fleet data with ISR (`revalidate: 60`) and a static fallback if the API is unavailable
- On-demand revalidation endpoint for admins
- Localised routing, `sitemap.xml`, `robots.txt`, web manifest, LocalBusiness JSON-LD
- Security headers, `poweredByHeader: false`, Vercel Analytics

## Tech stack

| Concern | Technologies |
|---|---|
| Framework | Next.js 16 (App Router), React 19, TypeScript |
| Styling | Tailwind CSS 4, daisyUI 5, Framer Motion, Lucide icons |
| i18n | next-intl 4 (`uz` default, `ru`, `en`) |
| Forms & UX | React Hook Form, react-hot-toast |
| Maps | Leaflet + OpenStreetMap |
| Auth | JWT verification with `jose` (edge-compatible) |
| Hosting | Vercel + Vercel Analytics |

## Architecture

```
src/
├── app/
│   ├── [locale]/        # public pages: home, about, fleet, faq, contact
│   ├── admin/           # admin console (dashboard, orders, fleet, media, settings, …)
│   ├── revalidate/      # on-demand ISR purge (admin only)
│   ├── sitemap.ts · robots.ts · manifest.ts
├── components/
│   ├── sections/        # landing-page sections
│   └── admin/           # admin UI
├── lib/                 # API helpers, admin auth config, data fetching
├── constants/           # static fallback data
├── i18n/                # locale routing
└── proxy.ts             # locale routing + /admin JWT guard (Next 16 proxy)
messages/                # uz.json · ru.json · en.json
```

**Same-origin API proxy:** `next.config.ts` rewrites `/api/*` and `/uploads/*` to `BACKEND_URL`. The browser only talks to the site's own origin, which keeps the admin cookie first-party and avoids CORS.

## Getting started

```bash
git clone https://github.com/SunnatbekYusupovTech/autokranFrontend.git
cd autokranFrontend
npm install
```

Create `.env.local`:

| Variable | Purpose |
|---|---|
| `BACKEND_URL` | Express API origin (default `http://localhost:4000`) |
| `JWT_SECRET` | Must match the API; used to verify admin tokens |
| `NEXT_PUBLIC_SITE_URL` | Canonical site URL (default `https://www.autokran.uz`) |
| `NEXT_PUBLIC_GOOGLE_SITE_VERIFICATION`, `NEXT_PUBLIC_YANDEX_VERIFICATION`, `NEXT_PUBLIC_BING_VERIFICATION` | Search-console verification (optional) |

```bash
npm run dev      # http://localhost:3000
npm run build
npm run start
npm run lint
```

Start the API first; see the [`autokran`](https://github.com/SUNNATBEE/autokran) repository.

## Author

**Sunnatbek Yusupov** — [LinkedIn](https://www.linkedin.com/in/sunnatbee/) · [sunnatbekyusupov.uz](https://sunnatbekyusupov.uz)
