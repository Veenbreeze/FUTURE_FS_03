# Quick Start Guide

Get your CRM running in 5 minutes!

## Prerequisites
- Node.js v18+
- MongoDB (local or Atlas)

## Quick Setup

### 1. Backend Setup
```bash
cd server
npm install
# Configure .env with your MongoDB URI and JWT_SECRET
npm start
```
Backend runs on: `http://localhost:5000`

### 2. Frontend Setup (New Terminal)
```bash
cd client
npm install
npm run dev
```
Frontend runs on: `http://localhost:3000`

### 3. Create Admin Account
1. Go to http://localhost:3000
2. Click "Register"
3. Enter email and password
4. Start managing leads!

## Available Scripts

**Backend:**
- `npm start` - Run production server
- `npm run dev` - Run with nodemon (auto-restart)

**Frontend:**
- `npm run dev` - Start dev server
- `npm run build` - Build for production
- `npm run preview` - Preview production build

## API Base URL
- Development: `http://localhost:5000/api`
- Update in `client/src/services/api.js` for production

## Database

### Local MongoDB
```bash
mongod
```

### MongoDB Atlas (Cloud)
1. Go to mongodb.com/cloud/atlas
2. Create cluster
3. Get connection string
4. Update `MONGODB_URI` in `server/.env`

## Default Admin Account
Create one on first registration - no pre-existing accounts!

## Troubleshooting

| Problem | Solution |
|---------|----------|
| "Cannot POST /api/auth/login" | Backend not running |
| "Cannot GET /" | Frontend not running |
| "MongoDB connection failed" | Start MongoDB, check connection string |
| Port 5000 in use | Change PORT in .env |
| Port 3000 in use | Change port in vite.config.js |

## Learn More
See [SETUP_GUIDE.md](./SETUP_GUIDE.md) for detailed documentation.

---

**Need help?** Check out the full setup guide or API documentation!
