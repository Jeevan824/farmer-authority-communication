# Soil Sync Hub - Backend API

A comprehensive Node.js backend API for the Farmer-Authority Communication Platform built with Express.js and MongoDB.

## 🚀 Features

- **JWT Authentication** with role-based access control (Farmer, Authority)
- **MongoDB Models** for Farmers, Authorities, Messages, Advisories, IoT Data, and Notifications
- **RESTful API** with comprehensive endpoints
- **Password Hashing** with bcrypt
- **CORS Support** for frontend integration
- **Input Validation** with express-validator
- **Error Handling** middleware
- **Rate Limiting** for security
- **File Upload** support
- **Mock Data Seeding** for testing

## 📋 Prerequisites

- Node.js (v18 or higher)
- MongoDB (v4.4 or higher)
- npm or yarn

## 🛠️ Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd soil-sync-hub-backend
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Environment Setup**
   ```powershell
   # For Windows
   copy env.example .env
   # For Unix/Linux
   cp env.example .env
   ```
   
   Update the `.env` file with your configuration:
   ```env
   PORT=3001
   NODE_ENV=development
   MONGODB_URI=mongodb://localhost:27017/soil-sync-hub
   JWT_SECRET=your-super-secret-jwt-key
   JWT_EXPIRE=7d
   FRONTEND_URL=http://localhost:8080
   ```

4. **Start MongoDB**
   ```powershell
   # For Windows (if installed as a service)
   net start MongoDB

   # For Windows (if using MongoDB Community Server)
   "C:\Program Files\MongoDB\Server\{version}\bin\mongod.exe" --dbpath="C:\data\db"
   
   # Or using Docker (works on all platforms)
   docker run -d -p 27017:27017 --name mongodb mongo:latest
   ```

5. **Seed the database** (optional)
   ```bash
   npm run seed
   ```

6. **Start the server**
```bash
   # Development
npm run dev

   # Production
   npm start
   ```

## 📚 API Documentation

### Base URL
```
http://localhost:3001/api
```

### Authentication

All protected routes require a Bearer token in the Authorization header:
```
Authorization: Bearer <your-jwt-token>
```

### Endpoints

#### Authentication Routes (`/api/auth`)

| Method | Endpoint | Description | Access |
|--------|----------|-------------|---------|
| POST | `/register/farmer` | Register new farmer | Public |
| POST | `/register/authority` | Register new authority | Public |
| POST | `/login/farmer` | Login farmer | Public |
| POST | `/login/authority` | Login authority | Public |
| POST | `/refresh` | Refresh access token | Public |
| POST | `/logout` | Logout user | Private |
| GET | `/me` | Get current user | Private |
| PUT | `/change-password` | Change password | Private |

#### Farmer Routes (`/api/farmers`)

| Method | Endpoint | Description | Access |
|--------|----------|-------------|---------|
| GET | `/` | Get all farmers | Authority |
| GET | `/:id` | Get farmer by ID | Private |
| PUT | `/:id` | Update farmer profile | Farmer |
| GET | `/:id/statistics` | Get farmer statistics | Private |
| GET | `/:id/iot-data` | Get farmer's IoT data | Private |
| GET | `/:id/messages` | Get farmer's messages | Private |
| GET | `/:id/advisories` | Get farmer's advisories | Private |
| DELETE | `/:id` | Deactivate farmer | Farmer |

#### Message Routes (`/api/messages`)

| Method | Endpoint | Description | Access |
|--------|----------|-------------|---------|
| GET | `/` | Get user's messages | Private |
| GET | `/unread` | Get unread messages | Private |
| GET | `/conversation/:participantId` | Get conversation | Private |
| POST | `/` | Send new message | Private |
| GET | `/:id` | Get message by ID | Private |
| PUT | `/:id/read` | Mark as read | Private |
| PUT | `/:id/reply` | Reply to message | Private |
| PUT | `/:id/archive` | Archive message | Private |
| DELETE | `/:id` | Delete message | Private |

#### Advisory Routes (`/api/advisories`)

| Method | Endpoint | Description | Access |
|--------|----------|-------------|---------|
| GET | `/` | Get advisories | Private |
| GET | `/urgent` | Get urgent advisories | Private |
| POST | `/` | Create advisory | Authority |
| GET | `/:id` | Get advisory by ID | Private |
| PUT | `/:id` | Update advisory | Authority |
| POST | `/:id/send` | Send advisory | Authority |
| POST | `/:id/feedback` | Add feedback | Farmer |
| PUT | `/:id/archive` | Archive advisory | Authority |
| DELETE | `/:id` | Delete advisory | Authority |

#### IoT Routes (`/api/iot`)

| Method | Endpoint | Description | Access |
|--------|----------|-------------|---------|
| GET | `/:farmerId` | Get farmer's IoT data | Private |
| GET | `/:farmerId/latest` | Get latest IoT data | Private |
| GET | `/:farmerId/statistics` | Get IoT statistics | Private |
| GET | `/:farmerId/alerts` | Get IoT alerts | Private |
| POST | `/` | Create IoT data | Private |
| GET | `/data/:id` | Get IoT data by ID | Private |
| PUT | `/data/:id/acknowledge-alert` | Acknowledge alert | Private |
| PUT | `/data/:id/status` | Update sensor status | Private |
| GET | `/sensors/types` | Get sensor types | Private |
| DELETE | `/data/:id` | Delete IoT data | Authority |

#### Notification Routes (`/api/notifications`)

