# MCRO-CRMS v2.0
## Municipal Civil Registrar's Office — General Luna, Quezon
### Civil Registry Management System

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18 + Vite |
| Router | React Router v6 |
| Charts | Recharts |
| HTTP | Axios |
| PDF | jsPDF + AutoTable |
| Backend / DB | Supabase (PostgreSQL) |
| Auth | Supabase Auth (email/password) |
| Hosting | Vercel (frontend static build) |

---

## Project Structure

```
mcro-crms/
├── supabase_setup.sql          ← Run in Supabase SQL Editor first
├── .env.example                ← Copy to .env.local and fill in
├── vercel.json                 ← SPA routing for Vercel
├── vite.config.js
├── package.json
├── index.html
└── src/
    ├── main.jsx
    ├── App.jsx                 ← Routes + Auth + Toast providers
    ├── index.css               ← Design system tokens + utilities
    ├── lib/
    │   ├── supabase.js         ← Supabase client
    │   ├── constants.js        ← Transaction types, barangays, services
    │   └── pdf.js              ← PDF export utilities
    ├── hooks/
    │   ├── useAuth.jsx         ← Auth context (signIn, signOut, profile)
    │   └── useToast.jsx        ← Toast notification context
    └── pages/
        ├── public/
        │   ├── LandingPage.jsx ← Full public landing page
        │   ├── LoginPage.jsx   ← Staff login (key-protected)
        │   └── QueueDisplay.jsx← TV/monitor queue display
        └── admin/
            ├── AdminLayout.jsx ← Sidebar + outlet
            ├── Dashboard.jsx
            ├── Records.jsx
            ├── Analytics.jsx
            ├── QueueManager.jsx
            ├── Reports.jsx
            ├── AuditLogs.jsx
            └── UserManagement.jsx
```

---

## Setup Instructions

### Step 1 — Create Supabase Project

