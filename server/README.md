# CRM Backend Server

Node.js + Express.js backend for the full-stack CRM application.

## Quick Start

```bash
npm install
npm start      # Production
npm run dev    # Development (auto-restart with nodemon)
```

Server runs on: `http://localhost:5000`

## Configuration

Create `.env` file in `server/` directory:

```env
MONGODB_URI=mongodb://localhost:27017/simple-crm
JWT_SECRET=your_super_secret_key_change_in_production
JWT_EXPIRE=7d
PORT=5000
```

### MongoDB Setup

**Local MongoDB:**
```bash
mongod
```

**MongoDB Atlas (Cloud):**
1. Go to https://www.mongodb.com/cloud/atlas
2. Create a cluster
3. Get connection string
4. Update `MONGODB_URI` in `.env`

## Project Structure

```
server/
├── config/
│   └── db.js              # MongoDB connection setup
├── models/
│   ├── User.js            # User schema & methods
│   └── Lead.js            # Lead schema with notes
├── controllers/
│   ├── authController.js  # Authentication logic
│   └── leadController.js  # Lead CRUD logic
├── routes/
│   ├── authRoutes.js      # Auth endpoints
│   └── leadRoutes.js      # Lead endpoints
├── middleware/
│   └── authMiddleware.js  # JWT verification
├── utils/
│   └── generateToken.js   # JWT token generation
├── index.js               # Main server file
├── .env                   # Environment variables
└── package.json
```

## API Endpoints

### Authentication

**Register:**
```
POST /api/auth/register
{
  "email": "admin@example.com",
  "password": "password123"
}
```

**Login:**
```
POST /api/auth/login
{
  "email": "admin@example.com",
  "password": "password123"
}
```

### Leads (All Require JWT Token)

**Get All:**
```
GET /api/leads
Headers: Authorization: Bearer <token>
```

**Get One:**
```
GET /api/leads/:id
Headers: Authorization: Bearer <token>
```

**Create:**
```
POST /api/leads
Headers: Authorization: Bearer <token>
{
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "+1234567890",
  "source": "website form"
}
```

**Update:**
```
PUT /api/leads/:id
Headers: Authorization: Bearer <token>
{
  "status": "contacted",
  "name": "Jane Doe"
}
```

**Delete:**
```
DELETE /api/leads/:id
Headers: Authorization: Bearer <token>
```

**Add Note:**
```
POST /api/leads/:id/notes
Headers: Authorization: Bearer <token>
{
  "text": "Called customer, will follow up tomorrow"
}
```

## Database Models

### User
```javascript
{
  email: String (unique, required),
  password: String (hashed with bcrypt),
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
  notes: [
    {
      text: String,
      createdAt: Date
    }
  ],
  createdAt: Date
}
```

## Key Features

### Security
- ✅ Password hashing with bcrypt (10 salt rounds)
- ✅ JWT authentication (7 day expiration)
- ✅ Protected routes with middleware
- ✅ CORS enabled
- ✅ Environment variable configuration

### API
- ✅ RESTful design
- ✅ JSON responses
- ✅ Error handling
- ✅ Input validation

### Database
- ✅ Mongoose schema validation
- ✅ Automatic timestamps
- ✅ Email uniqueness constraints
- ✅ Enum validation

## Development

### File Organization

**Models** (`server/models/`)
- Define database schemas
- Add validation methods
- Include pre/post hooks

**Controllers** (`server/controllers/`)
- Handle business logic
- Validate input
- Return responses

**Routes** (`server/routes/`)
- Define API endpoints
- Apply middleware
- Call controllers

**Middleware** (`server/middleware/`)
- Verify JWT tokens
- Handle authentication
- Error handling

### Adding a New Feature

1. Create model in `models/`
2. Create controller in `controllers/`
3. Create routes in `routes/`
4. Add routes to `index.js`

