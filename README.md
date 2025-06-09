# 🧠 Mental Health FastAPI Application

A comprehensive **mental health and wellness platform** built with FastAPI, featuring user authentication, achievement tracking, automated goal assignment, and multi-tenant customer management.

## 🌟 **Key Features**

### 🔐 **Enterprise-Grade Authentication System**
- **JWT-based authentication** with access and refresh tokens
- **Multi-layered password security**: Argon2 hashing with cryptographic salts
- **Account security**: Failed login tracking and automatic lockout (5 attempts → 30min lock)
- **Role-based access control**: User, Moderator, Admin, and Super Admin roles
- **Session management**: Token validation and automatic refresh

### 🏢 **Multi-Tenant Customer Management**
- **Organization support**: Multiple companies with isolated user bases
- **Subscription management**: Plan-based user limits and billing
- **Admin hierarchy**: Customer admins can manage their organization's users
- **User capacity control**: Configurable max users per customer

### 🎯 **Intelligent Achievement System**
- **Automated goal assignment**: AI-powered daily, weekly, and monthly goal scheduling
- **Background processing**: APScheduler for automatic goal distribution
- **Achievement categories**: Personal, professional, wellness activities
- **Progress tracking**: Real-time completion status and point accumulation
- **Excel import/export**: Bulk achievement management for administrators

### 📊 **Progress Analytics & Insights**
- **User statistics**: Achievement completion rates and point accumulation
- **Historical tracking**: Date-range filtered activity history
- **Admin dashboards**: Organization-wide performance metrics
- **Goal completion analytics**: Success rates by frequency and category

### 🔧 **Administrative Tools**
- **User management**: Activate, deactivate, promote, and demote users
- **Goal assignment**: Manual goal distribution for individuals or all users
- **System monitoring**: Health checks and scheduler status tracking
- **Account management**: Unlock locked accounts and reset failed attempts

### 🎨 **Modern Web Interface**
- **Responsive design**: Mobile-first UI with gradient themes
- **Interactive dashboards**: Real-time data visualization
- **User-friendly forms**: Achievement creation and profile management
- **Authentication flows**: Login, registration, and profile pages

---

## 🛠 **Technology Stack**

### **Backend Framework**
- **FastAPI 0.115.6**: High-performance async web framework
- **SQLAlchemy 2.0.36**: Modern ORM with relationship mapping
- **Alembic 1.13.1**: Database migration management
- **APScheduler**: Background task scheduling

### **Security & Authentication**
- **Argon2-CFI**: Industry-standard password hashing
- **PyJWT 2.8.0**: JSON Web Token implementation
- **Python-JOSE**: Cryptographic signing and verification
- **Passlib**: Password policy enforcement

### **Database & Storage**
- **SQLite**: Lightweight embedded database (production-ready for PostgreSQL)
- **User-Achievement relationships**: Many-to-many with status tracking
- **Multi-tenant data isolation**: Customer-based data segregation

### **Frontend & Templates**
- **Jinja2 3.1.2**: Server-side template rendering
- **Modern CSS**: Responsive design with gradients and animations
- **JavaScript**: Interactive forms and API integration

### **File Processing**
- **OpenPyXL 3.1.2**: Excel file parsing and generation
- **Background processing**: Async file upload and validation

---

## 📁 **Project Structure**

```
VCSHackathon/
├── app/
│   ├── api/                    # API routes and endpoints
│   │   ├── authentication.py   # JWT auth and security
│   │   ├── achievements.py     # Achievement management
│   │   ├── admin_routes.py     # Administrative functions
│   │   ├── user_routes.py      # User profile management
│   │   ├── customer_routes.py  # Multi-tenant features
│   │   └── page_routes.py      # Web page routing
│   ├── core/
│   │   └── database.py         # Database configuration
│   ├── models/                 # SQLAlchemy database models
│   │   ├── users.py           # User entity with auth fields
│   │   ├── achievements.py    # Achievement and relationships
│   │   └── customer.py        # Multi-tenant customer model
│   ├── schemas/               # Pydantic validation schemas
│   ├── services/              # Business logic layer
│   │   ├── goal_manager.py    # Automated goal assignment
│   │   ├── scheduler.py       # Background task scheduling
│   │   ├── crud.py           # Database operations
│   │   └── archievements_import.py # Excel processing
│   ├── templates/             # HTML templates
│   │   ├── dashboard.html     # User dashboard
│   │   ├── admin.html         # Admin panel
│   │   ├── achievements.html  # Achievement tracking
│   │   └── login.html         # Authentication forms
│   ├── static/               # CSS, JS, and images
│   └── main.py               # FastAPI application entry
├── alembic/                  # Database migrations
├── requirements.txt          # Python dependencies
└── README.md                # This documentation
```

