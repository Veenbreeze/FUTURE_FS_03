# Full-Stack CRM Application - Setup & Installation Guide

A complete Customer Relationship Management (CRM) system built with React, Node.js, Express, and MongoDB. This application manages leads from website contact forms with full authentication and CRUD operations.

## Project Overview

### Features
- ✅ Admin authentication with JWT
- ✅ Secure password hashing with bcrypt
- ✅ Complete Lead CRUD operations
- ✅ Lead status tracking (New, Contacted, Converted)
- ✅ Notes and follow-up management
- ✅ Protected routes with middleware
- ✅ Modern responsive UI with CSS
- ✅ RESTful API design

### Tech Stack

**Frontend:**
- React 19.2.5 (with Vite)
- React Router v7.1.1
- Axios for API calls
- Pure CSS for styling

**Backend:**
- Node.js with Express.js
- MongoDB with Mongoose
- JWT for authentication
- bcrypt for password hashing
- CORS enabled

## Project Structure

```
simple-crm/
├── client/                    # React Frontend (Vite)
│   ├── src/
│   │   ├── components/
│   │   │   ├── Navbar.jsx
│   │   │   └── LeadCard.jsx
│   │   ├── pages/
│   │   │   ├── Login.jsx
│   │   │   ├── Dashboard.jsx
│   │   │   └── LeadDetails.jsx
│   │   ├── services/
│   │   │   └── api.js
│   │   ├── context/
│   │   │   └── AuthContext.jsx
│   │   ├── styles/
│   │   │   ├── App.css
│   │   │   ├── Navbar.css
│   │   │   ├── Login.css
│   │   │   ├── Dashboard.css
│   │   │   ├── LeadCard.css
│   │   │   └── LeadDetails.css
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   └── index.css
│   ├── public/
│   │   └── index.html
│   ├── vite.config.js
│   └── package.json
│
└── server/                    # Express Backend
    ├── config/
    │   └── db.js
    ├── models/
    │   ├── User.js
    │   └── Lead.js
    ├── controllers/
    │   ├── authController.js
    │   └── leadController.js
    ├── routes/
    │   ├── authRoutes.js
    │   └── leadRoutes.js
    ├── middleware/
    │   └── authMiddleware.js
    ├── utils/
    │   └── generateToken.js
    ├── index.js
    ├── .env
    └── package.json
```

## Installation Guide

### Prerequisites
- Node.js (v18 or higher)
- npm or yarn
- MongoDB (local or cloud instance - MongoDB Atlas)

### Step 1: Install Backend Dependencies

```bash
cd server
npm install
```

### Step 2: Configure Backend Environment

Edit `server/.env` file with your configuration:

```env
MONGODB_URI=mongodb://localhost:27017/simple-crm
JWT_SECRET=your_jwt_secret_key_change_this_in_production
JWT_EXPIRE=7d
PORT=5000
```

**For MongoDB Cloud (Atlas):**
```env
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/simple-crm
JWT_SECRET=your_jwt_secret_key_change_this_in_production
JWT_EXPIRE=7d
PORT=5000
```

### Step 3: Start MongoDB

**If using local MongoDB:**
```bash
# Windows
mongod

# macOS/Linux
brew services start mongodb-community
# or
mongod
```

**Or use MongoDB Atlas:**
- Create a cluster at https://www.mongodb.com/cloud/atlas
- Get your connection string and update `.env`

### Step 4: Start Backend Server

```bash
cd server
npm start
```

You should see: `Server running on port 5000`

### Step 5: Install Frontend Dependencies

In a new terminal:

```bash
cd client
npm install
```

### Step 6: Start Frontend Development Server

```bash
cd client
npm run dev
```

The app will typically start at `http://localhost:3000` or the next available port.

## API Endpoints

### Authentication Endpoints

**Register New Admin:**
```
POST /api/auth/register
Content-Type: application/json

{
  "email": "admin@example.com",
  "password": "securepassword123"
}
```

**Login:**
```
POST /api/auth/login
Content-Type: application/json

{
  "email": "admin@example.com",
  "password": "securepassword123"
}

Response:
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### Lead Endpoints (All Require JWT Token)

**Get All Leads:**
```
GET /api/leads
Headers: Authorization: Bearer <token>
```

**Get Single Lead:**
```
GET /api/leads/:id
Headers: Authorization: Bearer <token>
```

**Create Lead:**
```
POST /api/leads
Headers: Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "+1234567890",
  "source": "website form",
  "status": "new"
}
```

**Update Lead:**
```
PUT /api/leads/:id
Headers: Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "John Updated",
  "status": "contacted",
  "phone": "+9876543210"
}
```

**Delete Lead:**
```
DELETE /api/leads/:id
Headers: Authorization: Bearer <token>
```

**Add Note to Lead:**
```
POST /api/leads/:id/notes
Headers: Authorization: Bearer <token>
Content-Type: application/json

