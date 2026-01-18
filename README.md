# Running Snowpivot Docker Image

This guide explains how to run Snowpivot using the Docker image tar file.

## Prerequisites

### Installing Docker Desktop

Docker Desktop is the easiest way to run Docker on your computer. Follow these steps:

#### For macOS

1. **Download Docker Desktop**
   - Visit: https://www.docker.com/products/docker-desktop/
   - Click "Download for Mac"
   - Choose the version for your Mac (Intel or Apple Silicon/M1/M2)

2. **Install Docker Desktop**
   - Open the downloaded `.dmg` file
   - Drag Docker to your Applications folder
   - Open Docker from Applications
   - Follow the setup wizard
   - You may need to enter your password to install system components

3. **Verify Installation**
   - Docker Desktop should appear in your menu bar (whale icon)
   - Open Terminal and run: `docker --version`
   - You should see something like: `Docker version 24.x.x`

#### For Windows

1. **Download Docker Desktop**
   - Visit: https://www.docker.com/products/docker-desktop/
   - Click "Download for Windows"
   - Download the installer

2. **Install Docker Desktop**
   - Run the installer (`Docker Desktop Installer.exe`)
   - Follow the installation wizard
   - You may need to enable WSL 2 (Windows Subsystem for Linux) if prompted
   - Restart your computer if required

3. **Verify Installation**
   - Launch Docker Desktop from the Start menu
   - Wait for Docker to start (whale icon in system tray)
   - Open Command Prompt or PowerShell and run: `docker --version`

#### For Linux

1. **Install Docker Engine**
   - Visit: https://docs.docker.com/engine/install/
   - Follow the instructions for your Linux distribution (Ubuntu, Debian, Fedora, etc.)

2. **Verify Installation**
   - Run: `docker --version`

## Quick Start

You can load and run the Docker image using either the **GUI method** (recommended for beginners) or the **command line method**.

### Method 1: Using Docker Desktop GUI (Recommended)

#### Step 1: Load the Docker Image

First, you need to load the image using the command line (this only needs to be done once):

```bash
docker load -i snowpivot-docker-image.tar
```

After loading, the image will appear in Docker Desktop's Images tab as `snowpivot:latest`.

#### Step 2: Run the Container

1. **Start the Container**
   - In Docker Desktop, go to the **Containers** tab
   - Click **Run** (or the play button) next to `snowpivot:latest`
   - In the dialog that appears:
     - **Container name**: Enter `snowpivot` (optional)
     - **Port mapping**: Set `7583:7583` (host port:container port)
     - Click **Run**

2. **Access the Application**
   - The container should start and show as "Running" in Docker Desktop
   - Open your browser and go to: **http://localhost:7583**

#### Managing the Container via GUI

- **View Logs**: Click on the container name, then click the **Logs** tab
- **Stop Container**: Click the **Stop** button (square icon)
- **Start Container**: Click the **Start** button (play icon)
- **Remove Container**: Click the **Delete** button (trash icon)

### Method 2: Using Command Line

#### Step 1: Load the Docker Image

If you have the `snowpivot-docker-image.tar` file:

```bash
# Load the image
docker load -i snowpivot-docker-image.tar
```

This will load the image as `snowpivot:latest`.

#### Step 2: Run the Container

```bash
# Run the container
docker run -p 7583:7583 snowpivot:latest
```

The application will be available at **http://localhost:7583**

## Running in the Background

### Using Docker Desktop GUI

When you run a container from Docker Desktop, it automatically runs in the background. The container will continue running even if you close the Docker Desktop window (as long as Docker Desktop is still running in the background).

### Using Command Line

To run the container in detached mode (background):

```bash
docker run -d -p 7583:7583 --name snowpivot snowpivot:latest
```

### Managing the Container

#### Using Docker Desktop GUI

- **View running containers**: Go to the **Containers** tab
- **View logs**: Click on the container name, then click the **Logs** tab
- **Stop container**: Click the **Stop** button
- **Start container**: Click the **Start** button
- **Remove container**: Click the **Delete** button

#### Using Command Line

```bash
# View running containers
docker ps

# View logs
docker logs snowpivot

# Stop the container
docker stop snowpivot

# Start a stopped container
docker start snowpivot

# Remove the container
docker rm snowpivot
```

## Custom Port

Run on a different port:

```bash
docker run -p 9000:7583 snowpivot:latest
# Access at http://localhost:9000
```

## Persistent Data Storage

To persist data between container restarts, you need to mount a volume.

### Using Docker Desktop GUI

1. When running the container, click **Optional settings** or **Advanced options**
2. Under **Volumes**, click **Bind mount**
3. Set:
   - **Host path**: Choose a folder on your computer (e.g., `C:\snowpivot-data` on Windows or `~/snowpivot-data` on Mac/Linux)
   - **Container path**: `/app/data`
4. Click **Run**

### Using Command Line

```bash
docker run -d -p 7583:7583 \
  -v $(pwd)/snowpivot-data:/app/data \
  --name snowpivot \
  snowpivot:latest
```

This will store all reports, settings, and data in the `snowpivot-data` directory in your current folder.

## Environment Variables

Set custom environment variables:

```bash
docker run -p 7583:7583 \
  -e PYTHONUNBUFFERED=1 \
  -e CUSTOM_VAR=value \
  snowpivot:latest
```

## Network Configuration

Run on a specific Docker network:

```bash
# Create a network
docker network create snowpivot-net

# Run container on the network
docker run -p 7583:7583 \
  --network snowpivot-net \
  --name snowpivot \
  snowpivot:latest
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

## Troubleshooting

### Docker Desktop Not Running

If you get errors like "Cannot connect to Docker daemon" or "Docker daemon is not running":

- **Mac/Windows**: Make sure Docker Desktop is running (check for the whale icon in menu bar/system tray)
- **Linux**: Make sure Docker service is running: `sudo systemctl start docker`

### Check Container Status

**Using Docker Desktop GUI:**
- Go to the **Containers** tab to see all containers and their status

**Using Command Line:**
```bash
docker ps -a | grep snowpivot
```

### View Logs

**Using Docker Desktop GUI:**
- Click on the container name, then click the **Logs** tab

**Using Command Line:**
```bash
docker logs snowpivot
```

### Check if Port is Available

```bash
# On Linux/Mac
lsof -i :7583

# On Windows
netstat -ano | findstr :7583
```

### Restart the Container

```bash
docker restart snowpivot
```

## Security Notes

- The Docker image contains obfuscated code for security
- Do not share the image file with unauthorized parties
- Keep Docker and your system updated
- Use strong passwords for any authentication within the application

## Demo Version Limitations

This demo version is limited to:
- **1 report**
- **1 data connection**
- **1 email connection**

Please delete existing items before creating new ones.

---

**Version:** This documentation is for Snowpivot Docker image v1.0.0


