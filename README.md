# MERN Todo App - DevOps CI/CD Project

> A full-stack todo application demonstrating modern DevOps practices with automated deployment pipeline

![Todo App Landing Page](./assets/app-screenshot.png)

**Live Demo:** [https://buildnship.site](https://buildnship.site)

---

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Features](#features)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [DevOps Pipeline](#devops-pipeline)
- [Deployment](#deployment)
- [What I Learned](#what-i-learned)

---

## 🎯 Overview

This project is a production-ready MERN (MongoDB, Express, React, Node.js) todo application deployed on AWS with a fully automated CI/CD pipeline. The main focus is on implementing DevOps best practices including containerization, automated deployments, reverse proxy configuration, and SSL/HTTPS security.

### Key Achievements

✅ **Containerized Architecture** - Multi-container Docker setup with Docker Compose  
✅ **Automated CI/CD** - GitHub Actions pipeline with zero-downtime deployments  
✅ **Container Registry** - GitHub Container Registry (GHCR) for image management  
✅ **Cloud Infrastructure** - AWS EC2 with custom domain configuration  
✅ **Reverse Proxy** - Nginx for routing and load balancing  
✅ **SSL/HTTPS** - Let's Encrypt certificate with auto-renewal  
✅ **DNS Management** - Route 53 hosted zone with proper A records  

---

## 🏗️ Architecture

```mermaid
graph TB
    subgraph User["👤 Developer"]
        Dev[Local Development]
    end
    
    Dev -->|git push| GitHub[GitHub Repository]
    
    subgraph CI["⚙️ GitHub Actions CI/CD"]
        Checkout[1. Checkout Code]
        Build[2. Build Docker Images]
        Push[3. Push to GHCR]
        Deploy[4. SSH & Deploy to EC2]
        
        Checkout --> Build --> Push --> Deploy
    end
    
    GitHub --> Checkout
    
    subgraph GHCR["📦 GHCR"]
        BackendImg["Backend Image<br/>sha-xxxxx"]
        FrontendImg["Frontend Image<br/>sha-xxxxx"]
    end
    
    Push --> GHCR
    
    subgraph AWS["☁️ AWS Cloud - ap-south-1"]
        subgraph Route53["🌐 Route 53"]
            DNS["buildnship.site<br/>→ 13.235.123.121"]
        end
        
        subgraph EC2["🖥️ EC2 Instance - t2.micro - Ubuntu 24.04"]
            subgraph Nginx["🔀 Nginx Reverse Proxy"]
                SSL["SSL/TLS: Let's Encrypt<br/>Port 80/443"]
            end
            
            subgraph Docker["🐳 Docker Compose"]
                Frontend["Frontend Container<br/>React App<br/>Port 3000"]
                Backend["Backend Container<br/>Express API<br/>Port 5000"]
                MongoDB["MongoDB Container<br/>Database<br/>Port 27017"]
            end
            
            Network["Docker Network: todo-network"]
        end
    end
    
    Deploy --> EC2
    GHCR -.->|Pull Images| Docker
    
    DNS --> SSL
    SSL --> Frontend
    SSL --> Backend
    Frontend -.->|API Calls| Backend
    Backend --> MongoDB
    
    Users["🌍 End Users"] -->|HTTPS| DNS
    
    style Users fill:#667eea,stroke:#333,stroke-width:2px,color:#fff
    style AWS fill:#FF9900,stroke:#333,stroke-width:2px,color:#000
    style EC2 fill:#FFA500,stroke:#333,stroke-width:2px
    style GHCR fill:#2088FF,stroke:#333,stroke-width:2px,color:#fff
    style CI fill:#2088FF,stroke:#333,stroke-width:2px,color:#fff
    style Docker fill:#2496ED,stroke:#333,stroke-width:2px,color:#fff
```

### System Flow

1. **Development** → Developer pushes code to GitHub repository
2. **CI/CD Trigger** → GitHub Actions workflow automatically starts
3. **Build Phase** → Docker images built for backend and frontend
4. **Registry** → Images pushed to GitHub Container Registry with Git SHA tags
5. **Deployment** → SSH into EC2, pull new images, restart containers
6. **DNS Resolution** → Route 53 resolves domain to EC2 public IP
7. **SSL Termination** → Nginx handles HTTPS and routes traffic
8. **Application** → Frontend serves UI, Backend provides API, MongoDB stores data

### Data Flow

1. **User Request** → HTTPS request to `buildnship.site`
2. **DNS Resolution** → Route 53 resolves to EC2 IP
3. **SSL Termination** → Nginx handles SSL/TLS
4. **Reverse Proxy** → Nginx routes to appropriate container
   - `/` → Frontend container (port 3000)
   - `/api/*` → Backend container (port 5000)
5. **Backend Processing** → Express API queries MongoDB
6. **Response** → Secure response back to user

---

## 🛠️ Tech Stack

### Frontend
- **React** - UI library
- **Axios** - HTTP client
- **CSS3** - Styling

### Backend
- **Node.js** - Runtime environment
- **Express.js** - Web framework
- **Mongoose** - MongoDB ODM
- **CORS** - Cross-origin resource sharing

### Database
- **MongoDB** - NoSQL database

### DevOps & Infrastructure
- **Docker** - Containerization
- **Docker Compose** - Multi-container orchestration
- **GitHub Actions** - CI/CD pipeline
- **GitHub Container Registry (GHCR)** - Container image storage
- **AWS EC2** - Virtual server hosting
- **AWS Route 53** - DNS management
- **Nginx** - Reverse proxy & web server
- **Let's Encrypt** - SSL/TLS certificates
- **Certbot** - Certificate management

---

## ✨ Features

### Application Features
- ✅ Create new todos
- ✅ Mark todos as complete/incomplete
- ✅ Delete todos
- ✅ Persistent storage with MongoDB
- ✅ Real-time updates
- ✅ Responsive design

### DevOps Features
- ✅ Automated build and deployment
- ✅ Zero-downtime deployments
- ✅ Container-based architecture
- ✅ Multi-platform builds (AMD64 & ARM64)
- ✅ Environment-specific configurations
- ✅ Automated SSL certificate renewal
- ✅ Image versioning with Git SHA tags
- ✅ Automatic cleanup of old images

---

## 📁 Project Structure

```
mern-todo-app-devops/
├── .github/
│   └── workflows/
│       └── deploy.yml          # CI/CD pipeline configuration
├── backend/
│   ├── models/
│   │   └── Todo.js            # Todo schema
│   ├── routes/
│   │   └── todos.js           # API routes
│   ├── server.js              # Express server
│   ├── package.json
│   ├── Dockerfile             # Backend container image
│   └── .dockerignore
├── frontend/
│   ├── src/
│   │   ├── App.js             # Main React component
│   │   └── App.css            # Styles
│   ├── public/
│   ├── package.json
│   ├── Dockerfile             # Frontend container image
│   └── .dockerignore
├── docker-compose.yml         # Multi-container configuration
├── .gitignore
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Node.js (v18+)
- Docker & Docker Compose
- Git

### Local Development

1. **Clone the repository**
   ```bash
   git clone https://github.com/Syed-Moazzam/mern-todo-app-devops.git
   cd mern-todo-app-devops
   ```

2. **Start with Docker Compose**
   ```bash
   # Build and start all services
   docker compose up --build

   # Or run in detached mode
   docker compose up -d
   ```

3. **Access the application**
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:5000/api/todos
   - Health Check: http://localhost:5000/health

4. **Stop the application**
   ```bash
   docker compose down
   ```

### Manual Setup (Without Docker)

**Backend:**
```bash
cd backend
npm install
npm start
```

**Frontend:**
```bash
cd frontend
npm install
npm start
```

**MongoDB:**
- Install MongoDB locally or use MongoDB Atlas
- Update `MONGODB_URI` in backend/.env

---

## 🔄 DevOps Pipeline

### CI/CD Workflow

The deployment pipeline is triggered automatically on every push to the `main` branch:

```yaml
Push to main → Build Images → Push to GHCR → Deploy to EC2
```

### Pipeline Steps

1. **Build Job**
   - Checkout code from repository
   - Login to GitHub Container Registry
   - Build multi-platform Docker images (AMD64, ARM64)
   - Tag images with Git commit SHA
   - Push images to GHCR

2. **Deploy Job**
   - SSH into EC2 instance
   - Generate `.env` with current image tag
   - Pull latest code from GitHub
   - Pull new Docker images from GHCR
   - Stop old containers
   - Start new containers
   - Clean up old images

### GitHub Secrets Required

Configure these secrets in your GitHub repository settings:

- `EC2_HOST` - EC2 instance public IP
- `EC2_SSH_KEY` - Private SSH key for EC2 access
- `GHCR_PAT` - GitHub Personal Access Token (for pulling images)

---

## 🌐 Deployment

### AWS Infrastructure Setup

1. **EC2 Instance**
   - Launch Ubuntu 24.04 LTS instance
   - Instance type: t2.micro (free tier)
   - Region: ap-south-1 (Mumbai)
   - Security groups: Allow ports 22, 80, 443

2. **Domain Configuration**
   - Purchase domain (e.g., buildnship.site)
   - Create Route 53 hosted zone
   - Update nameservers at domain registrar
   - Create A record pointing to EC2 IP

3. **Server Setup**
   ```bash
   # Install Docker
   sudo apt update
   sudo apt install docker.io docker-compose-plugin
   
   # Install Nginx
   sudo apt install nginx
   
   # Install Certbot
   sudo apt install certbot python3-certbot-nginx
   ```

4. **SSL Certificate**
   ```bash
   sudo certbot --nginx -d buildnship.site -d www.buildnship.site
   ```

### Nginx Configuration

Located at `/etc/nginx/sites-available/buildnship.site`:

```nginx
server {
    listen 80;
    server_name buildnship.site www.buildnship.site;
    
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
    
    location /api {
        proxy_pass http://localhost:5000/api;
        # ... proxy headers
    }
}
```

---

## 📚 What I Learned

### Docker & Containerization
- Creating optimized Dockerfiles
- Multi-stage builds for smaller images
- Docker Compose for orchestration
- Container networking
- Volume management for data persistence

### CI/CD Pipeline
- GitHub Actions workflow syntax
- Building multi-platform images
- Container registry management
- SSH-based deployments
- Environment variable handling

### AWS Cloud Services
- EC2 instance management
- Security group configuration
- Route 53 DNS management
- Understanding cloud networking

### Nginx & Web Servers
- Reverse proxy configuration
- SSL/TLS certificate setup
- Request routing
- Let's Encrypt certificate automation

### DevOps Best Practices
- Infrastructure as Code principles
- Automated testing and deployment
- Version control with Git
- Environment-specific configurations
- Zero-downtime deployment strategies

---

## 🔧 Troubleshooting

### Common Issues

**Containers not starting:**
```bash
docker compose logs
docker ps -a
```

**Port already in use:**
```bash
# Find process using port
sudo lsof -i :3000
# Kill the process
kill -9 <PID>
```

**Image pull errors:**
- Verify GHCR packages are public
- Check image names match docker-compose.yml
- Ensure proper GitHub token permissions

**502 Bad Gateway:**
- Check if containers are running: `docker ps`
- Verify Nginx configuration: `sudo nginx -t`
- Check application logs: `docker compose logs`

---

## 📈 Future Enhancements

- [ ] Add automated testing (Jest, React Testing Library)
- [ ] Implement monitoring (Prometheus + Grafana)
- [ ] Set up centralized logging (ELK stack)
- [ ] Add staging environment
- [ ] Implement database backups
- [ ] Migrate to Kubernetes for orchestration
- [ ] Add load balancer for multiple instances
- [ ] Implement blue-green deployments

---

## 🤝 Contributing

This is a learning project, but suggestions and improvements are welcome!

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -am 'Add new feature'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Create a Pull Request

---

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

---

## 👤 Author

**Syed Moazzam Ahmed**

- GitHub: [@Syed-Moazzam](https://github.com/Syed-Moazzam)
- Project Link: [https://github.com/Syed-Moazzam/mern-todo-app-devops](https://github.com/Syed-Moazzam/mern-todo-app-devops)
- Live Demo: [https://buildnship.site](https://buildnship.site)

---

## 🙏 Acknowledgments

- Docker documentation
- AWS documentation
- GitHub Actions documentation
- Nginx documentation
- The DevOps community

---

**⭐ If you found this project helpful, please consider giving it a star!**
