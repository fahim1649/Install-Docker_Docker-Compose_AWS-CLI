# Install-Docker_Docker-Compose_AWS-CLI
```javascript
#!/bin/bash

set -e  # exit immediately if a command exits with non-zero

# ================================
# EDIT THESE VALUES BEFORE RUNNING
# ================================
AWS_ACCESS_KEY_ID="YOUR_ACCESS_KEY"
AWS_SECRET_ACCESS_KEY="YOUR_SECRET_KEY"
AWS_DEFAULT_REGION="ap-south-1"
AWS_OUTPUT_FORMAT="json"
# ================================

# ================================
# Step 1: Install Docker
# ================================
echo "Installing Docker..."
sudo apt-get update -y
sudo apt-get install -y docker.io

echo "Starting Docker service..."
sudo systemctl start docker
sudo systemctl enable docker

echo "Docker Version:"
docker --version

# ================================
# Configure Docker permissions
# ================================
echo "Configuring Docker permissions..."

# Fix ownership of the docker.sock file
sudo chown $USER:$(id -gn $USER) -R /var/run/docker.sock

# Make socket accessible (⚠️ wide open, be careful in production)
sudo chmod 777 /var/run/docker.sock

# Add user to docker group
sudo usermod -aG docker $USER

# Apply new group immediately
newgrp docker <<EONG
echo "Docker group applied for user: $USER"
docker run hello-world || true
EONG

# ================================
# Step 2: Install Docker Compose
# ================================
echo "Installing Docker Compose..."
DOCKER_COMPOSE_VERSION="1.29.2"
wget https://github.com/docker/compose/releases/download/$DOCKER_COMPOSE_VERSION/docker-compose-Linux-x86_64 -O docker-compose
sudo mv docker-compose /usr/bin/
sudo chmod +x /usr/bin/docker-compose

echo "Docker Compose Version:"
docker-compose --version

echo "✅ Docker and Docker Compose installed successfully!"


# ================================
# Step 3: Install AWS CLI v2
# ================================
echo "Downloading AWS CLI v2..."
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

echo "Installing unzip..."
sudo apt-get update -y
sudo apt-get install -y unzip

echo "Unzipping AWS CLI package..."
unzip -o awscliv2.zip

echo "Installing AWS CLI..."
sudo ./aws/install

echo "AWS CLI Version:"
aws --version

echo "Configuring AWS credentials..."
aws configure set aws_access_key_id "$AWS_ACCESS_KEY_ID"
aws configure set aws_secret_access_key "$AWS_SECRET_ACCESS_KEY"
aws configure set default.region "$AWS_DEFAULT_REGION"
aws configure set default.output "$AWS_OUTPUT_FORMAT"

echo "✅ AWS CLI installed and configured successfully!"


✅ docker --version 

✅ docker-compose --version

echo "✅ Current AWS Configuration:"
aws configure list

```
