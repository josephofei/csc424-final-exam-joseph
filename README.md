# CSC 424 Final Exam

## DevOps Setup

To run the full application stack locally, use:

```bash
docker compose up --build -d
```

The application runs on port 80 through the Nginx reverse proxy.

Frontend URL:
```text
http://localhost
```

Backend API test URL:
```text
http://localhost/api/ping
```

## Services

### Frontend
The frontend service is a React + Vite application. It is containerized using a multi-stage Docker build where the application is built in a Node.js environment and served using Nginx.

### Backend
The backend service is a .NET API application. It uses a multi-stage Docker build with the .NET SDK image for building and the .NET runtime image for production deployment.

### Nginx
The Nginx service acts as a reverse proxy. It routes:
- `/` requests to the frontend container
- `/api/` requests to the backend container

Only the Nginx container exposes port 80 to the host machine.

## Docker Compose

The Docker Compose configuration defines:
- frontend service
- backend service
- nginx service

All services communicate using a shared custom Docker network.

## CI/CD Pipeline

The CI/CD pipeline is implemented using GitHub Actions.

On every push to the `main` branch, the workflow:
1. Builds the frontend and backend Docker images
2. Pushes the images to GitHub Container Registry (GHCR)
3. SSHs into the QA virtual machine
4. Pulls the latest images
5. Deploys the updated containers using Docker Compose

Sensitive credentials such as SSH keys and registry authentication are stored securely using GitHub Secrets.