| Method | Endpoint | Description | Access |
|--------|----------|-------------|---------|
| GET | `/` | Get notifications | Private |
| GET | `/unread` | Get unread notifications | Private |
| GET | `/stats` | Get notification stats | Private |
| POST | `/` | Create notification | Authority |
| GET | `/:id` | Get notification by ID | Private |
| PUT | `/:id/read` | Mark as read | Private |
| PUT | `/:id/acknowledge` | Acknowledge notification | Private |
| PUT | `/:id/archive` | Archive notification | Private |
| PUT | `/mark-all-read` | Mark all as read | Private |
| DELETE | `/:id` | Delete notification | Private |
| GET | `/types` | Get notification types | Private |

## 🗄️ Database Models

### Farmer
- Personal information (name, email, phone)
- Address details (village, district, state, pincode)
- Farm details (land area, crops, irrigation)
- IoT sensors configuration
- Preferences and settings

### Authority
- Personal information (name, email, phone)
- Professional details (designation, department)
- Jurisdiction information
- Permissions and access control
- Statistics and activity tracking

### Message
- Sender and recipient information
- Message content and metadata
- Delivery tracking and status
- Attachments support
- Conversation threading

### Advisory
- Content and categorization
- Target audience specification
- Delivery methods and scheduling
- Engagement tracking
- Feedback collection

### IoTData
- Sensor readings and metadata
- Location and quality information
- Alert management
- Status tracking
- Historical data

### Notification
- Multi-channel delivery
- Priority and categorization
- Delivery tracking
- User engagement
- Action buttons

## 🔐 Authentication & Authorization

### JWT Token Structure
```json
{
  "id": "user_id",
  "role": "farmer|authority",
  "farmerId": "FR000001", // for farmers
  "authorityId": "AUTH000001" // for authorities
}
```

### Role-Based Access
- **Farmers**: Can access their own data and send messages
- **Authorities**: Can access all farmers' data and send advisories
- **System**: Can create notifications and manage IoT data

## 🧪 Testing

### Test Credentials (after seeding)
```
Farmers:
- Email: ravi.kumar@email.com, Password: password123
- Email: sunita.devi@email.com, Password: password123
- Email: rajesh.patel@email.com, Password: password123

Authorities:
- Email: rajesh.kumar@agriculture.gov.in, Password: password123
- Email: priya.sharma@agriculture.gov.in, Password: password123
```

### Sample API Calls

**Login as Farmer:**
```powershell
# Windows PowerShell
Invoke-RestMethod -Method Post -Uri "http://localhost:3001/api/auth/login/farmer" `
  -Headers @{"Content-Type"="application/json"} `
  -Body '{"email": "ravi.kumar@email.com", "password": "password123"}'

# Or using curl (if installed)
curl.exe -X POST http://localhost:3001/api/auth/login/farmer `
  -H "Content-Type: application/json" `
  -d "{\"email\": \"ravi.kumar@email.com\", \"password\": \"password123\"}"
```

**Get Farmer's IoT Data:**
```powershell
# Windows PowerShell
Invoke-RestMethod -Method Get -Uri "http://localhost:3001/api/iot/64a1b2c3d4e5f6789012345" `
  -Headers @{"Authorization"="Bearer <your-token>"}

# Or using curl (if installed)
curl.exe -X GET http://localhost:3001/api/iot/64a1b2c3d4e5f6789012345 `
  -H "Authorization: Bearer <your-token>"
```

**Send Message:**
```powershell
# Windows PowerShell
$body = @{
    recipientId = "64a1b2c3d4e5f6789012346"
    subject = "Query about wheat cultivation"
    content = "Need guidance on fertilizer application"
    priority = "medium"
} | ConvertTo-Json

Invoke-RestMethod -Method Post -Uri "http://localhost:3001/api/messages" `
  -Headers @{
    "Authorization"="Bearer <your-token>"
    "Content-Type"="application/json"
  } `
  -Body $body

# Or using curl (if installed)
curl.exe -X POST http://localhost:3001/api/messages `
  -H "Authorization: Bearer <your-token>" `
  -H "Content-Type: application/json" `
  -d "{\"recipientId\":\"64a1b2c3d4e5f6789012346\",\"subject\":\"Query about wheat cultivation\",\"content\":\"Need guidance on fertilizer application\",\"priority\":\"medium\"}"
```

## 🚀 Deployment

### Environment Variables
```env
NODE_ENV=production
PORT=3001
MONGODB_URI=mongodb://your-mongodb-connection-string
JWT_SECRET=your-production-jwt-secret
JWT_EXPIRE=7d
FRONTEND_URL=https://your-frontend-domain.com
```

### Production Setup
1. Set up MongoDB Atlas or self-hosted MongoDB
2. Configure environment variables
3. Install PM2 for process management:
   ```bash
   npm install -g pm2
   pm2 start server.js --name "soil-sync-hub-api"
   ```
4. Set up reverse proxy with Nginx
5. Configure SSL certificates
6. Set up monitoring and logging

## 📊 Monitoring & Logging

- Health check endpoint: `GET /health`
- Request logging with timestamps
- Error tracking and reporting
- Performance monitoring
- Database connection monitoring

## 🔧 Development

### Project Structure
```
├── models/           # MongoDB models
├── routes/           # API route handlers
├── middleware/       # Custom middleware
├── scripts/          # Database seeding scripts
├── server.js         # Application entry point
├── package.json      # Dependencies and scripts
└── README.md         # Documentation
```

### Available Scripts
```bash
npm run dev          # Start development server with nodemon
npm start            # Start production server
npm run seed         # Seed database with sample data
npm test             # Run tests (when implemented)
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License.

## 🆘 Support

For support and questions:
- Create an issue in the repository
- Contact the development team
- Check the API documentation

---

**Built with ❤️ for agricultural communities**