# Server API Documentation

Express backend for the CRM application.

## Setup

```bash
npm install
npm start      # Production
npm run dev    # Development (with nodemon)
```

Runs on: `http://localhost:5000`

## Project Structure

```
server/
├── config/
│   └── db.js              # MongoDB connection
├── models/
│   ├── User.js            # User schema
│   └── Lead.js            # Lead schema
├── controllers/
│   ├── authController.js  # Auth logic
│   └── leadController.js  # Lead logic
├── routes/
│   ├── authRoutes.js      # Auth endpoints
│   └── leadRoutes.js      # Lead endpoints
├── middleware/
│   └── authMiddleware.js  # JWT verification
├── utils/
│   └── generateToken.js   # JWT token generation
├── index.js               # Server entry point
├── .env                   # Environment variables
└── package.json
```

## Environment Variables

Create `.env` file:

```env
MONGODB_URI=mongodb://localhost:27017/simple-crm
JWT_SECRET=your_super_secret_key_min_32_chars_recommended
JWT_EXPIRE=7d
PORT=5000
```

### MongoDB Connection Examples

**Local:**
```
MONGODB_URI=mongodb://localhost:27017/simple-crm
```

**Atlas Cloud:**
```
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/simple-crm?retryWrites=true&w=majority
```

## API Endpoints

### Base URL
```
http://localhost:5000/api
```

### Authentication Routes

#### Register
```
POST /auth/register
Content-Type: application/json

{
  "email": "admin@example.com",
  "password": "password123"
}

Response (201):
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

#### Login
```
POST /auth/login
Content-Type: application/json

{
  "email": "admin@example.com",
  "password": "password123"
}

Response (200):
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### Lead Routes (All require JWT token)

Add token to header:
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

#### Get All Leads
```
GET /leads

Response (200):
{
  "success": true,
  "count": 5,
  "data": [
    {
      "_id": "507f1f77bcf86cd799439011",
      "name": "John Doe",
      "email": "john@example.com",
      "phone": "+1234567890",
      "source": "website form",
      "status": "new",
      "notes": [],
      "createdAt": "2024-01-15T10:30:00Z"
    }
  ]
}
```

#### Get Single Lead
```
GET /leads/:id

Response (200):
{
  "success": true,
  "data": {
    "_id": "507f1f77bcf86cd799439011",
    "name": "John Doe",
    "email": "john@example.com",
    "phone": "+1234567890",
    "source": "website form",
    "status": "new",
    "notes": [
      {
        "text": "Called, no answer",
        "createdAt": "2024-01-15T11:00:00Z"
      }
    ],
    "createdAt": "2024-01-15T10:30:00Z"
  }
}
```

#### Create Lead
```
POST /leads
Content-Type: application/json

{
  "name": "Jane Smith",
  "email": "jane@example.com",
  "phone": "+9876543210",
  "source": "referral",
  "status": "new"
}

Response (201):
{
  "success": true,
  "data": {
    "_id": "507f1f77bcf86cd799439012",
    "name": "Jane Smith",
    "email": "jane@example.com",
    "phone": "+9876543210",
    "source": "referral",
    "status": "new",
    "notes": [],
    "createdAt": "2024-01-15T12:00:00Z"
  }
}
```

#### Update Lead
```
PUT /leads/:id
Content-Type: application/json

{
  "name": "Jane Updated",
  "status": "contacted",
  "phone": "+1111111111"
}

Response (200):
{
  "success": true,
  "data": {
    "_id": "507f1f77bcf86cd799439012",
    "name": "Jane Updated",
    "email": "jane@example.com",
    "phone": "+1111111111",
    "source": "referral",
    "status": "contacted",
    "notes": [],
    "createdAt": "2024-01-15T12:00:00Z"
  }
}
```

#### Delete Lead
```
DELETE /leads/:id

Response (200):
{
  "success": true,
  "message": "Lead deleted",
  "data": {
    "_id": "507f1f77bcf86cd799439012",
    "name": "Jane Smith",
    "email": "jane@example.com"
  }
}
```

