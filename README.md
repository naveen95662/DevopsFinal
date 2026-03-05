AWS DevOps Capstone: Automated CI/CD & Monitoring Pipeline

Project Description
**This project demonstrates a production-ready DevOps lifecycle for a Node.js application. It automates the process of taking code from a repository, containerizing it with Docker, and deploying it to an AWS EC2 instance using Jenkins. To ensure "Day 2" operational stability, the project includes a full monitoring stack (Prometheus & Grafana) and automated system maintenance scripts.**
-----------------------------------------------------------------------
Tech Stack:

Cloud: AWS (EC2)
CI/CD: Jenkins
Containerization: Docker & Docker Hub
Monitoring: Prometheus & Node Exporter
Visualization: Grafana
Scripting: Bash (for automated maintenance)
Language: Node.js

-----------------------------------------------------------------------
Setup Instructions

To run this project locally or on a new Ubuntu server, follow these commands:

1. Prerequisites
Ensure Docker and Node.js are installed:
Command to install : sudo apt update && sudo apt install docker.io nodejs npm -y

2. Build and Run Locally
# Clone the repository
git clone <your-repo-url>
cd <project-folder>

# Build the Docker image
docker build -t node-capstone-app .

# Run the container
docker run -d -p 3000:3000 --name my-app node-capstone-app

Access the app at http://localhost:3000

3. Monitoring Setup
Start the monitoring agents:

# Start Node Exporter
./node_exporter &

# Start Prometheus
./prometheus --config.file=prometheus.yml &

--------------------------------------------------------------------

CI/CD Flow Explained
The pipeline is defined in a Jenkinsfile and follows these automated stages:

Cleanup Workspace: Deletes old build artifacts to ensure a clean environment.

Checkout Code: Pulls the latest version of the code from GitHub.

Build Docker Image: Creates a new container image tagged with the specific Jenkins Build ID.

Login & Push: Authenticates with Docker Hub and stores the image in the registry.

Deploy to EC2: Automatically stops the old container and launches the new version on Port 3000.