---

## 🚀 **Quick Start Guide**

### **1. Installation**

```bash
# Clone the repository
git clone <repository-url>
cd VCSHackathon

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### **2. Database Setup**

```bash
# Initialize database
alembic upgrade head

# Or create tables directly
python -c "from app.core.database import create_tables; create_tables()"
```

### **3. Run the Application**

```bash
# Start the development server
uvicorn app.main:app --reload

# Access the application
# API Documentation: http://localhost:8000/docs
# Web Interface: http://localhost:8000/achievements
# Admin Panel: http://localhost:8000/admin
```

---

## 📡 **API Endpoints**

### **Authentication**
- `POST /api/v1/auth/register` - User registration
- `POST /api/v1/auth/login` - User authentication
- `GET /api/v1/auth/me` - Current user profile

### **Achievement Management**
- `GET /api/v1/achievements/` - Get user's current goals
- `POST /api/v1/achievements/upload` - Upload Excel achievements
- `POST /api/v1/achievements/preview` - Preview Excel data
- `POST /api/v1/achievements/{id}/complete` - Mark achievement complete

### **Administrative**
- `POST /api/v1/admin/goals/assign-daily-all` - Assign daily goals to all users
- `POST /api/v1/admin/users/{id}/activate` - Activate user account
- `GET /api/v1/admin/stats` - Organization statistics
- `POST /api/v1/admin/achievements/create` - Create new achievement

### **User Management**
- `GET /api/v1/users/` - List users (admin only)
- `PUT /api/v1/users/{id}` - Update user profile
- `POST /api/v1/users/{id}/unlock` - Unlock locked account

### **System Health**
- `GET /health` - Application health check
- `GET /scheduler/status` - Background scheduler status

---

## 🔒 **Security Features**

### **Password Policy**
- Minimum 12 characters (configurable for testing)
- Required: Uppercase, lowercase, digit, special character
- Pattern validation prevents common weak passwords
- Argon2 hashing with unique salts per user

### **Account Protection**
- **Failed login tracking**: Automatic account lockout
- **JWT token security**: Short-lived access tokens with refresh mechanism
- **Role-based permissions**: Granular access control by user role
- **Input validation**: SQL injection and XSS prevention

### **Multi-Tenant Security**
- **Data isolation**: Customer-based data segregation
- **Admin scope limitation**: Admins can only manage their organization
- **Subscription enforcement**: User limit validation

---

## 🤖 **Automated Features**

### **Background Scheduler**
- **Daily Goals**: 5 random achievements assigned at midnight
- **Weekly Goals**: 3 achievements assigned every Monday
- **Monthly Goals**: 2 achievements assigned on 1st of month
- **Cleanup Process**: Expired goals removed automatically

### **Achievement Assignment Logic**
- **New User Onboarding**: Immediate goal assignment upon registration
- **Random Selection**: Diverse goal distribution to prevent repetition
- **Frequency-based**: Daily/Weekly/Monthly categorization
- **Due Date Management**: Automatic deadline calculation

---

## 📊 **Data Models**

### **User Entity**
```python
class User:
    id: int
    username: str
    email: str
    password_hash: str
    salt: str
    full_name: str
    role: str  # user, moderator, admin, super_admin
    customer_id: int
    is_active: bool
    failed_login_attempts: int
    locked_until: datetime
    last_login: datetime
    created_at: datetime
```

### **Achievement Entity**
```python
class Achievement:
    id: int
    title: str
    description: str
    point_value: int
    duration: int
    frequency: str  # daily, weekly, monthly
    created_at: datetime
