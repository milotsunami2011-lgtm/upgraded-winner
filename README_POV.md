# Emergency Services Driver POV Recording Platform

A comprehensive web platform for viewing, managing, and streaming driver point-of-view (POV) recordings from emergency services vehicles including police cars, ambulances, and fire trucks.

## 🎯 Features

- **POV Video Player**: Responsive HTML5 video player for streaming recordings
- **Database Management**: PostgreSQL database for storing video metadata
- **Video Streaming**: Direct streaming support with view analytics
- **Job-Based Organization**: Videos organized by emergency service type and vehicle
- **User Authentication**: JWT-based authentication with role-based access
- **Admin Dashboard**: Manage videos, vehicles, and users
- **Metadata Tracking**: Date, location, duration, incident type, and more

## 📋 Tech Stack

### Backend
- **Framework**: FastAPI (Python)
- **Database**: PostgreSQL
- **Authentication**: JWT
- **Async Tasks**: Celery (video processing)
- **Caching**: Redis

### Frontend
- **Framework**: React with TypeScript
- **UI Library**: Material-UI
- **State Management**: Zustand
- **HTTP Client**: Axios

### Infrastructure
- **Containerization**: Docker & Docker Compose
- **Reverse Proxy**: Nginx
- **Orchestration**: Docker Compose

## 🚀 Quick Start

### Prerequisites
- Docker & Docker Compose
- Git

### Installation

1. **Clone and setup:**
```bash
git clone https://github.com/milotsunami2011-lgtm/upgraded-winner.git
cd upgraded-winner
cp .env.example .env
```

2. **Start services:**
```bash
docker-compose up -d
```

3. **Access the application:**
- Frontend: http://localhost:3000
- Backend API: http://localhost:8000
- API Docs: http://localhost:8000/docs

4. **Create admin user (optional):**
```bash
docker-compose exec backend python -c "
from app.database import SessionLocal
from app.models import User
from app.security import hash_password

db = SessionLocal()
admin = User(
    username='admin',
    email='admin@example.com',
    hashed_password=hash_password('admin123'),
    is_admin=True,
    is_active=True
)
db.add(admin)
db.commit()
print('Admin user created: admin / admin123')
"
```

## 📁 Project Structure

```
emergency-services-pov/
├── backend/                 # Python FastAPI backend
│   ├── app/
│   │   ├── main.py         # FastAPI application
│   │   ├── config.py       # Configuration
│   │   ├── models.py       # Database models
│   │   ├── schemas.py      # Request/response schemas
│   │   ├── security.py     # JWT authentication
│   │   └── routers/        # API endpoints
│   ├── requirements.txt
│   └── Dockerfile
├── frontend/                # React TypeScript frontend
│   ├── src/
│   │   ├── pages/          # Page components
│   │   ├── components/     # Reusable components
│   │   ├── services/       # API client
│   │   ├── store/          # State management
│   │   └── App.tsx
│   ├── package.json
│   └── Dockerfile
├── docs/                    # Documentation
├── docker-compose.yml       # Multi-container setup
├── nginx.conf              # Nginx configuration
├── .env.example            # Environment template
└── README.md               # This file
```

## 🎬 Usage

### Login
1. Navigate to http://localhost:3000
2. Register a new account or login
3. Browse POV recordings

### Upload Video
1. Click "Upload" in navigation
2. Select vehicle and fill in details
3. Upload video file
4. Video processing begins

### Filter Videos
- Filter by vehicle type (Police, Ambulance, Fire Truck)
- Search by title or description
- Sort by date or views

## 🔌 API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user
- `GET /api/auth/me` - Get current user

### Videos
- `GET /api/videos` - List videos
- `POST /api/videos` - Create video metadata
- `GET /api/videos/{id}` - Get video details
- `PUT /api/videos/{id}` - Update video
- `DELETE /api/videos/{id}` - Delete video
- `POST /api/videos/{id}/upload` - Upload video file
- `GET /api/videos/{id}/stream` - Stream video

### Vehicles
- `GET /api/vehicles` - List vehicles
- `POST /api/vehicles` - Create vehicle (admin)
- `GET /api/vehicles/{id}` - Get vehicle details
- `PUT /api/vehicles/{id}` - Update vehicle (admin)
- `DELETE /api/vehicles/{id}` - Delete vehicle (admin)

### Users
- `GET /api/users` - List users (admin)
- `GET /api/users/{id}` - Get user details
- `PUT /api/users/me` - Update current user
- `DELETE /api/users/{id}` - Delete user (admin)

## 🛠️ Development

### Backend Development
```bash
cd backend
python -m venv venv
source venv/bin/activate  # or `venv\Scripts\activate` on Windows
pip install -r requirements.txt
uvicorn app.main:app --reload
```

### Frontend Development
```bash
cd frontend
npm install
npm start
```

## 🐳 Docker Commands

```bash
# Start services
docker-compose up -d

# Stop services
docker-compose down

# View logs
docker-compose logs -f backend
docker-compose logs -f frontend

# Rebuild containers
docker-compose up -d --build

# Remove everything including volumes
docker-compose down -v
```

## 📚 Documentation

- [Setup Guide](./docs/SETUP.md) - Detailed installation instructions
- [Architecture](./docs/ARCHITECTURE.md) - System architecture overview
- [API Reference](./docs/API.md) - Complete API documentation

## 🔐 Security Features

- JWT-based authentication
- bcrypt password hashing
- CORS configuration
- Role-based access control
- File upload validation
- SQL injection prevention (via SQLAlchemy ORM)

## 📊 Database Schema

### Users Table
- id, username, email, hashed_password, full_name, is_active, is_admin

### Vehicles Table
- id, name, vehicle_type, license_plate, unit_number, department, location

### Videos Table
- id, title, description, vehicle_id, uploader_id, duration, file_path, file_size, format, recording_date, location, incident_type, is_processed, is_public, view_count

### VideoViews Table
- id, video_id, user_id, ip_address, watch_duration

## 🚨 Vehicle Types

- **POLICE**: Police car recordings
- **AMBULANCE**: Ambulance/EMS recordings
- **FIRE_TRUCK**: Fire truck recordings

## 🌐 Deployment

For production deployment:

1. Update `.env` with production values
2. Use proper SSL certificates
3. Configure database backups
4. Set up monitoring and logging
5. Use managed services (e.g., AWS RDS for database)

See [Deployment Guide](./docs/DEPLOYMENT.md) for detailed instructions.

## 📝 License

MIT License - See LICENSE file for details

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Open a Pull Request

## ❓ Troubleshooting

### Port 3000/8000 already in use
```bash
# Change ports in docker-compose.yml or:
sudo lsof -i :3000  # Find process using port 3000
sudo kill -9 <PID>  # Kill the process
```

### Database connection errors
```bash
# Check database container
docker-compose logs db

# Verify database is running
docker-compose ps
```

### Video upload fails
- Check available disk space
- Verify file permissions in /app/videos
- Check MAX_VIDEO_SIZE in .env

## 📞 Support

For issues and questions:
- Check existing issues on GitHub
- Review documentation in /docs folder
- Create a new GitHub issue with details

## 🎓 Learning Resources

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [React Documentation](https://react.dev/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [Docker Documentation](https://docs.docker.com/)

---

**Last Updated**: 2026-07-07
