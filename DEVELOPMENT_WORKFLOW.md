# Development Workflow Guide

A guide for developing, testing, and extending the CRM application.

---

## 🚀 Quick Commands Reference

### Backend

```bash
# Navigate to server
cd server

# Install dependencies
npm install

# Run production server
npm start

# Run development server (auto-restart on changes)
npm run dev

# Install new package
npm install package-name

# Install dev dependency
npm install --save-dev nodemon
```

### Frontend

```bash
# Navigate to client
cd client

# Install dependencies
npm install

# Run development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview

# Install new package
npm install package-name
```

---

## 📋 Pre-Development Checklist

Before starting development, ensure:

- [ ] Node.js 18+ installed
- [ ] MongoDB running locally or Atlas configured
- [ ] Server `.env` file created and configured
- [ ] Both `node_modules` installed
- [ ] Backend starts without errors
- [ ] Frontend starts without errors
- [ ] Can login with test account

---

## 🔧 Common Development Tasks

### Adding a New API Endpoint

**1. Backend - Create Controller:**
```javascript
// server/controllers/leadController.js (add new function)
exports.getLeadsByStatus = async (req, res) => {
  try {
    const { status } = req.params;
    const leads = await Lead.find({ status });
    res.json({ success: true, data: leads });
  } catch (error) {
    res.status(500).json({ success: false, message: error.message });
  }
};
```

**2. Backend - Add Route:**
```javascript
// server/routes/leadRoutes.js
router.get('/status/:status', protect, getLeadsByStatus);
```

**3. Frontend - Add API Call:**
```javascript
// client/src/services/api.js
export const leadsAPI = {
  getByStatus: (status) => api.get(`/leads/status/${status}`)
};
```

**4. Frontend - Use in Component:**
```javascript
// client/src/pages/Dashboard.jsx
import { leadsAPI } from '../services/api';

const fetchByStatus = async (status) => {
  const response = await leadsAPI.getByStatus(status);
  setLeads(response.data.data);
};
```

### Adding a New Page

**1. Create Component:**
```javascript
// client/src/pages/NewPage.jsx
import React from 'react';
import '../styles/NewPage.css';

const NewPage = () => {
  return <div className="new-page">...</div>;
};

export default NewPage;
```

**2. Create Styles:**
```css
/* client/src/styles/NewPage.css */
.new-page {
  max-width: 1200px;
  margin: 0 auto;
  padding: 30px 20px;
}
```

**3. Add Route:**
```javascript
// client/src/App.jsx
import NewPage from './pages/NewPage';

// In Routes
<Route path="/new-page" element={<ProtectedRoute><NewPage /></ProtectedRoute>} />
```

**4. Add Navigation:**
```javascript
// client/src/components/Navbar.jsx
<Link to="/new-page">New Page</Link>
```

### Modifying Database Models

**Example: Add phone validation to User**

```javascript
// server/models/User.js
const userSchema = new mongoose.Schema({
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true },
  phone: {
    type: String,
    match: [/^\+?[\d\s-()]{10,}$/, 'Invalid phone number']
  },
  createdAt: { type: Date, default: Date.now }
});
```

---

## 🧪 Testing Your Changes

### Backend Testing with cURL

**Test Create Lead:**
```bash
curl -X POST http://localhost:5000/api/leads \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Test Lead",
    "email": "test@example.com",
    "source": "manual"
  }'
```

**Test Get Leads:**
```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
  http://localhost:5000/api/leads
```

### Frontend Testing

1. Open browser DevTools (F12)
2. Go to Application → LocalStorage
3. Verify token is stored
4. Go to Console → `fetch('http://localhost:5000/api/health')`
5. Check network tab for API calls

### Manual Testing Flow

1. **Test Registration:**
   - Go to login page
   - Click "Register"
   - Enter email & password
   - Verify token saved

2. **Test Lead Creation:**
   - Click "+ Add New Lead"
   - Fill form
   - Submit
   - Verify lead appears in list

3. **Test Lead Update:**
   - Click lead
   - Edit information
   - Change status
   - Save changes

4. **Test Notes:**
   - Add note to lead
   - Verify note displays
   - Add another note
   - Check order (newest first)

5. **Test Deletion:**
   - Delete a lead
   - Confirm deletion
   - Verify removed from list

---

## 🐛 Debugging Tips

### Frontend Debugging

**Check Console for Errors:**
```javascript
// DevTools Console
console.error('My error message');
console.log('Variable:', variable);
```

**Check Network Requests:**
1. Open DevTools Network tab
2. Perform action
3. Look for API call
4. Check request/response

**Check Local Storage:**
```javascript
// In DevTools Console
localStorage.getItem('token')
localStorage.clear()  // Clear all
```

### Backend Debugging

**Add Logging:**
```javascript
// server/controllers/leadController.js
console.log('Received request:', req.body);
console.log('Error:', error);
```

**Test Individual Routes:**
```bash
# Test endpoint directly
curl http://localhost:5000/api/health

# With verbose output
curl -v http://localhost:5000/api/health
```

**Check Environment Variables:**
```javascript
// In server/index.js
console.log('MongoDB URI:', process.env.MONGODB_URI);
console.log('Port:', process.env.PORT);
```

### Database Debugging

**Connect to MongoDB:**
```bash
mongosh  # or mongo

# Inside MongoDB
use simple-crm
db.users.find()
db.leads.find()
```

---

## 📦 Adding Dependencies

### Backend Example: Add Express Validator

```bash
cd server
npm install express-validator
```

