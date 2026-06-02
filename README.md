# GBP Manager - Google Business Profile Post Scheduler

A complete full-stack application for managing and scheduling Google Business Profile posts.

## Features

- **Multi-user authentication** (Email/Password + Google OAuth ready)
- **Business profile management** - Add and manage multiple GBP locations
- **Post scheduling** - Schedule posts with images, CTAs, and custom timing
- **Auto-publisher** - Backend cron job automatically publishes scheduled posts
- **Complete data isolation** - Each user sees only their own data
- **Responsive design** - Works on desktop, tablet, and mobile

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React 18 + Vite + Tailwind CSS + React Query + Zustand |
| Backend | Node.js + Express + MongoDB + Passport.js |
| Auth | JWT + bcrypt + Google OAuth 2.0 (ready) |
| Scheduler | node-cron |

## Quick Start (Local Development)

### 1. Clone and Setup

```bash
# Extract the project
cd gbp-scheduler

# Setup backend
cd backend
cp .env.example .env
npm install

# Setup frontend (in a new terminal)
cd ../frontend
npm install
```

### 2. Get a Free MongoDB Database

1. Go to [MongoDB Atlas](https://www.mongodb.com/atlas)
2. Sign up for free (no credit card needed)
3. Create a cluster → Click "Connect" → "Drivers" → "Node.js"
4. Copy your connection string
5. Paste it in `backend/.env` as `MONGODB_URI`

### 3. Generate Secrets

```bash
# In your terminal, run:
node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"
# Copy output → paste as JWT_SECRET in backend/.env

node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
# Copy output → paste as SESSION_SECRET in backend/.env
```

### 4. Start Development

```bash
# Terminal 1: Start backend
cd backend
npm run dev
# Server runs on http://localhost:5000

# Terminal 2: Start frontend
cd frontend
npm run dev
# App opens on http://localhost:5173
```

## Deployment (Free)

### Backend → Render.com (Free)

1. Go to [render.com](https://render.com) → Sign up with GitHub
2. Create "New Web Service" → Connect your repo
3. Set root directory: `backend`
4. Build command: `npm install`
5. Start command: `npm start`
6. Add environment variables from your `.env` file
7. Deploy (free tier: spins down after 15min inactivity, wakes up in ~30s)

### Frontend → Vercel (Free)

1. Go to [vercel.com](https://vercel.com) → Sign up with GitHub
2. Import your project
3. Set framework preset: `Vite`
4. Set root directory: `frontend`
5. Add environment variable: `VITE_API_URL=https://your-backend.onrender.com`
6. Deploy (always stays awake on free tier)

### Update CORS

After deployment, update `backend/.env`:
```
FRONTEND_URL=https://your-frontend.vercel.app
```

## Google OAuth Setup (Optional - for real Google Sign-In)

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create project → Enable **Google Business Profile API**
3. APIs & Services → Credentials → Create OAuth client ID
4. Application type: **Web application**
5. Authorized origins: `https://your-frontend.vercel.app`
6. Authorized redirect: `https://your-backend.onrender.com/api/auth/google/callback`
7. Copy Client ID and Secret to backend `.env`
8. Redeploy backend

**Note:** Google Business Profile API requires business verification. Apply at [Google's Partner Portal](https://developers.google.com/my-business/content/prereqs).

## Project Structure

```
gbp-scheduler/
├── backend/
│   ├── src/
│   │   ├── server.js          # Express server entry
│   │   ├── middleware/
│   │   │   ├── auth.js        # JWT verification
│   │   │   └── passport.js    # Google OAuth strategy
│   │   ├── models/
│   │   │   ├── User.js        # User schema
│   │   │   ├── Business.js    # Business schema
│   │   │   └── Post.js        # Post schema
│   │   └── routes/
│   │       ├── auth.js        # Auth endpoints
│   │       ├── businesses.js  # Business CRUD
│   │       ├── posts.js       # Post CRUD
│   │       └── users.js       # User stats
│   ├── .env.example
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── App.jsx            # Router setup
│   │   ├── store/
│   │   │   └── authStore.js   # Zustand auth state
│   │   ├── hooks/
│   │   │   └── useApi.js      # React Query hooks
│   │   ├── components/
│   │   │   └── Layout.jsx     # Sidebar layout
│   │   └── pages/
│   │       ├── Login.jsx
│   │       ├── Register.jsx
│   │       ├── Dashboard.jsx
│   │       ├── Businesses.jsx
│   │       ├── Posts.jsx
│   │       ├── Scheduled.jsx
│   │       ├── Published.jsx
│   │       └── Settings.jsx
│   ├── index.html
│   ├── vite.config.js
│   └── package.json
└── README.md
```

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/register` | Create account |
| POST | `/api/auth/login` | Login with email |
| GET | `/api/auth/google` | Google OAuth login |
| GET | `/api/auth/me` | Get current user |
| GET | `/api/businesses` | List businesses |
| POST | `/api/businesses` | Create business |
| DELETE | `/api/businesses/:id` | Delete business |
| GET | `/api/posts` | List posts |
| POST | `/api/posts` | Create post |
| PUT | `/api/posts/:id` | Update post |
| DELETE | `/api/posts/:id` | Delete post |
| GET | `/api/users/stats` | Get user stats |
| DELETE | `/api/users/account` | Delete account |

## License

MIT - Free to use and modify.
