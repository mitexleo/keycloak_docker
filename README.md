# Keycloak Deployment with Docker Compose and Nginx

This repository contains the necessary configuration to deploy Keycloak using Docker Compose, with Postgres as the database and Nginx as the reverse proxy.

## Prerequisites

- Docker and Docker Compose installed on your machine.
- A domain name (e.g., `auth.yourdomain.com`) with DNS configured to point to your server's IP address.
- Certbot installed for obtaining SSL certificates.

## Directory Structure

keycloak-deployment/
├── docker-compose.yml
├── nginx/
│   ├── nginx.conf
├── README.md

## Setup Instructions

### 1. Clone the Repository

git clone https://github.com/mitexleo/keycloak-deployment.git
cd keycloak-deployment

### 2. Configure Nginx

Copy the provided Nginx configuration to the appropriate directory on your server.

sudo cp nginx/sites-available/auth.yourdomain.com /etc/nginx/sites-available/
sudo ln -s /etc/nginx/sites-available/auth.yourdomain.com /etc/nginx/sites-enabled/

### 3. Obtain SSL Certificates

Use Certbot to obtain SSL certificates for your domain.

sudo certbot --nginx -d auth.yourdomain.com 

### 4. Start Keycloak and Postgres with Docker Compose

Run the following command to start the Keycloak and Postgres containers.

docker-compose up -d

### 5. Verify the Deployment

Open a web browser and navigate to `https://auth.yourdomain.com`. You should see the Keycloak welcome page.

## Docker Compose Configuration

The `docker-compose.yml` file defines two services: `postgres` and `keycloak`.

### Postgres Service

- **Image**: `postgres:16.2`
- **Environment Variables**:
  - `POSTGRES_DB`: Database name (`keycloak_db`)
  - `POSTGRES_USER`: Database user (`keycloak_db`)
  - `POSTGRES_PASSWORD`: Database user password (`keycloak_db_user_password`)
- **Volumes**: Persistent storage for Postgres data.

### Keycloak Service

- **Image**: `quay.io/keycloak/keycloak:26.0`
- **Environment Variables**:
  - `KC_HTTP_ENABLED`: Enable HTTP (`true`)
  - `KC_HEALTH_ENABLED`: Enable health checks (`true`)
  - `KEYCLOAK_ADMIN`: Admin username (`admin`)
  - `KEYCLOAK_ADMIN_PASSWORD`: Admin password (`jFWQYNU9wr6w3wos`)
  - `KC_DB`: Database type (`postgres`)
  - `KC_DB_URL`: Database URL (`jdbc:postgresql://postgres/keycloak_db`)
  - `KC_DB_USERNAME`: Database user (`keycloak_db`)
  - `KC_DB_PASSWORD`: Database user password (`keycloak_db_user_password`)
  - `KC_HOSTNAME_STRICT`: Strict hostname checking (`false`)
  - `KC_PROXY_HEADERS`: Proxy headers (`xforwarded`)
- **Ports**: Maps port 8080 on the host to port 8080 on the container.
- **Depends On**: Ensures the Postgres service starts before Keycloak.

## Nginx Configuration

The Nginx configuration file sets up a reverse proxy for Keycloak and handles SSL termination.

### Key Features

- **Server Name**: `auth.yourdomain.com`
- **Proxy Settings**: Forwards requests to `http://localhost:8080`
- **SSL Configuration**: Managed by Certbot
- **Timeout Settings**: Increased buffer sizes and timeouts for larger Keycloak responses

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request on GitHub.

## Acknowledgments

- Keycloak
- Postgres
- Nginx
- Certbot
