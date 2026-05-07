# Full-Stack CRM Application 🚀

A complete, production-ready Customer Relationship Management (CRM) system built with modern web technologies. Manage leads from website contact forms with full authentication, CRUD operations, and a beautiful responsive UI.

## Overview

This is a **full-stack application** consisting of:
- **Frontend:** React 19 + Vite + React Router + Axios
- **Backend:** Node.js + Express.js + MongoDB + Mongoose
- **Authentication:** JWT + bcrypt
- **Styling:** Modern CSS

Perfect for learning full-stack development or as a foundation for a production application.

## Features ✨

### Core Features
- ✅ Admin authentication (register & login)
- ✅ Secure password hashing with bcrypt
- ✅ JWT-based authentication
- ✅ Complete Lead CRUD operations
- ✅ Lead status tracking (New, Contacted, Converted)
- ✅ Notes and follow-up management
- ✅ Protected routes with middleware
- ✅ Responsive modern UI
- ✅ RESTful API design

### Technical Features
- ✅ Modular, clean code architecture
- ✅ Error handling middleware
- ✅ CORS enabled for development
- ✅ Environment variable configuration
- ✅ Input validation
- ✅ Automatic request logging

## Quick Start ⚡

### Prerequisites
- Node.js 18+ ([download](https://nodejs.org))
- MongoDB 5+ (local or [MongoDB Atlas](https://www.mongodb.com/cloud/atlas))

### 1. Backend Setup
```bash
cd server
npm install
# Edit .env with your MongoDB URI
npm start
```

Server runs on: `http://localhost:5000`

### 2. Frontend Setup (New Terminal)
```bash
cd client
npm install
npm run dev
```

Frontend runs on: `http://localhost:3000`

### 3. First Steps
1. Go to http://localhost:3000
2. Click "Register" to create admin account
3. Login with your credentials
4. Start managing leads!

## Project Structure

```
simple-crm/
├── client/                          # React Frontend
│   ├── src/
│   │   ├── components/              # Reusable components
│   │   │   ├── Navbar.jsx
│   │   │   └── LeadCard.jsx
│   │   ├── pages/                   # Page components
│   │   │   ├── Login.jsx
│   │   │   ├── Dashboard.jsx
│   │   │   └── LeadDetails.jsx
│   │   ├── services/                # API integration
│   │   │   └── api.js
│   │   ├── context/                 # React Context
│   │   │   └── AuthContext.jsx
│   │   ├── styles/                  # CSS files
│   │   │   ├── App.css
│   │   │   ├── Navbar.css
│   │   │   ├── Login.css
│   │   │   ├── Dashboard.css
│   │   │   ├── LeadCard.css
│   │   │   └── LeadDetails.css
│   │   ├── App.jsx                  # Main app & routing
│   │   ├── main.jsx                 # Entry point
│   │   └── index.css                # Global styles
│   ├── vite.config.js
│   └── package.json
│
├── server/                          # Express Backend
│   ├── config/
│   │   └── db.js                    # MongoDB connection
│   ├── models/
│   │   ├── User.js                  # User schema
│   │   └── Lead.js                  # Lead schema
│   ├── controllers/
│   │   ├── authController.js        # Auth logic
│   │   └── leadController.js        # Lead logic
│   ├── routes/
│   │   ├── authRoutes.js            # Auth endpoints
│   │   └── leadRoutes.js            # Lead endpoints
│   ├── middleware/
│   │   └── authMiddleware.js        # JWT verification
│   ├── utils/
│   │   └── generateToken.js         # JWT generation
│   ├── index.js                     # Server entry
│   ├── .env                         # Configuration
│   └── package.json
│
├── SETUP_GUIDE.md                   # Detailed setup guide
├── QUICKSTART.md                    # Quick start guide
├── API_DOCUMENTATION.md             # Complete API docs
└── README.md                        # This file
```

## Documentation

### Quick References
- **[QUICKSTART.md](./QUICKSTART.md)** - Get running in 5 minutes
- **[SETUP_GUIDE.md](./SETUP_GUIDE.md)** - Complete setup & configuration
- **[API_DOCUMENTATION.md](./API_DOCUMENTATION.md)** - Full API reference
- **[client/README.md](./client/README.md)** - Frontend documentation
- **[server/README.md](./server/README.md)** - Backend documentation

### API Endpoints

**Authentication:**
- `POST /api/auth/register` - Register new admin
- `POST /api/auth/login` - Admin login

**Leads (Protected):**
- `GET /api/leads` - Get all leads
- `GET /api/leads/:id` - Get single lead
- `POST /api/leads` - Create lead
- `PUT /api/leads/:id` - Update lead
- `DELETE /api/leads/:id` - Delete lead
- `POST /api/leads/:id/notes` - Add note

See [API_DOCUMENTATION.md](./API_DOCUMENTATION.md) for complete details.

## Available Scripts

### Frontend
```bash
cd client
npm run dev      # Start development server
npm run build    # Build for production
npm run preview  # Preview production build
```

### Backend
```bash
cd server
npm start        # Run production server
npm run dev      # Run with auto-reload (nodemon)
```

## Database Models

### User
```javascript
{
  email: String (unique, required),
  password: String (hashed),
  createdAt: Date
}
```

### Lead
```javascript
{
  name: String (required),
  email: String (required),
  phone: String (optional),
  source: String (enum: 'website form', 'referral', 'manual'),
  status: String (enum: 'new', 'contacted', 'converted'),
  notes: Array of { text, createdAt },
  createdAt: Date
}
```

## Configuration

### Environment Variables (.env)

**Backend (`server/.env`):**
```env
MONGODB_URI=mongodb://localhost:27017/simple-crm
JWT_SECRET=your_secret_key_here
JWT_EXPIRE=7d
PORT=5000
```

**Frontend:** Update `src/services/api.js` for different environments

## Development Workflow

### Frontend Development
1. Create component in `src/components/` or `src/pages/`
2. Add API calls using `src/services/api.js`
3. Create route in `src/App.jsx`
4. Add styles in `src/styles/`

### Backend Development
1. Create model in `server/models/`
2. Create controller in `server/controllers/`
3. Create routes in `server/routes/`
4. Test with Postman or cURL

### Testing API
```bash
# Using cURL
curl -H "Authorization: Bearer TOKEN" http://localhost:5000/api/leads

# Using Postman
# Import endpoints from API_DOCUMENTATION.md
```

## Features Walkthrough

### 1. Authentication
- Register new admin account
- Login with email & password
- JWT token stored in localStorage
- Auto-logout on token expiration
- Protected routes

### 2. Dashboard
- View all leads
- Filter by status (All, New, Contacted, Converted)
- Add new leads inline
- Quick edit/delete actions
- Lead count for each status

### 3. Lead Management
- **Create:** Add new lead with source tracking
- **Read:** View lead details and history
- **Update:** Edit lead info and status
- **Delete:** Remove leads with confirmation
- **Notes:** Add follow-up notes to leads

## Security

### Implementation
- ✅ Passwords hashed with bcrypt (10 rounds)
- ✅ JWT authentication (expires 7 days)
- ✅ Protected API routes
- ✅ CORS enabled
- ✅ Environment variables for secrets

### Production Checklist
- ⚠️ Change `JWT_SECRET` to strong random string
- ⚠️ Use HTTPS/SSL
- ⚠️ Enable rate limiting
- ⚠️ Add input validation
- ⚠️ Add request logging
- ⚠️ Use database backups

## Performance Optimization

### Frontend
- Lazy load components with React.lazy()
- Optimize images
- Virtual scrolling for large lists
- Memoize components with React.memo

### Backend
- Add pagination for large datasets
- Database indexing
- Request caching
- Connection pooling

## Deployment

### Frontend (Vercel/Netlify)
```bash
cd client
npm run build
# Deploy dist/ folder
```

### Backend (Heroku/Railway/Render)
```bash
# Set environment variables on platform
# Deploy with git push or CLI
```

### MongoDB (Atlas)
1. Create cluster on MongoDB Atlas
2. Update `MONGODB_URI` in backend .env
3. Whitelist IP addresses

## Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| "Cannot POST /api/auth/login" | Backend not running on port 5000 |
| "Cannot connect to MongoDB" | Start MongoDB or check connection string |
| "Token is invalid" | Clear localStorage & login again |
| "CORS error" | Ensure CORS is enabled in backend |
| "Lead not found (404)" | Verify lead ID exists |

See [SETUP_GUIDE.md](./SETUP_GUIDE.md#troubleshooting) for more solutions.

## Future Enhancements

- [ ] Pagination for leads list
- [ ] Advanced filtering & search
- [ ] Email notifications
- [ ] Lead import/export
- [ ] Team collaboration
- [ ] Analytics dashboard
- [ ] File attachments
- [ ] Activity logging
- [ ] Mobile app (React Native)
- [ ] Two-factor authentication

## Code Examples

### Creating a Lead (Frontend)
```javascript
import { leadsAPI } from '../services/api';

const handleCreate = async () => {
  try {
    const response = await leadsAPI.create({
      name: 'John Doe',
      email: 'john@example.com',
      source: 'website form'
    });
    console.log('Lead created:', response.data);
  } catch (error) {
    console.error('Error:', error.message);
  }
};
```

### Creating a Lead (Backend)
```javascript
exports.createLead = async (req, res) => {
  const { name, email, phone, source } = req.body;
  
  if (!name || !email) {
    return res.status(400).json({
      success: false,
      message: 'Name and email required'
    });
  }
  
  const lead = await Lead.create({
    name, email, phone, source
  });
  
  res.status(201).json({
    success: true,
    data: lead
  });
};
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Open a pull request

## License

MIT License - feel free to use this project for learning or commercial purposes.

## Support

Need help?
- Check [SETUP_GUIDE.md](./SETUP_GUIDE.md)
- Review [API_DOCUMENTATION.md](./API_DOCUMENTATION.md)
- Check GitHub issues
- Contact the development team

## Author

Built as a complete full-stack learning project. Perfect for developers learning MERN stack.

---

## Getting Help

- **Setup Issues?** → Read [SETUP_GUIDE.md](./SETUP_GUIDE.md)
- **Need API Docs?** → Check [API_DOCUMENTATION.md](./API_DOCUMENTATION.md)
- **Quick Start?** → See [QUICKSTART.md](./QUICKSTART.md)
- **Frontend Help?** → See [client/README.md](./client/README.md)
- **Backend Help?** → See [server/README.md](./server/README.md)

---

**Happy Building! 🎉**

Made with ❤️ for the developer community.
