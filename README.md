# 2026 Photo Calendar

Upload 12 photos → get a print-ready PDF calendar.

## Stack
- **Frontend**: Static HTML/CSS/JS (single file)
- **PDF Generation**: Client-side via [pdf-lib](https://pdf-lib.js.org/) — no server processing needed
- **Email Collection**: Supabase (Postgres + REST API)
- **Hosting**: Vercel (static)
- **Domain**: Cloudflare (DNS)

## How It Works

1. User uploads 12 photos (one per month) — files stay in the browser
2. User enters their email
3. pdf-lib loads the template PDF, composites each photo into the green placeholder area with cover-fit + clipping
4. Browser generates the final print-ready PDF (~3-5MB per image × 12 = ~30-50MB total)
5. PDF auto-downloads, email saved to Supabase
6. No images are ever uploaded to a server

## Setup

### 1. Supabase
- Create a new Supabase project (or use your existing one: `jlafwmbsuruwmzqsrjca`)
- Run `supabase-setup.sql` in the SQL Editor
- Copy your project URL and anon key

### 2. Configure
Edit `public/index.html` and replace:
```js
const SUPABASE_URL = 'https://YOUR_PROJECT.supabase.co';
const SUPABASE_ANON_KEY = 'YOUR_ANON_KEY';
```

### 3. Deploy to Vercel
```bash
cd calendar-project
vercel
```

### 4. Domain (optional)
Point your domain to Vercel via Cloudflare DNS (A record or CNAME).

## Template Specs
- Page size: 752.68 × 1063.57 pts (265.5 × 375.2mm / ~10.5" × 14.8")
- Photo area: x=29, y=459.6, w=694.7, h=568.2 pts (top half)
- Calendar grid: x=29, y=26.5, w=694.6, h=432.2 pts (bottom half)
- All calendar text/numbers are vector outlines (not font-based)
- 13 pages: 12 months (Jan=page 0) + back cover (page 12)

## Adding Payment Later
The flow is designed to slot payment in before PDF generation.
Add Razorpay/Stripe between the "Preview" and "Generate" steps.

## File Structure
```
calendar-project/
├── public/
│   ├── index.html          # The entire app (single file)
│   └── 2026_template.pdf   # Calendar template
├── vercel.json             # Vercel routing config
├── supabase-setup.sql      # Database schema
└── README.md
```