```

### **User-Achievement Relationship**
```python
class UserAchievement:
    user_id: int
    achievement_id: int
    status: str  # pending, completed, failed
    due_date: datetime
    created_at: datetime
```

---

## 🎯 **Usage Examples**

### **Register a New User**
```bash
curl -X POST "http://localhost:8000/api/v1/auth/register" \
-H "Content-Type: application/json" \
-d '{
  "username": "john_doe",
  "email": "john@example.com",
  "full_name": "John Doe",
  "password": "SecurePass123!",
  "role": "user"
}'
```

### **Upload Achievements Excel**
```bash
curl -X POST "http://localhost:8000/api/v1/achievements/upload" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-F "file=@achievements.xlsx"
```

### **Get Current User Goals**
```bash
curl -X GET "http://localhost:8000/api/v1/achievements/" \
-H "Authorization: Bearer YOUR_JWT_TOKEN"
```

---

## 🧪 **Testing**

### **Development Testing**
- **Password Policy**: Currently set to 6 characters for testing
- **Sample Data**: 21 mental health achievements pre-loaded
- **API Testing**: Use `/docs` for interactive testing

### **Production Preparation**
1. **Restore password requirements** to 12+ characters
2. **Configure environment variables** for JWT secrets
3. **Switch to PostgreSQL** for production database
4. **Set up SSL/TLS** for secure communication

---

## 🔧 **Configuration**

### **Environment Variables**
```bash
# JWT Configuration
JWT_SECRET_KEY=your-super-secret-key
JWT_ALGORITHM=HS256
JWT_ACCESS_TOKEN_EXPIRE_MINUTES=30

# Database
DATABASE_URL=sqlite:///./app.db

# Password Policy (Production)
MIN_PASSWORD_LENGTH=12
MAX_PASSWORD_LENGTH=128
```

### **Scheduler Configuration**
- **Daily Goals**: 00:01 every day
- **Weekly Goals**: 00:01 every Monday
- **Monthly Goals**: 00:01 on 1st of month
- **Cleanup**: 23:59 every day

---

## 🎨 **UI Features**

### **Responsive Design**
- **Mobile-first**: Optimized for mobile devices
- **Modern aesthetics**: Gradient backgrounds and smooth animations
- **Interactive elements**: Hover effects and form validation
- **Accessibility**: Proper contrast and keyboard navigation

### **User Dashboard**
- **Achievement progress**: Visual progress bars and completion status
- **Point tracking**: Real-time score updates
- **Goal overview**: Current daily, weekly, and monthly objectives
- **History tracking**: Past achievements and completion dates

---

## 📈 **Performance Features**

### **Database Optimization**
- **Indexed queries**: Optimized for user and achievement lookups
- **Relationship mapping**: Efficient many-to-many queries
- **Connection pooling**: SQLAlchemy session management

### **Background Processing**
- **Async operations**: Non-blocking file uploads and processing
- **Task scheduling**: Efficient resource utilization
- **Error handling**: Robust failure recovery and logging

---

## 🔮 **Future Enhancements**

### **Planned Features**
- **Real-time notifications**: WebSocket integration for live updates
- **Social features**: User groups and achievement sharing
- **Analytics dashboard**: Advanced reporting and insights
- **Mobile app**: React Native or Flutter companion app
- **AI recommendations**: Machine learning-based goal suggestions

### **Technical Improvements**
- **Containerization**: Docker deployment setup
- **Monitoring**: Application performance monitoring
- **Testing suite**: Comprehensive unit and integration tests
- **CI/CD pipeline**: Automated deployment and testing

---

## 📞 **Support & Contributing**

### **Getting Help**
- **API Documentation**: Available at `/docs` endpoint
- **Health Check**: Monitor system status at `/health`
- **Scheduler Status**: View background jobs at `/scheduler/status`

### **Development**
- **Code Structure**: Follow existing patterns in `/api`, `/services`, `/models`
- **Database Changes**: Use Alembic migrations for schema updates
- **Testing**: Test authentication and protected endpoints thoroughly

---

**Built with ❤️ for mental health and wellness** 