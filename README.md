In this DevOps task, you need to build and deploy a full-stack CRUD application using the MEAN stack (MongoDB, Express, Angular 15, and Node.js). The backend will be developed with Node.js and Express to provide REST APIs, connecting to a MongoDB database. The frontend will be an Angular application utilizing HTTPClient for communication.  

The application will manage a collection of tutorials, where each tutorial includes an ID, title, description, and published status. Users will be able to create, retrieve, update, and delete tutorials. Additionally, a search box will allow users to find tutorials by title.

## Project setup

### Node.js Server

cd backend

npm install

You can update the MongoDB credentials by modifying the `db.config.js` file located in `app/config/`.

Run `node server.js`

### Workflow file: `.github/workflows/ci-cd-pipeline.yml`

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      
      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_HUB_USERNAME }}
          password: ${{ secrets.DOCKER_HUB_ACCESS_TOKEN }}
      
      - name: Build & Push Backend
        run: |
          docker build -t <dockerhub-username>/backend-crud-app:latest ./backend
          docker push <dockerhub-username>/backend-crud-app:latest
      
      - name: Build & Push Frontend
        run: |
          docker build -t <dockerhub-username>/frontend-crud-app:latest ./frontend
          docker push <dockerhub-username>/frontend-crud-app:latest
      
      - name: SSH to Server & Deploy
        uses: appleboy/ssh-action@v0.1.7
        with:
          host: ${{ secrets.VM_IP }}
          username: ubuntu
          key: ${{ secrets.VM_SSH_KEY }}
          script: |
            cd /home/ubuntu/crud-dd-task-mean-app
            docker-compose pull
            docker-compose up -d
```

### Application Screenshots

## CI/CD Execution on GitHub Actions
![GitHub Actions Workflow](screenshots/CI-CD.png)

## Docker Image Build & Push Logs
![Docker Build Logs](screenshots/docker-logs.png)

## Frontend UI Running on VM
![Frontend Application](screenshots/GUI.png)

## Backend API Response in Browser/Postman
![API Response](screenshots/api-response.png)

## Nginx Setup & Infrastructure

- Frontend served via Nginx inside the container
- Port 80 mapped to host for public access
- Backend container communicates with MongoDB container internally
- Docker Compose ensures **multi-container orchestration**, **automatic network setup**, and volume persistence for MongoDB

## Repository Link

[GitHub Repository](https://github.com/Bharath18sv/crud-dd-task-mean-app)