**Use in code:**
```javascript
// server/middleware/validationMiddleware.js
const { body, validationResult } = require('express-validator');

exports.validateLead = [
  body('name').notEmpty().withMessage('Name required'),
  body('email').isEmail().withMessage('Valid email required')
];

// In routes
router.post('/', validateLead, createLead);
```

### Frontend Example: Add Date Library

```bash
cd client
npm install date-fns
```

**Use in component:**
```javascript
import { format } from 'date-fns';

const formattedDate = format(new Date(lead.createdAt), 'MMM dd, yyyy');
```

---

## 🔄 Git Workflow (if using version control)

```bash
# Check status
git status

# Add changes
git add .

# Commit changes
git commit -m "Add new feature: X"

# Push to remote
git push origin main
```

---

## 🎨 Frontend Development Tips

### Component Reusability

**Before (Not Reusable):**
```javascript
// Specific to dashboard
const DashboardButton = () => {
  return <button className="dashboard-btn">Click</button>;
};
```

**After (Reusable):**
```javascript
// Generic button component
const Button = ({ text, variant = 'primary', onClick }) => {
  return <button className={`btn btn-${variant}`} onClick={onClick}>{text}</button>;
};
```

### State Management Pattern

```javascript
// ❌ Don't do this
const [allLeads, setAllLeads] = useState([]);
const [filteredLeads, setFilteredLeads] = useState([]);
const [sortedLeads, setSortedLeads] = useState([]);

// ✅ Do this instead
const [leads, setLeads] = useState([]);
const filteredLeads = leads.filter(/* condition */);
const sortedLeads = filteredLeads.sort(/* logic */);
```

### CSS Best Practices

```css
/* ❌ Don't use too specific selectors */
.dashboard-container .filter-section .filter-btn {}

/* ✅ Use cleaner selectors */
.filter-btn {}

/* ❌ Don't hardcode colors */
color: #3498db;

/* ✅ Use CSS variables */
:root {
  --color-primary: #3498db;
}
.element { color: var(--color-primary); }
```

---

## ⚡ Backend Development Tips

### Error Handling Pattern

```javascript
// ❌ No error handling
const lead = await Lead.findById(id);
res.json(lead);

// ✅ Proper error handling
try {
  const lead = await Lead.findById(id);
  if (!lead) {
    return res.status(404).json({ success: false, message: 'Not found' });
  }
  res.json({ success: true, data: lead });
} catch (error) {
  res.status(500).json({ success: false, message: error.message });
}
```

### Route Organization

```javascript
// ✅ Good - Clear structure
router.route('/')
  .get(protect, getLeads)
  .post(protect, createLead);

router.route('/:id')
  .get(protect, getLead)
  .put(protect, updateLead)
  .delete(protect, deleteLead);
```

### Middleware Usage

```javascript
// ✅ Apply middleware to specific routes
router.get('/admin-only', protect, requireAdmin, getAdminData);

// ✅ Apply to all routes in group
router.use(protect);
router.get('/', getAll);
router.post('/', create);
```

---

## 🚀 Performance Optimization

### Frontend

**Lazy Load Components:**
```javascript
import { lazy, Suspense } from 'react';

const LeadDetails = lazy(() => import('./pages/LeadDetails'));

// In routes
<Suspense fallback={<div>Loading...</div>}>
  <LeadDetails />
</Suspense>
```

**Memoize Components:**
```javascript
import { memo } from 'react';

const LeadCard = memo(({ lead }) => (
  <div>{lead.name}</div>
));

export default LeadCard;
```

### Backend

**Database Indexing:**
```javascript
// server/models/Lead.js
const leadSchema = new mongoose.Schema({
  email: { type: String, index: true },  // Index for faster queries
  status: { type: String, enum: [...], index: true }
});
```

**Query Optimization:**
```javascript
// ❌ Slow - loads all fields
const leads = await Lead.find();

// ✅ Fast - only needed fields
const leads = await Lead.find().select('name email status');

// ✅ Faster - read-only
const leads = await Lead.find().lean();
```

---

## 📝 Code Style Guidelines

### JavaScript/JSX
- Use `const` by default, `let` when needed
- Use arrow functions
- Destructure props and objects
- Use meaningful variable names

### CSS
- Use classes (not IDs for styling)
- Group related styles
- Use consistent spacing
- Follow mobile-first approach

### Comments
- Comment WHY not WHAT
- Keep comments up-to-date
- Use JSDoc for functions

```javascript
// ❌ Bad comment - obvious
const result = items.filter(item => item.active); // Filter active items

// ✅ Good comment - explains why
// Filter for active items to show only current listings
const result = items.filter(item => item.active);
```

---

## 🔐 Security During Development

- ❌ Never commit `.env` files
- ❌ Never hardcode API keys
- ✅ Always validate user input
- ✅ Always verify JWT tokens
- ✅ Use HTTPS in production
- ✅ Keep dependencies updated

---

## 📚 Useful Resources

- **React Docs:** https://react.dev
- **Express Guide:** https://expressjs.com
- **MongoDB Manual:** https://docs.mongodb.com
- **MDN Web Docs:** https://developer.mozilla.org
- **JavaScript.info:** https://javascript.info

---

## 🆘 Getting Help

1. Check browser console for errors
2. Check terminal for backend errors
3. Review the full documentation
4. Check existing code examples
5. Search for similar issues online

---

## ✅ Before Committing Code

- [ ] Code follows style guidelines
- [ ] No console.log statements left
- [ ] Error handling implemented
- [ ] Tested locally
- [ ] No security vulnerabilities
- [ ] Comments updated
- [ ] Dependencies installed

---

**Happy Developing! 🎉**

Remember: Clean code is a journey, not a destination. Keep learning and improving!