{
  "text": "Follow-up call scheduled for tomorrow"
}
```

## Database Models

### User Schema
```javascript
{
  email: String (required, unique),
  password: String (required, hashed),
  createdAt: Date (default: now)
}
```

### Lead Schema
```javascript
{
  name: String (required),
  email: String (required),
  phone: String (optional),
  source: String (enum: 'website form', 'referral', 'manual'),
  status: String (enum: 'new', 'contacted', 'converted'),
  notes: [
    {
      text: String,
      createdAt: Date
    }
  ],
  createdAt: Date (default: now)
}
```

## Usage Guide

### First Time Setup

1. **Register an Admin Account**
   - Navigate to the login page
   - Click "Register" tab
   - Enter email and password
   - Click "Register" button

2. **Login**
   - Use your registered credentials
   - You'll be redirected to the dashboard

3. **Add Your First Lead**
   - Click "+ Add New Lead" button
   - Fill in the form (name and email are required)
   - Submit to create the lead

4. **Manage Leads**
   - View all leads in dashboard
   - Filter by status (All, New, Contacted, Converted)
   - Click "View Details" to open lead details page
   - Edit lead information
   - Add notes and follow-ups

## Troubleshooting

### MongoDB Connection Error
- Ensure MongoDB is running
- Check connection string in `.env`
- Verify firewall/network settings

### Frontend can't connect to backend
- Ensure backend is running on port 5000
- Check CORS is enabled in backend
- Verify API base URL in `client/src/services/api.js`

### JWT Token Errors
- Clear localStorage: Open DevTools Console and run `localStorage.clear()`
- Login again
- Check JWT_SECRET in `.env` is set

### Port Already in Use
- Change PORT in `server/.env`
- Change port in `client/vite.config.js`

## Development Tips

### Adding a New Feature

1. **Backend:**
   - Create/update model in `server/models/`
   - Create controller in `server/controllers/`
   - Create routes in `server/routes/`
   - Add middleware if needed in `server/middleware/`

2. **Frontend:**
   - Create component in `src/components/` or `src/pages/`
   - Add API calls in `src/services/api.js`
   - Create route in `src/App.jsx`
   - Add styles in `src/styles/`

### Testing API Endpoints
Use Postman or cURL:

```bash
# Get all leads
curl -H "Authorization: Bearer <your_token>" http://localhost:5000/api/leads

# Create lead
curl -X POST http://localhost:5000/api/leads \
  -H "Authorization: Bearer <your_token>" \
  -H "Content-Type: application/json" \
  -d '{"name":"John","email":"john@example.com"}'
```

## Production Deployment

### Backend Deployment (Heroku/Railway/Render)

1. Set environment variables on hosting platform
2. Update `package.json` with:
   ```json
   "engines": {
     "node": "18.x"
   }
   ```
3. Ensure MongoDB is accessible
4. Deploy the backend

### Frontend Deployment (Vercel/Netlify)

1. Build the frontend:
   ```bash
   npm run build
   ```

2. Update API URL in `src/services/api.js` to your production backend

3. Deploy built files to hosting platform

## Security Considerations

- ✅ Passwords are hashed with bcrypt (10 salt rounds)
- ✅ JWT tokens expire after 7 days (configurable)
- ✅ Protected routes require valid token
- ✅ CORS enabled for development
- ⚠️ Change `JWT_SECRET` to a strong random string in production
- ⚠️ Use HTTPS in production
- ⚠️ Use environment variables for all secrets
- ⚠️ Implement rate limiting for production

## Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| "Cannot POST /api/auth/login" | Backend not running or wrong port |
| "token is not defined" | Login first before accessing protected routes |
| "MongoDB connection failed" | Start MongoDB or check connection string |
| "CORS error" | Ensure backend has CORS enabled |
| "404 Lead not found" | Wrong lead ID or lead was deleted |

## Performance Optimization

### Backend
- Add pagination for large lead lists
- Implement caching with Redis
- Use database indexing
- Add request logging/monitoring

### Frontend
- Lazy load components with React.lazy()
- Optimize images
- Implement virtual scrolling for large lists
- Use React memo for component optimization

## Next Steps / Future Enhancements

- [ ] Add pagination for leads list
- [ ] Implement lead search and advanced filters
- [ ] Add email notifications
- [ ] Create lead import/export functionality
- [ ] Add team collaboration features
- [ ] Implement activity/audit logging
- [ ] Add dashboard analytics
- [ ] Mobile app with React Native
- [ ] Add file attachments for leads
- [ ] Implement two-factor authentication

## License

This project is open source and available under the MIT License.

## Support

For issues or questions, please create an issue in the repository or contact the development team.

---

**Happy CRM Building! 🚀**
