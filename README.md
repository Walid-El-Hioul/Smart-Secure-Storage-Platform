# Secure Smart Storage Platform (SSSP)

The Secure Smart Storage Platform (SSSP) is a centralized, secure environment for storing and managing sensitive business data. The system implements advanced security measures including Role-Based Access Control (RBAC) and the Least Privilege principle to ensure that users can only access the data they need to perform their specific job functions.

This repository contains both server and client components of the SSSP platform.

## Components

### Server Component

The SSSP Server provides the backend infrastructure, API endpoints, and security enforcement mechanisms.

#### Server Features

- **Centralized Data Management**: Securely store and organize sensitive business data with appropriate security classifications
- **Advanced Access Control**: Role-Based Access Control (RBAC) with predefined roles and granular permissions
- **Least Privilege Implementation**: Users are granted minimal permissions required for their job function
- **Secure File Operations**: Encrypted file storage, transfer, and integrity verification
- **Security Monitoring**: Detailed security event logging and audit trails
- **API Interface**: RESTful API for all operations with FastAPI

### Client Component

The SSSP Client is a command-line interface (CLI) application that allows users to interact with the SSSP Server.

#### Client Features

- **Role-based access control**: Access different features based on assigned role
- **Secure file management**: Upload, download, and manage files securely
- **Directory permission management**: Set and modify access permissions
- **Comprehensive security events logging**: View and monitor security events
- **SSL certificate management**: Manage secure connections to the server
- **Command-line interface**: Streamlined workflows for all operations

## System Requirements

### Server Requirements

- Linux-based operating system (Ubuntu/Debian recommended)
- Python 3.8+
- MySQL 8.0+
- Root or sudo access for installation
- Recommended: 2GB RAM, 2GB storage space

### Client Requirements

- Linux-based operating system (Ubuntu/Debian recommended)
- Python 3.6+
- Internet connection to reach the server
- Minimal system resources required

## Installation

### Server Installation

1. Download the latest server release from GitHub:

   ```bash
   wget https://github.com/Walid-El-Hioul/SSSP/releases/download/v1.0.0/sssp-server-1.0.0.tar.gz
   ```

2. Extract the archive:

   ```bash
   tar -xzvf sssp-server-1.0.0.tar.gz
   ```

3. Make the installation script executable:

   ```bash
   cd SSSP_Server
   chmod +x ./scripts/installer.sh
   ```

4. Run the installation script with sudo:

   ```bash
   sudo ./scripts/installer.sh
   ```

5. Follow the prompts to configure your installation. You'll need to provide:
   - Storage directory path
   - SSL/HTTPS settings (recommended)
   - Domain name
   - Email notification settings
   - MySQL database credentials

### Client Installation

1. Download the latest client release from GitHub:

   ```bash
   wget https://github.com/Walid-El-Hioul/SSSP/releases/download/v1.0.0/sssp-client-1.0.0.tar.gz
   ```

2. Extract the archive:

   ```bash
   tar -xzvf sssp-client-1.0.0.tar.gz
   ```

3. Make the installation script executable:

   ```bash
   cd SSSP_Client_CLI
   chmod +x ./scripts/installer.sh
   ```

4. Run the installation script with sudo:

   ```bash
   sudo ./scripts/installer.sh
   ```

5. Follow the prompts to specify the server URL and certificate path.

## Server Management

### Starting the Service

```bash
sudo systemctl start sssp
```

### Stopping the Service

```bash
sudo systemctl stop sssp
```

### Restarting the Service

```bash
sudo systemctl restart sssp
```

### Checking Service Status

```bash
sudo systemctl status sssp
```

### Viewing Logs

```bash
# View main application log
sudo tail -f /var/log/sssp/sssp.log

# View error log
sudo tail -f /var/log/sssp/sssp-error.log
```

### Enabling Auto-start at Boot

```bash
sudo systemctl enable sssp
```

### Disabling Auto-start at Boot

```bash
sudo systemctl disable sssp
```

## Client Usage

The SSSP client is designed to be used as an interactive command-line interface (CLI).

### Starting the Client

To start the SSSP client, use the `sssp` command:

```bash
sssp
```

This will launch the interactive CLI interface where you can enter commands.

### Specifying a Server

By default, the client connects to the server URL specified in your configuration file. If you need to connect to a different server, you can use the `--server` (or `-s`) option:

```bash
sssp --server https://different-server:8443
```

### Interactive Usage

Once the client is running, you'll see a prompt:

```bash
sssp:username>
```

