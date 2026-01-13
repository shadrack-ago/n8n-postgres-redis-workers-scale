# n8n with PostgreSQL, Redis and Workers

This setup provides a scalable n8n configuration with:
- PostgreSQL database for persistence
- Redis for queue management
- Multiple n8n workers for horizontal scaling
- Ngrok integration for external access

## Quick Setup

1. **Copy environment file:**
   ```bash
   cp .env.example .env
   ```

2. **Edit the .env file:**
   - Update `N8N_ENCRYPTION_KEY` with a secure random string
   - Change passwords if desired
   - Verify your Ngrok domain is correct

3. **Start Ngrok tunnel:**
   ```bash
   ngrok http --domain=devoted-arriving-dogfish.ngrok-free.app 5678
   ```

4. **Start the services:**
   ```bash
   docker compose up -d
   ```

## Services

- **n8n**: Main n8n instance (port 5678)
- **n8n-worker-1**: First worker instance
- **n8n-worker-2**: Second worker instance  
- **postgres**: PostgreSQL database (port 54322)
- **redis**: Redis for queue management

## Scaling Workers

To add more workers, add new services to `compose.yaml`:

```yaml
n8n-worker-3:
  <<: *shared
  image: n8nio/n8n
  command: worker
  depends_on:
    - n8n
```

## Accessing n8n

- URL: https://devoted-arriving-dogfish.ngrok-free.app
- Username: admin
- Password: Meshackago

## Database Access

- Host: localhost
- Port: 54322
- Database: n8n
- User: n8n_user
- Password: n8n_password_123

## Stopping Services

```bash
docker compose down
```

## Monitoring

Check logs for all services:
```bash
docker compose logs -f
```

Check specific service logs:
```bash
docker compose logs -f n8n
docker compose logs -f n8n-worker-1
```