Example:
```javascript
// models/Product.js
const productSchema = new mongoose.Schema({
  name: String,
  price: Number
});

// controllers/productController.js
exports.getProducts = async (req, res) => {
  const products = await Product.find();
  res.json({ success: true, data: products });
};

// routes/productRoutes.js
router.get('/', protect, getProducts);

// index.js
app.use('/api/products', require('./routes/productRoutes'));
```

## Testing

### Using cURL

**Register:**
```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@test.com","password":"pass123"}'
```

**Login:**
```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@test.com","password":"pass123"}'
```

**Get Leads:**
```bash
curl -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  http://localhost:5000/api/leads
```

### Using Postman

1. Import API endpoints
2. Set `Authorization` header with token
3. Test each endpoint
4. Check responses

## Troubleshooting

### MongoDB Connection Error
```
Error: connect ECONNREFUSED 127.0.0.1:27017
```
**Solution:** Start MongoDB - `mongod`

### JWT Token Invalid
```
Error: Not authorized to access this route
```
**Solution:** 
- Verify token in Authorization header
- Check token format: `Bearer <token>`
- Login again to get new token

### Port Already in Use
```
Error: listen EADDRINUSE :::5000
```
**Solution:** Change PORT in `.env` or kill process on port 5000

### CORS Error
```
Access to XMLHttpRequest blocked by CORS policy
```
**Solution:** CORS is enabled by default, check frontend URL

## Performance Tips

1. **Database Indexing:**
   ```javascript
   // In models
   email: { type: String, index: true }
   ```

2. **Query Optimization:**
   ```javascript
   // Use .lean() for read-only queries
   const leads = await Lead.find().lean();
   ```

3. **Caching:**
   - Implement Redis for frequently accessed data
   - Cache authentication checks

4. **Pagination:**
   ```javascript
   leads = await Lead.find()
     .skip((page - 1) * limit)
     .limit(limit);
   ```

## Production Deployment

### Environment Variables (Production)
```env
NODE_ENV=production
MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/db
JWT_SECRET=very_long_random_string_min_32_chars
JWT_EXPIRE=7d
PORT=5000
```

### Deployment Platforms
- **Heroku** - Git-based deployment
- **Railway** - Modern alternative to Heroku
- **Render** - Free and paid options
- **DigitalOcean** - VPS hosting
- **AWS** - Elastic Beanstalk or EC2

### Security Checklist
- ✅ Change JWT_SECRET
- ✅ Enable HTTPS/SSL
- ✅ Set NODE_ENV to production
- ✅ Use strong database password
- ✅ Enable rate limiting
- ✅ Add request logging
- ✅ Monitor error logs
- ✅ Backup database regularly

## Dependencies

- **express** - Web framework
- **mongoose** - MongoDB ORM
- **bcrypt** - Password hashing
- **jsonwebtoken** - JWT handling
- **cors** - Cross-origin requests
- **dotenv** - Environment variables
- **nodemon** - Development auto-restart

## Scripts

```bash
npm start          # Run server
npm run dev        # Run with auto-reload
npm test           # Run tests (if configured)
```

## Learning Resources

- [Express.js Guide](https://expressjs.com/)
- [Mongoose Documentation](https://mongoosejs.com/)
- [JWT Explained](https://jwt.io/introduction)
- [RESTful API Best Practices](https://restfulapi.net/)
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)

## Common Patterns

### Protected Route
```javascript
// Routes
router.get('/', protect, getAllLeads);

// Middleware checks JWT automatically
```

### Error Response
```javascript
res.status(400).json({
  success: false,
  message: 'Error description'
});
```

### Success Response
```javascript
res.status(200).json({
  success: true,
  data: results
});
```

## Contributing

1. Follow existing code style
2. Add comments for complex logic
3. Test changes before committing
4. Update documentation
5. Follow RESTful conventions

## Support

- Full project docs: [../README.md](../README.md)
- Setup guide: [../SETUP_GUIDE.md](../SETUP_GUIDE.md)
- API documentation: [../API_DOCUMENTATION.md](../API_DOCUMENTATION.md)

---

Happy coding! 🚀