#### Add Note to Lead
```
POST /leads/:id/notes
Content-Type: application/json

{
  "text": "Follow-up email sent"
}

Response (200):
{
  "success": true,
  "data": {
    "_id": "507f1f77bcf86cd799439011",
    "name": "John Doe",
    "email": "john@example.com",
    "phone": "+1234567890",
    "source": "website form",
    "status": "new",
    "notes": [
      {
        "text": "Called, no answer",
        "createdAt": "2024-01-15T11:00:00Z"
      },
      {
        "text": "Follow-up email sent",
        "createdAt": "2024-01-15T14:30:00Z"
      }
    ],
    "createdAt": "2024-01-15T10:30:00Z"
  }
}
```

### Health Check
```
GET /api/health

Response (200):
{
  "success": true,
  "message": "Server is running"
}
```

## Error Responses

### 400 Bad Request
```json
{
  "success": false,
  "message": "Please provide name and email"
}
```

### 401 Unauthorized
```json
{
  "success": false,
  "message": "Not authorized to access this route"
}
```

### 404 Not Found
```json
{
  "success": false,
  "message": "Lead not found"
}
```

### 500 Server Error
```json
{
  "success": false,
  "message": "Error message here"
}
```

## Database Models

### User
```javascript
{
  email: String (unique, required),
  password: String (hashed, required),
  createdAt: Date (default: now)
}
```

**Password Hashing:** Uses bcrypt with 10 salt rounds

### Lead
```javascript
{
  name: String (required),
  email: String (required),
  phone: String (optional),
  source: String (enum: 'website form', 'referral', 'manual', default: 'manual'),
  status: String (enum: 'new', 'contacted', 'converted', default: 'new'),
  notes: [
    {
      text: String (required),
      createdAt: Date (default: now)
    }
  ],
  createdAt: Date (default: now, sorted desc)
}
```

## Testing with cURL

### Register
```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@example.com","password":"pass123"}'
```

### Login
```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@example.com","password":"pass123"}'
```

### Get Leads (replace TOKEN with actual token)
```bash
curl -H "Authorization: Bearer TOKEN" \
  http://localhost:5000/api/leads
```

### Create Lead
```bash
curl -X POST http://localhost:5000/api/leads \
  -H "Authorization: Bearer TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name":"John","email":"john@example.com"}'
```

## Security Features

- ✅ **Password Hashing:** bcrypt (10 rounds)
- ✅ **JWT Authentication:** Tokens expire after 7 days
- ✅ **Protected Routes:** All lead endpoints require valid JWT
- ✅ **CORS Enabled:** Safe cross-origin requests
- ✅ **Environment Variables:** Secrets not in code
- ⚠️ **TODO:** Rate limiting for production
- ⚠️ **TODO:** Input validation & sanitization

## Middleware

### authMiddleware
- Verifies JWT token in Authorization header
- Attaches user to request object
- Returns 401 if token invalid/missing

## Performance Tips

1. Add pagination for large result sets
2. Implement caching (Redis)
3. Add database indexing on frequently queried fields
4. Use request compression
5. Implement rate limiting

## Deployment

### Environment Setup
```env
NODE_ENV=production
MONGODB_URI=your_mongodb_url
JWT_SECRET=very_long_random_secret_string
JWT_EXPIRE=7d
PORT=5000
```

### Build for Production
```bash
npm install --production
```

### Run on Production Server
```bash
npm start
```

## Dependencies

- **express** - Web framework
- **mongoose** - MongoDB ORM
- **bcrypt** - Password hashing
- **jsonwebtoken** - JWT handling
- **cors** - Cross-origin requests
- **dotenv** - Environment variables
- **nodemon** - Auto-restart (dev)

## Troubleshooting

### MongoDB Connection Failed
- Ensure MongoDB is running
- Check connection string in .env
- Verify network/firewall access

### JWT Token Errors
- Regenerate token (login again)
- Check JWT_SECRET is set and consistent
- Verify token in Authorization header format

### CORS Errors
- Ensure CORS middleware is enabled
- Frontend URL is allowed in CORS configuration
- Frontend must send with credentials if needed

---

See [../SETUP_GUIDE.md](../SETUP_GUIDE.md) for complete project documentation.
