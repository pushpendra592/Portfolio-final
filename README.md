# Portfolio Final - Jenkins CI/CD Docker Deployment

This project is a static portfolio website that is containerized using Docker and deployed on an AWS EC2 instance through a Jenkins CI/CD pipeline.

The pipeline automates the complete deployment process, starting from pulling the source code from GitHub, building a Docker image, validating the container health, pushing the image to Docker Hub, and finally deploying the updated container on an AWS EC2 instance.

---

## Live Deployment

The project is deployed on AWS EC2 and can be accessed using the link below:

[http://44.223.5.145/](http://44.223.5.145/)

> Note: This link uses the public IPv4 address of the EC2 instance. If the EC2 instance is stopped and started again, the public IP may change unless an Elastic IP is assigned.

---

## Project Structure

```text
portfolio-final/
│
├── Dockerfile
├── Jenkinsfile
├── index.html
├── profile.jpeg
├── resume.pdf
└── README.md
```

---

## Technologies Used

* HTML
* Docker
* Nginx Alpine
* Jenkins
* GitHub
* Docker Hub
* AWS EC2

---

## Docker Image

The Docker image for this project is hosted on Docker Hub.

```text
regentshark7/portfolio-final:latest
```

## Dockerfile

The project uses an Nginx Alpine base image to serve the static website files.

```dockerfile
FROM nginx:alpine

COPY . /usr/share/nginx/html/

EXPOSE 80
```

### Explanation

* `nginx:alpine` is used as a lightweight web server image.
* All project files are copied into the Nginx HTML directory.
* Port `80` is exposed to serve the website.

---

## Jenkins CI/CD Pipeline

The CI/CD pipeline is defined in the `Jenkinsfile`.

The pipeline performs the following tasks:

1. Pulls the latest source code from GitHub.
2. Builds a Docker image from the project files.
3. Runs a temporary test container.
4. Performs a health check using `curl`.
5. Pushes the Docker image to Docker Hub.
6. Connects to the AWS EC2 instance using SSH.
7. Pulls the latest Docker image on EC2.
8. Stops and removes the old running container.
9. Runs the new container on port `80`.
10. Verifies the deployed application using the EC2 public IP.

---

## Pipeline Stages

```text
Checkout Source Code
Build Docker Image
Run Test Container
Health Check
Stop Test Container
Push Docker Image to Docker Hub
Deploy to AWS EC2
Verify EC2 Deployment
```

---

## Running the Project Locally

To run the project locally using Docker, follow these steps.

### 1. Build the Docker image

```bash
docker build -t portfolio-final .
```

### 2. Run the Docker container

```bash
docker run -d -p 8081:80 --name portfolio-container portfolio-final
```

### 3. Open the website

Open the following URL in your browser:

```text
http://localhost:8081
```

### 4. Stop and remove the container

```bash
docker rm -f portfolio-container
```

---

## Manual Deployment on EC2

The application can also be manually deployed on the EC2 instance using Docker.

### 1. Pull the Docker image

```bash
docker pull regentshark7/portfolio-final:latest
```

### 2. Run the container

```bash
docker run -d -p 80:80 --name portfolio-container regentshark7/portfolio-final:latest
```

### 3. Open the deployed application

```text
http://44.223.5.145/
```

---

## Jenkins Credentials Required

The Jenkins pipeline requires two credentials.

### Docker Hub Credentials

```text
ID: dockerhub-credentials
Type: Username with password
Username: Docker Hub username
Password: Docker Hub access token
```

### EC2 SSH Key

```text
ID: ec2-ssh-key
Type: SSH Username with private key
Username: ubuntu
Private Key: EC2 .pem key content
```

The `.pem` file should not be uploaded to GitHub.

---

## Security Note

The EC2 private key file should never be committed to GitHub.

Add the following lines to `.gitignore`:

```gitignore
*.pem
*.ppk
```

---

## AWS EC2 Security Group Rules

The EC2 instance should allow the following inbound rules:

| Type       | Port | Source    |
| ---------- | ---: | --------- |
| SSH        |   22 | My IP     |
| HTTP       |   80 | 0.0.0.0/0 |
| Custom TCP | 8080 | 0.0.0.0/0 |

Port `80` is required for public access to the deployed website.

Port `22` is required for Jenkins to connect to the EC2 instance using SSH.

---

## Final Deployment URL

```text
http://44.223.5.145/
```

---

## Project Summary

This project demonstrates an automated DevOps workflow using Jenkins, Docker, Docker Hub, and AWS EC2. The Jenkins pipeline builds and validates the application as a Docker container, pushes the image to Docker Hub, and deploys it to an EC2 instance where it is served through Nginx on port `80`.

```
```
