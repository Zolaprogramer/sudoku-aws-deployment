# AWS Cloud Sudoku Project

This repository contains the Sudoku web application developed and deployed as an internship task for JSGuru. The project is designed with a decoupled architecture, leveraging a static frontend and a serverless backend infrastructure.

## Architecture Overview

The application architecture is optimized for performance, scalability, and security:
- **Frontend / Web Server:** Hosted on an AWS EC2 t2.micro instance running Ubuntu Server. Nginx is configured as the primary web server to serve static assets and act as a secure infrastructure layer.
- **Backend Architecture:** Serverless infrastructure utilizing AWS Lambda and API Gateway hidden behind a CloudFront distribution available at https://sudoku.lazarm.com.
- **API Routing:** Implemented via an Nginx reverse proxy block (location /puzzle) to safely forward client requests to the backend API without exposing direct endpoints.

## Infrastructure Setup

### 1. Security Groups (AWS EC2)
The instance is protected by an AWS Security Group with the following minimal rules:
- Port 22 (SSH): Restricted access for system administration.
- Port 80 (HTTP): Open to public web traffic.

### 2. Nginx Reverse Proxy Configuration
The server redirects API queries from the static layer to the remote AWS Lambda backend using the following reverse proxy directive inside /etc/nginx/sites-available/default:

```nginx
location /puzzle {
        proxy_pass https://sudoku.lazarm.com;
        proxy_set_header Host sudoku.lazarm.com;
        proxy_ssl_server_name on;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```

## Authors
- Lazar Malbašić - Student of Programming and Software Engineering
