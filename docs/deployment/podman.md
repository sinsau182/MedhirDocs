## Containerization with Podman

### Overview

The project utilizes Podman for containerization, providing a secure, rootless container runtime environment. Podman offers several advantages over Docker, including:
- Rootless operation by default
- No daemon process required
- Native systemd integration
- Better security model

### Prerequisites

- Podman installed on both development and production servers
- Rootless user setup on the server
- Network access between development and production environments
- Sufficient disk space for container images

### Development Environment Setup

1. **Build Container Images**

```bash
# Build frontend image
podman build -t medhir-frontend ./frontend

# Build backend image
podman build -t medhir-backend ./backend
```

2. **Save Images for Transport**

```bash
# Create tar archive of the image
podman save -o medhir.tar medhir
```

3. **Transfer to Production Server**

```bash
# Transfer the image to server's shared directory
scp medhir.tar arunk@192.168.0.200:shared
```

### Production Deployment

#### 1. Image Loading

```bash
# Load the image on the production server
sudo -u podman -i podman load --input ~/shared/medhir-backend.tar
```

#### 2. Container Deployment

Frontend Container:
```bash
sudo -u podman -i podman run -d \
    -p 3000:3000 \
    -e NEXT_PUBLIC_API_URL="http://192.168.0.200:8080" \
    --network shared \
    --name temp-medhir \
    localhost/medhir
```

### Podman Command Reference

#### Basic Syntax
```bash
sudo -u podman -i podman run -d \
    -p <public_PORT>:<server_PORT> \
    -e <ENVIRONMENT_VARIABLES> \
    --network <NETWORK_NAME> \
    --name <CONTAINER_NAME> \
    <IMAGE_NAME>
```

#### Parameter Explanation

- `-d`: Run container in detached mode (background)
- `-p`: Port mapping (host:container)
- `-e`: Environment variables
- `--network`: Network configuration
- `--name`: Container name
- `<IMAGE_NAME>`: Container image to use

### Security Considerations

1. **Rootless Operation**
   - Always use `sudo -u podman -i` for server operations
   - Prevents privilege escalation
   - Enhances security isolation

2. **Network Security**
   - Use dedicated networks for container communication
   - Implement proper firewall rules
   - Restrict container access to necessary ports

3. **Image Security**
   - Use trusted base images
   - Regularly update container images
   - Scan images for vulnerabilities

### Container Management

#### Common Commands

```bash
# List running containers
podman ps

# View container logs
podman logs <container_name>

# Stop container
podman stop <container_name>

# Remove container
podman rm <container_name>

# List images
podman images
```

### Troubleshooting

#### Common Issues

1. **Permission Denied**
   - Ensure proper user permissions
   - Verify podman user configuration
   - Check SELinux contexts

2. **Network Issues**
   - Verify network configuration
   - Check port availability
   - Validate firewall rules

3. **Container Startup Failures**
   - Check container logs
   - Verify environment variables
   - Validate image integrity

### Best Practices

1. **Image Management**
   - Use specific version tags
   - Implement image cleanup policies
   - Regular security updates

2. **Resource Management**
   - Set resource limits
   - Monitor container performance
   - Implement logging rotation

3. **Deployment Strategy**
   - Use deployment scripts
   - Implement rollback procedures
   - Maintain deployment documentation

### Monitoring and Maintenance

1. **Health Checks**
   - Implement container health checks
   - Monitor container status
   - Set up alerts for failures

2. **Logging**
   - Configure proper logging
   - Implement log rotation
   - Set up log aggregation

3. **Updates**
   - Regular image updates
   - Security patch management
   - Dependency updates

---

*Last Updated: [Current Date]*
