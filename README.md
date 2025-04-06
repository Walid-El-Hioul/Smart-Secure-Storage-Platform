# Secure Smart Storage Platform (SSSP) - Server

The Secure Smart Storage Platform (SSSP) is a centralized, secure environment for storing and managing sensitive business data. The system implements advanced security measures including Role-Based Access Control (RBAC) and the Least Privilege principle to ensure that users can only access the data they need to perform their specific job functions.

## Features

- **Centralized Data Management**: Securely store and organize sensitive business data with appropriate security classifications
- **Advanced Access Control**: Role-Based Access Control (RBAC) with predefined roles and granular permissions
- **Least Privilege Implementation**: Users are granted minimal permissions required for their job function
- **Secure File Operations**: Encrypted file storage, transfer, and integrity verification
- **Security Monitoring**: Detailed security event logging and audit trails
- **API Interface**: RESTful API for all operations with FastAPI

## System Requirements

- Linux-based operating system (Ubuntu/Debian recommended)
- Python 3.8+
- MySQL 8.0+
- Root or sudo access for installation
- Recommended: 2GB RAM, 2GB storage space

## Installation

1. Download the latest release from GitHub:

   ```bash
   wget https://github.com/Walid-El-Hioul/SSSP/releases
   ```

2. Extract the archive:

   ```bash
   tar -xzvf sssp-server.tar.gz
   ```

3. Make the installation script executable:

   ```bash
   cd sssp-server
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

## Configuration

The main configuration file is located at `/opt/sssp/config/config.yaml`. After installation, you can modify configuration settings by editing this file:

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

## SSL Certificate Management

By default, the installer creates a self-signed SSL certificate. For production use, you may want to replace it with a valid certificate from a trusted authority.

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

## Uninstallation

To uninstall the SSSP Server:

```bash
chmod +x /opt/sssp/scripts/uninstaller.sh
sudo /opt/sssp/scripts/uninstaller.sh
```

This will remove the application files, service user, and systemd service, but will preserve your data and database. To completely remove all data, add the `--purge` flag.

## Troubleshooting

### Service Won't Start

1. Check the logs:

   ```bash
   sudo journalctl -u sssp.service -n 50
   ```

2. Verify database connection:

   ```bash
   sudo -u sssp mysql -u sssp_user -p -e "SELECT 1" sssp_db
   ```

3. Check file permissions:

   ```bash
   ls -la /opt/sssp
   ls -la /etc/sssp/ssl
   ls -la /var/log/sssp
   ```

### SSL Issues

If you're having problems with SSL:

1. Verify certificate paths in configuration:

   ```bash
   grep ssl /opt/sssp/config/config.yaml
   ```

2. Check certificate validity:

   ```bash
   openssl x509 -in /etc/sssp/ssl/server.crt -text -noout
   ```

3. Try temporarily disabling SSL by modifying the systemd service file.

### Database Connection Issues

1. Verify MySQL is running:

   ```bash
   sudo systemctl status mysql
   ```

2. Check database credentials in config.yaml
3. Try to connect manually to test credentials

## Security Recommendations

1. Use a proper SSL certificate for production
2. Change the default MySQL password
3. Set up a firewall to restrict access to the server

## Support

For additional support or to report issues, please visit:

- GitHub Issues: <https://github.com/Walid-El-Hioul/SSSP/issues>
- Documentation: <https://github.com/Walid-El-Hioul/SSSP>

## License

This project is licensed under the terms of the [LICENSE](./LICENSE) file.
