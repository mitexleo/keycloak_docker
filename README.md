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

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request on GitHub.

## Acknowledgments

- Keycloak
- Postgres
- Nginx
- Certbot
