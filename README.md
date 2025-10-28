# Automated Installation of Riven, Plex (Optional), and Zurg with rclone
[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/W7W517M4V1)

This script simplifies the installation of Riven and provides optional setups for Plex Media Server and Zurg, integrated with rclone. For Windows users, it also automatically installs WSL (Windows Subsystem for Linux) and Docker if not already present.

**Now support both traditional root and rootless Docker installations**

## Prerequisites

- You will need your **Real-Debrid API token** to integrate Real-Debrid with Riven and Zurg. To obtain your token, visit [Real-Debrid API Token](https://real-debrid.com/apitoken) and generate it.

## Installation Instructions

### Option 1: Automated Installation (Linux/Windows)

#### Step 1: Prepare the Directory
- Place all the provided bash scripts in the directory where you intend to run your Docker containers.

#### Step 2: Start the Installation
- Follow the appropriate steps for your operating system, ensuring you use **sudo/admin privileges** during the process.

#### Step 3: Configure Plex Media Library
Once Zurg and rclone have been initiated and the necessary mounts have been set up, the script will automatically create two important directories for your Plex Media Server:
- `/mnt/library/movies`: This folder will store your movie collection.
- `/mnt/library/shows`: This folder will store your TV shows.

#### **This should be configured after the script went through Plex Setup.**

#### Linux Systems:
1. Open a terminal with the necessary permissions.
2. Start the installation by running:
    ```bash
    sudo bash ./main_setup.sh
    ```

#### Windows Systems:
1. The script will automatically install WSL if it's not already on your system.
2. **Important**: Open **Terminal** as an **Administrator**.
3. In WSL, start the installation by running:
    ```bash
    ./windows_install.bat
    ```
4. If WSL was installed during this process, set up a username and password when prompted. After WSL starts, type `exit` to continue. Use the newly created password when needed.
5. **Troubleshooting Tip**: If you're unable to access the Riven site after installation, run:
    ```bash
    .\windows_proxy.bat
    ```

### windows_proxy.bat: Proxy WSL Docker Bridge IP to Machine IP
The `windows_proxy.bat` script is designed to simplify access to Docker containers running inside WSL by binding the Docker network's bridge (NAT) IP (typically in the 172.x.x.x range) to your Windows machine's IP address.

This allows any service running inside Docker containers, including Riven, Plex, or other applications, to be accessed using your machine's IP address rather than the internal Docker IP. This makes the containers easily reachable from outside the WSL environment without complex networking setups.

Running `windows_proxy.bat` will ensure that any ports exposed by Docker containers in WSL will be proxied to your machine's IP, providing seamless access to all containerized services.

#### NOTE
- During installation, you'll have the option to install Plex Media Server.
- Zurg and rclone will be automatically installed **only if they are not present** in the directory. If they are already installed, the script will skip their installation.


```

### Setup Instructions for Synology

1. **Create Directory Structure**: 
   - Create a folder on your Synology NAS for the Riven setup (e.g., `/volume1/docker/riven`)
   - Create subdirectories: `data`, `rivenfrontend`

2. **Configure Environment Variables**:
   - Update `ORIGIN` in both riven and riven-frontend services with your actual Synology IP or domain
   - Update `BACKEND_URL` in riven-frontend with your Synology IP
   - Modify `PUID` and `PGID` to match your Synology user (typically 1026 and 101 for first admin user)
   - Update timezone (`TZ`) as needed
   - Generate a secure `AUTH_SECRET` for the frontend
   - Set your `BACKEND_API_KEY`

3. **Volume Mappings**:
   - Adjust volume paths to match your Synology directory structure
   - Ensure `/dev/fuse` device is available for VFS functionality
   - Update rclone cache and media paths as needed

4. **Network Access**:
   - Riven backend will be available on port 8083
   - Frontend will be available on port 3000
   - PostgreSQL will be available on port 5433

5. **Start the Services**:
   ```bash
   docker-compose up -d
   ```

### Important Notes for Synology Setup:

- **Privileged Mode**: The riven container requires privileged mode for FUSE mounting
- **Shared Memory**: Configured with 4GB shared memory for optimal performance
- **Memory Limits**: Set to 6666M, adjust based on your NAS capabilities
- **PostgreSQL**: Optional database service for advanced setups
- **Security**: Uses apparmor:unconfined for container permissions

### Troubleshooting:

- If containers fail to start, check Synology Docker logs
- Ensure all volume paths exist and have correct permissions
- Verify network connectivity between containers
- For VFS issues, ensure FUSE support is enabled on your Synology

This setup provides a complete Riven media management solution optimized for Synology NAS environments with Real-Debrid integration.
