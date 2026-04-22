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
```annot delete own)
- [x] All changes logged
