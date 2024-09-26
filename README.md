# Project Name

This project consists of a PHP Laravel API and a Nuxt.js client, both containerized using Docker and orchestrated with Docker Compose. The project also includes an Nginx server acting as a reverse proxy and a MySQL database for the Laravel API.

## Features

- PHP Laravel API (Backend)
- Nuxt.js Client (Frontend)
- MySQL Database
- Nginx Reverse Proxy with SSL (Self-signed certificate for HTTPS)

## Prerequisites

Make sure you have the following installed on your machine:

- Docker: [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)
- Docker Compose: [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

## Step-by-Step Instructions

### 1. Clone the Repository

```
git clone https://github.com/Nourann5/Challenge.git
cd Challenge
```

### 2. Build and Run Containers

Run the following command to build the Docker images and start the services:

bash
docker-compose up --build


This command will:

- Set up a MySQL database service.
- Build and run the PHP Laravel API container.
- Build and run the Nuxt.js Client container.
- Set up the Nginx server as a reverse proxy.

### 3. Access the Application

Once the services are up and running, you can access the Nuxt.js application through the Nginx reverse proxy:

- Open your /etc/hosts and list 127.0.0.1 as book.example.com
- Then open your browser and go to https://books.example.com .

### 4. Services Overview

#### Laravel API (Backend)

The API service runs inside a container with the following setup:
- PHP 8.2-cli-buster
- Composer for managing dependencies
- Connects to a MySQL database

#### Nuxt.js Client (Frontend)

The client service runs inside a container and serves the Nuxt.js frontend application.

#### MySQL Database

A MySQL container for storing the API's data.

#### Nginx Server (Reverse Proxy)

Nginx is set up as a reverse proxy, forwarding HTTP/HTTPS traffic to the client container.

#### SSL Certificate 

- A self-signed SSL certificate has been generated for the Nginx server, making it accessible over HTTPS on port 443.
- The following steps shows how i created it:
1- Make sure you have OpenSSL installed , check by using :
   ```
   openssl version
   ```
  If it’s not installed, you can install it using your package manager, ex for UBUNTU :
   ```
   sudo apt update
   sudo apt install openssl
   ```

2- Generate a Private Key:
   ```
   openssl genrsa -out wildcard.example.com.key 2048
   ```

3- Create a Certificate Signing Request (CSR):
   ```
   openssl req -new -key wildcard.example.com.key -out wildcard.example.com.csr
   ```
  (And Specified `*.example.com` when asked for Common Name)

4- Creating the Self-Signed Certificate:
   ```
   openssl x509 -req -days 365 -in wildcard.example.com.csr -signkey wildcard.example.com.key -out wildcard.example.com.crt
   ```

5- Verify the Certificate:
   ```
   openssl x509 -in wildcard.example.com.crt -text -noout
   ```

6- Configure Nginx to use the self-signed certificate in `nginx.conf` file by adding ssl_certificate & ssl_certificate_key paths.


### 5. CI/CD with GitHub Actions

This repository also includes a GitHub Actions workflow for CI/CD:
- On each push to the main branch with updating any of /api or /client folders, Docker images for the API and Client are built and pushed to Docker Hub.
- You can configure your Docker Hub credentials by setting the DOCKER_USERNAME and DOCKER_PASSWORD secrets in your repository.

### 6. Shutting Down the Services

To stop and remove the running containers, run:

```
docker-compose down
```

### 7. Troubleshooting

If you encounter issues with the containers or services, try rebuilding the images:

```
docker-compose up --build --force-recreate
```

You can also view the logs for each service by running:

```
docker-compose logs -f
```

### 8. Folder Structure

```
├── .github/workflows/   # GithubActions For CI/CD 
│
├── api/                # PHP Laravel API
│   ├── Dockerfile
├── client/             # Nuxt.js Client
│   ├── Dockerfile
├── cert/              # SSL certificate
│   ├── nginx.crt      # Self-signed certificate
│   ├── nginx.key      # Private key
│
├── nginx.conf          # Nginx Configurations
├── docker-compose.yml  # Docker Compose file to orchestrate services
└── README.md           # Project documentation
```


## Screenshots

Screenshots here to show the application up and running with Docker Compose and the application being accessed through the Nginx Proxy:


![Screenshot 2024-09-27 004320](https://github.com/user-attachments/assets/3fcb3b99-bf52-49ea-a624-7600e21c168c)





![Screenshot 2024-09-27 005333](https://github.com/user-attachments/assets/a1e8db04-4a69-4d93-ae9e-5099206d6ede)





![Screenshot_1](https://github.com/user-attachments/assets/90b2c4ce-a44d-435d-921d-26b0f5362a57)





![Screenshot_3](https://github.com/user-attachments/assets/5ce28837-3483-48d7-bfc8-d6b99b3560b9)





![Screenshot 2024-09-27 013202](https://github.com/user-attachments/assets/406f75fc-7885-4c7c-9b6c-54d3ab938170)



### Example Docker Compose Commands

These to demonstrate the commands users will frequently run:


#### Start all services (with build)
```
docker-compose up --build
```

#### Stop and remove containers
```
docker-compose down
```

#### View logs for a service (e.g., nginx)
```
docker-compose logs nginx
```

