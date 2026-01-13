# 🚀 n8n with PostgreSQL, Redis & Workers

A **production-ready, scalable n8n setup** with PostgreSQL database, Redis queue system, and multiple workers for horizontal scaling. Perfect for testing n8n's enterprise capabilities!

## 🌟 What Makes This Special?

- **📊 PostgreSQL**: Reliable database for workflow persistence
- **⚡ Redis**: High-performance queue system for job distribution  
- **🏗️ Multiple Workers**: Scale horizontally by adding more workers
- **🌐 External Access**: Ngrok integration for secure HTTPS access
- **🔧 Production Ready**: Based on enterprise architecture patterns

## 🎯 Perfect For

- Testing n8n scaling capabilities
- Learning distributed system architecture
- Development before cloud deployment
- High-availability workflow processing

---

## 🚀 Quick Start (5 Minutes)

### Prerequisites
- [Docker](https://docker.com) installed
- [Ngrok](https://ngrok.com/download) account

### Step 1: Setup Environment
```bash
# Copy the environment template
cp .env.example .env

# Edit the .env file with your settings
# - Change N8N_ENCRYPTION_KEY to something secure
# - Update passwords if desired
# - Verify your Ngrok domain
```

### Step 2: Start Ngrok Tunnel
```bash
# This creates secure HTTPS access to your local n8n
ngrok http --domain=your-domain.ngrok-free.app 5679
```

### Step 3: Launch n8n Stack
```bash
# Pull latest images and start all services
docker compose pull
docker compose up -d
```

### Step 4: Access n8n
- **URL**: `https://your-domain.ngrok-free.app`
- **Username**: `admin` (or your custom username)
- **Password**: Your configured password

---

## 🏗️ Architecture Overview

```
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│   n8n UI    │    │   PostgreSQL │    │    Redis    │
│   (Port:    │◄──►│   Database   │◄──►│   Queue     │
│   5679)     │    │  (Port:54322)│    │  System     │
└─────────────┘    └──────────────┘    └─────────────┘
       ▲                   ▲                   ▲
       │                   │                   │
       └───────────────────┼───────────────────┘
                           │
       ┌───────────────────┼───────────────────┐
       │                   │                   │
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ n8n Worker  │    │ n8n Worker  │    │  Add More   │
│     #1      │    │     #2      │    │  Workers!   │
└─────────────┘    └─────────────┘    └─────────────┘
```

---

## 📋 Services Included

| Service | Purpose | Port | Access |
|---------|---------|------|--------|
| **n8n** | Main web interface | 5679 | HTTPS via Ngrok |
| **n8n-worker-1** | Workflow processor | - | Internal |
| **n8n-worker-2** | Workflow processor | - | Internal |
| **postgres** | Database | 54322 | localhost |
| **redis** | Queue system | - | Internal |

---

## 🔧 Scaling Your Setup

### Add More Workers
Edit `compose.yaml` and add:
```yaml
n8n-worker-3:
  <<: *shared
  image: n8nio/n8n
  command: worker
  depends_on:
    - n8n
```

Then restart:
```bash
docker compose up -d
```

### Monitor Performance
```bash
# Check all services status
docker compose ps

# Monitor resource usage
docker stats

# View real-time logs
docker compose logs -f
```

---

## 🗄️ Database Access

Connect directly to PostgreSQL:
- **Host**: localhost
- **Port**: 54322
- **Database**: n8n
- **Username**: n8n_user
- **Password**: (from your .env file)

---

## 🛠️ Management Commands

### Start Services
```bash
docker compose pull    # Get latest images
docker compose up -d   # Start in background
```

### Stop Services
```bash
docker compose down    # Stop and remove containers
```

### View Logs
```bash
docker compose logs -f           # All services
docker compose logs -f n8n       # Main n8n only
docker compose logs -f n8n-worker-1  # Specific worker
```

### Update n8n
```bash
docker compose pull
docker compose up -d
```

---

## 🌐 Cloud Deployment

Ready for production? See `cloud.txt` for a complete guide to deploy this setup to:
- Google Cloud Platform
- AWS EC2  
- DigitalOcean
- And more!

The cloud guide includes:
- SSL certificate setup
- Domain configuration
- Production optimizations
- Backup strategies
- Monitoring setup

---

## 🆘 Troubleshooting

### n8n Not Accessible
1. Check all containers are running: `docker compose ps`
2. Verify Ngrok is active and using port 5679
3. Check logs: `docker compose logs n8n`

### Workers Not Processing Jobs
1. Check Redis status: `docker compose logs redis`
2. Verify worker logs: `docker compose logs n8n-worker-1`
3. Restart services: `docker compose restart`

### Database Issues
1. Check PostgreSQL logs: `docker compose logs postgres`
2. Verify connection settings in .env file
3. Ensure database was created properly

---

## 💡 Pro Tips

1. **Security**: Change default passwords before production use
2. **Backups**: Data persists in Docker volumes, but backup regularly
3. **Performance**: Monitor RAM usage and add workers as needed
4. **Updates**: Keep n8n updated for latest features and security
5. **Scaling**: This architecture can handle 1000+ concurrent workflows

---

## 📚 Learn More

- [n8n Official Documentation](https://docs.n8n.io)
- [Docker Compose Documentation](https://docs.docker.com/compose)
- [PostgreSQL Documentation](https://www.postgresql.org/docs)
- [Redis Documentation](https://redis.io/documentation)

---

## 🤝 Contributing

Found this helpful? ⭐ **Star this repository** and consider contributing improvements!

---

## 📄 License

This project is open source and available under the MIT License.

## 🙏 Acknowledgments

This project is inspired by and based on the original work by [mateusrovedaa]([https://github.com/mateusrovedaa/n8n-postgres-redis-workers.git]). 
Their foundation provided the starting point for this enhanced version with additional features:

- Enhanced documentation for beginners
- Cloud deployment guide
- Production-ready configurations
- Comprehensive troubleshooting section

Thank you to the original author for their contribution to the n8n community!