1. Go to [supabase.com](https://supabase.com) and create a new project
2. Wait for the project to finish provisioning
3. Go to **Settings → API** and copy:
   - **Project URL** → `VITE_SUPABASE_URL`
   - **anon public key** → `VITE_SUPABASE_ANON_KEY`

### Step 2 — Run the Database Setup

1. In the Supabase Dashboard, go to **SQL Editor**
2. Paste the entire contents of `supabase_setup.sql`
3. Click **Run**

### Step 3 — Create the Default Admin Account

1. In Supabase Dashboard, go to **Authentication → Users**
2. Click **Add User**:
   - Email: `admin@mcro-generaluna.gov.ph`
   - Password: `Admin@2024`
3. Copy the user's UUID
4. Go to **SQL Editor** and run:

```sql
INSERT INTO public.profiles (id, email, role)
VALUES ('<PASTE_UUID_HERE>', 'admin@mcro-generaluna.gov.ph', 'admin')
ON CONFLICT (id) DO UPDATE SET role = 'admin';
```

### Step 4 — Configure Environment

```bash
cp .env.example .env.local
```

Edit `.env.local`:
```
VITE_SUPABASE_URL=https://YOUR_PROJECT_ID.supabase.co
VITE_SUPABASE_ANON_KEY=YOUR_ANON_KEY_HERE
```

### Step 5 — Run Locally

```bash
npm install
npm run dev
```

Open http://localhost:5173

### Step 6 — Deploy to Vercel

1. Push to GitHub
2. In Vercel, import the repository
3. Add environment variables (`VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`)
4. Deploy

The `vercel.json` file handles SPA routing automatically.

---

## Accessing the System

| URL | Description |
|---|---|
| `/` | Public landing page |
| `/login?key=mcro-staff-2024` | Staff login page |
| `/queue-display` | TV queue display (fullscreen) |
| `/app/dashboard` | Admin dashboard (requires login) |
| `/app/records` | Records management |
| `/app/analytics` | Analytics & charts |
| `/app/queue` | Queue management |
| `/app/reports` | PDF reports |
| `/app/audit` | Audit logs |
| `/app/users` | User management (admin only) |

> **Note:** Visiting `/login` without the key redirects to the landing page.

---

## Default Credentials

| Field | Value |
|---|---|
| Email | `admin@mcro-generaluna.gov.ph` |
| Password | `Admin@2024` |
| Role | `admin` |

⚠️ **Change the default password after first login.**

---

## Adding Static Assets

Place these files in the `/public` folder:
- `/public/municipal-hall.png` — Hero section background photo
- `/public/staff.png` — Staff section photo

The system will display gracefully without them (dark overlay backgrounds).

---

## Supabase RLS Summary

| Table | Public Read | Auth Read | Auth Write |
|---|---|---|---|
| `records` | ✗ | ✓ | ✓ |
| `audit_logs` | ✗ | ✓ | ✓ (insert) |
| `queue` | ✓ | ✓ | ✓ |
| `settings` | ✓ | ✓ | ✓ |
| `profiles` | ✗ | own + admin | admin |

---

## Design System

| Token | Value |
|---|---|
| Navy | `#0F1F3D` |
| Gold | `#C9973A` |
| Cream | `#FAF8F4` |
| Heading Font | Playfair Display / Noto Serif |
| Body Font | DM Sans |

---

## Features Checklist

### Public Landing Page
- [x] Sticky glassmorphism header
- [x] Live clock (Philippine Standard Time)
- [x] Live Queue Display (auto-refresh 2.5s)
- [x] Office Open/Closed indicator
- [x] Philippine Holiday auto-banner
- [x] Public Announcement banner
- [x] 6 service cards with expandable details
- [x] Records on file table
- [x] Staff feature section
- [x] Requirements accordion
- [x] Fees & processing time grid
- [x] FAQ accordion (6 Q&As)
- [x] Contact section with Google Maps
- [x] 4-column footer with live queue
- [x] Scroll-to-top button

### Admin Panel
- [x] Key-protected login (`?key=mcro-staff-2024`)
- [x] Animated login page
- [x] Collapsible sidebar navigation
- [x] Role-based access (admin/staff)

### Dashboard
- [x] 6 stat cards
- [x] Quick Actions grid (3×2)
- [x] Monthly volume bar chart
- [x] Status distribution pie chart
- [x] Top transaction types list

### Records Management
- [x] Paginated table (15/page)
- [x] Search by client/owner/mobile
- [x] Filter by status/type/date
- [x] Add record modal
- [x] Edit with mandatory reason field
- [x] Delete with mandatory reason field
- [x] Audit logging for all changes
- [x] PDF export (landscape, MCRO header)
- [x] All 33 transaction types
- [x] All 28 barangays

### Analytics
- [x] Summary cards (total/pending/processing/completed)
- [x] Monthly volume bar chart (6 months)
- [x] Top 10 transaction types (horizontal bar)
- [x] Status breakdown pie chart
- [x] Sex distribution pie chart
- [x] Top barangays horizontal bar

### Queue Management
- [x] Public Announcement Manager
- [x] Today's stats (total/served/waiting)
- [x] Issue ticket (manual or auto)
- [x] Now Serving display
- [x] Call Next button with two-tone chime
- [x] Sound toggle (🔔/🔕)
- [x] Waiting list with individual call
- [x] Reset Queue button
- [x] TV Queue Display at `/queue-display`

### Reports
- [x] Daily report (summary + full list)
- [x] Weekly report
- [x] Monthly report
- [x] MCRO-branded PDF output

### Audit Logs
- [x] Paginated table (30/page)
- [x] Search by description/email
- [x] Filter by action type
- [x] Color-coded action badges

### User Management (Admin)
- [x] List all staff accounts
- [x] Avatar initials, email, role, date
- [x] Add user (email/password/role)
- [x] Edit user
- [x] Delete user (cannot delete own)
- [x] All changes logged
