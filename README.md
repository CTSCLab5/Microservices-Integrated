# Microservices-Integrated

A complete microservices architecture implementation using Spring Boot, Docker, and API Gateway pattern.

##  Project Overview

This project demonstrates a microservices architecture with four independent services orchestrated through Docker Compose:

- **API Gateway**: Routes requests to appropriate microservices
- **Item Service**: Manages item-related operations
- **Order Service**: Handles order processing
- **Payment Service**: Processes payment transactions

##  Architecture

```
Client Request
      ↓
API Gateway (Port 8080)
      ↓
   ┌──┴──┬──────────┐
   ↓     ↓          ↓
Item  Order    Payment
8081  8082      8083
```

##  Services

| Service | Port | Description |
|---------|------|-------------|
| API Gateway | 8080 | Entry point for all API requests |
| Item Service | 8081 | Manages inventory and item data |
| Order Service | 8082 | Handles order creation and management |
| Payment Service | 8083 | Processes payments and transactions |

##  Prerequisites

- Docker Desktop installed and running
- Docker Compose
- At least 4GB of available RAM
- Ports 8080-8083 available

##  Getting Started

### 1. Clone the Repository

```bash
git clone <repository-url>
cd Microservices-Integrated
```

### 2. Build and Run

```bash
# Build and start all services
docker-compose up --build

# Or run in detached mode
docker-compose up -d
```

### 3. Verify Services

```bash
# Check running containers
docker-compose ps

# View logs
docker-compose logs -f

# View logs for specific service
docker-compose logs -f api-gateway
```

##  Testing

### Using Postman

1. Import the collection: `Microservices_API_Collection.postman_collection.json`
2. Refer to `POSTMAN_TESTING_GUIDE.md` for detailed testing instructions
3. All requests should be sent through the API Gateway at `http://localhost:8080`

### Health Check Endpoints

- API Gateway: `http://localhost:8080/actuator/health`
- Item Service: `http://localhost:8081/actuator/health`
- Order Service: `http://localhost:8082/actuator/health`
- Payment Service: `http://localhost:8083/actuator/health`

##  Stopping Services

```bash
# Stop all services
docker-compose down

# Stop and remove volumes
docker-compose down -v
```

##  Troubleshooting

### Container Name Conflicts

If you see "container name already in use" errors:
```bash
docker-compose down
docker-compose up -d
```

### Port Already in Use

Check if ports 8080-8083 are available:
```bash
# Windows PowerShell
netstat -ano | findstr "8080"
netstat -ano | findstr "8081"
netstat -ano | findstr "8082"
netstat -ano | findstr "8083"
```

### Build Failures

```bash
# Clean rebuild
docker-compose down
docker-compose build --no-cache
docker-compose up
```

### View Container Logs

```bash
# All services
docker-compose logs

# Specific service
docker-compose logs api-gateway
docker-compose logs item-service
docker-compose logs order-service
docker-compose logs payment-service
```

##  Project Structure

```
Microservices-Integrated/
├── docker-compose.yml                    # Container orchestration
├── api-gateway/                          # API Gateway service
│   ├── Dockerfile                        # Multi-stage build
│   ├── pom.xml                          # Maven dependencies
│   └── src/                             # Source code
├── item-service/                         # Item microservice
│   ├── Dockerfile
│   ├── pom.xml
│   └── src/
├── order-sevice/                         # Order microservice
│   ├── Dockerfile
│   ├── pom.xml
│   └── src/
└── payment-service/                      # Payment microservice
    ├── Dockerfile
    ├── pom.xml
    └── src/
```

##  Development Workflow

1. Make changes to service code
2. Rebuild specific service:
   ```bash
   docker-compose build <service-name>
   docker-compose up -d <service-name>
   ```
3. Test the changes
4. View logs if needed

##  Docker Commands Quick Reference

```bash
# Start services
docker-compose up -d

# Stop services
docker-compose down

# Rebuild services
docker-compose build

# View running containers
docker-compose ps

# View logs
docker-compose logs -f

# Restart specific service
docker-compose restart api-gateway

# Remove all containers and networks
docker-compose down --remove-orphans
```

##  Notes

- All services use Java 17 and Spring Boot
- Maven builds are handled inside Docker (multi-stage builds)
- Services communicate through Docker network `microservices-net`
- No need to build JARs locally; Docker handles compilation

##  Contributing

1. Create a feature branch
2. Make your changes
3. Test thoroughly
4. Submit a pull request
