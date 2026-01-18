# snowpivot_demo
# Running Snowpivot Demo Docker Image

This guide explains how to run Snowpivot using Docker. The Docker image contains a pre-built, ready-to-run version of Snowpivot.

## Prerequisites

- **Docker** installed on your system
  - Download: https://docs.docker.com/get-docker/
  - Verify installation: `docker --version`

## Quick Start

### Option 1: Pull from Docker Registry (Recommended)

If the image is available on a Docker registry (Docker Hub, GitHub Container Registry, etc.):

```bash
# Pull the image
docker pull <registry>/snowpivot:latest

# Run the container
docker run -p 7583:7583 <registry>/snowpivot:latest
```

**Example with Docker Hub:**
```bash
docker pull yourusername/snowpivot:latest
docker run -p 7583:7583 yourusername/snowpivot:latest
```

### Option 2: Load from Image File

If you received a `.tar` or `.tar.gz` file:

```bash
# If compressed, decompress first
gunzip snowpivot-image.tar.gz

# Load the image
docker load -i snowpivot-image.tar

# Run the container
docker run -p 7583:7583 snowpivot:latest
```

## Access the Application

Once the container is running, open your web browser and navigate to:

**http://localhost:7583**

The application should be accessible immediately.

## Running in Background (Detached Mode)

To run Snowpivot in the background:

```bash
docker run -d \
  --name snowpivot \
  -p 7583:7583 \
  --restart unless-stopped \
  <registry>/snowpivot:latest
```

## Managing the Container

### View Logs
```bash
docker logs snowpivot
```

### Follow Logs (Real-time)
```bash
docker logs -f snowpivot
```

### Stop the Container
```bash
docker stop snowpivot
```

### Start the Container
```bash
docker start snowpivot
```

### Remove the Container
```bash
docker stop snowpivot
docker rm snowpivot
```

## Data Persistence

To persist your data (reports, settings, etc.) across container restarts:

```bash
# Create a local data directory
mkdir -p ./snowpivot-data

# Run with volume mount
docker run -d \
  --name snowpivot \
  -p 7583:7583 \
  -v ./snowpivot-data:/app/data \
  -v snowpivot-storage:/app/backend/storage \
  --restart unless-stopped \
  <registry>/snowpivot:latest
```

This will save your data to the `./snowpivot-data` directory on your host machine.

## Using Docker Compose (Recommended for Production)

If you received a `docker-compose.yml` file, you can use it to run Snowpivot:

```bash
# Start the application
docker-compose up -d

# View logs
docker-compose logs -f

# Stop the application
docker-compose down

# Stop and remove volumes (clean slate)
docker-compose down -v
```

## Troubleshooting

### Port Already in Use

If you see an error like `port 7583 is already allocated`:

**Option 1:** Stop the existing container
```bash
docker ps  # Find the container using port 7583
docker stop <container-id>
```

**Option 2:** Use a different port
```bash
docker run -p 8080:7583 <registry>/snowpivot:latest
# Then access at http://localhost:8080
```

**Option 3:** Check what's using the port
```bash
# On Linux/Mac
lsof -i :7583

# On Windows
netstat -ano | findstr :7583
```

### Container Won't Start

1. **Check logs:**
   ```bash
   docker logs snowpivot
   ```

2. **Verify Docker is running:**
   ```bash
   docker ps
   ```

3. **Check image exists:**
   ```bash
   docker images | grep snowpivot
   ```

### Permission Errors

On Linux, if you encounter permission errors:

```bash
# Add your user to the docker group
sudo usermod -aG docker $USER

# Log out and back in for changes to take effect
```

### Out of Disk Space

If Docker runs out of space:

```bash
# Clean up unused images
docker system prune -a

# Remove stopped containers
docker container prune
```

## Advanced Configuration

### Custom Port

Run on a different port:

```bash
docker run -p 9000:7583 <registry>/snowpivot:latest
# Access at http://localhost:9000
```

### Environment Variables

Set custom environment variables:

```bash
docker run -p 7583:7583 \
  -e PYTHONUNBUFFERED=1 \
  -e CUSTOM_VAR=value \
  <registry>/snowpivot:latest
```

### Network Configuration

Run on a specific Docker network:

```bash
# Create a network
docker network create snowpivot-net

# Run container on the network
docker run -p 7583:7583 \
  --network snowpivot-net \
  <registry>/snowpivot:latest
```

## System Requirements

- **Minimum:**
  - 2 GB RAM
  - 1 CPU core
  - 500 MB disk space

- **Recommended:**
  - 4 GB RAM
  - 2 CPU cores
  - 2 GB disk space

## Support

For issues or questions:
- Check the logs: `docker logs snowpivot`
- Review this documentation
- Contact support with the error messages from the logs

## Security Notes

- The Docker image contains obfuscated code for security
- Do not share the image file with unauthorized parties
- Keep Docker and your system updated
- Use strong passwords for any authentication within the application

---

**Version:** This documentation is for Snowpivot Docker image v1.0.0