Here, you can enter commands to interact with the SSSP server. For example:

```bash
sssp:username> help
```

This will display a list of available commands that you can use.

### Available Commands

Commands are entered at the interactive prompt (not as command-line arguments). Some common commands include:

#### General Commands

- `help` - Show available commands
- `exit` - Exit the CLI
- `whoami` - Display current user information
- `logout` - Log out and clear saved credentials
- `clear` - Clear the terminal screen

#### File Management

- `list_files --directory_id <id>` - List files in a specific directory
- `upload_file <file_path> --directory_id <id>` - Upload a file to a directory
- `download_file <file_id> --output <path>` - Download a file
- `delete_file <file_id>` - Delete a file

#### Directory Management

- `list_directories` - List available directories
- `create_directory <name> --parent_id <id>` - Create a new directory
- `delete_directory <id>` - Delete a directory

#### Permission Management

- `list_permissions` - List available permissions
- `user_permissions <user_id>` - View a user's permissions
- `grant_permission <user_id> <permission>` - Grant a permission to a user
- `revoke_permission <user_id> <permission>` - Revoke a permission from a user

#### Security Monitoring

- `view_events` - View security events log
- `send_alert <message>` - Send security alert to administrators

### Command Examples

Here are some example commands that can be run in the interactive mode:

```bash
sssp:username> list_files --directory_id users
sssp:username> upload_file /path/to/file.txt --directory_id docs
sssp:username> download_file abc123 --output /path/to/save.txt
sssp:username> list_permissions
```

## Configuration

### Server Configuration

The main server configuration file is located at `/opt/sssp/config/config.yaml`. After installation, you can modify configuration settings by editing this file:

```bash
sudo nano /opt/sssp/config/config.yaml
```

Important configuration sections:

- `security`: JWT token settings
- `database`: MySQL connection details
- `email`: Email notification settings
- `paths`: Storage location and SSL paths
- `server`: Port settings

After changing the configuration, restart the service:

```bash
sudo systemctl restart sssp
```

### Client Configuration

The client default configuration file path is stored in `/opt/sssp_client/config/config.yaml`. You can modify it with any text editor:

```bash
nano /opt/sssp_client/config/config.yaml
```

Key settings include:

- Server URL
- Certificate path
- Installation directory

## SSL Certificate Management

### Server Certificates

By default, the server installer creates a self-signed SSL certificate. For production use, you may want to replace it with a valid certificate from a trusted authority.

To replace the certificates:

1. Place your new certificate and key files in the SSL directory:

   ```bash
   sudo cp your-cert.crt /etc/sssp/ssl/server.crt
   sudo cp your-key.key /etc/sssp/ssl/server.key
   ```

2. Update permissions:

   ```bash
   sudo chown sssp:sssp /etc/sssp/ssl/server.crt /etc/sssp/ssl/server.key
   sudo chmod 640 /etc/sssp/ssl/server.crt /etc/sssp/ssl/server.key
   ```

3. Restart the service:

   ```bash
   sudo systemctl restart sssp
   ```

### Client Certificates

The client needs access to the server's certificate for secure communication. If you change the server certificate, you'll need to update the client configuration file `/opt/sssp_client/config/config.yaml`:

```yaml
server:
  cert_path: /path/to/server.crt
```

- **Note:** Don't use double quote with the `cert_path`

## Uninstallation

### Uninstall Server

To uninstall the SSSP Server:

```bash
chmod +x /opt/sssp/scripts/uninstaller.sh
sudo /opt/sssp/scripts/uninstaller.sh
```

This will remove the application files, service user, and systemd service, but will preserve your data and database. To completely remove all data, add the `--purge` flag.

### Uninstall Client

To uninstall the SSSP Client:

```bash
chmod +x /opt/sssp_client/scripts/uninstaller.sh
sudo /opt/sssp_client/scripts/uninstaller.sh
```

## Security Recommendations

1. Use a proper SSL certificate for production
2. Change the default MySQL password
3. Set up a firewall to restrict access to the server
4. Regularly update both server and client components
5. Monitor security logs regularly

## Support

For additional support or to report issues, please visit:

- GitHub Issues: <https://github.com/Walid-El-Hioul/SSSP/issues>
- Documentation: <https://github.com/Walid-El-Hioul/SSSP>

## License

This project is licensed under a proprietary license. See the [LICENSE](./LICENSE) file for details.

Copyright (c) 2025 Secure Smart Storage Platform (SSSP)
All rights reserved.
