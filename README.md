# Project-SuperMario-CICD

---

### Script file to install Jenkins & Terraform
````
#!/bin/bash

set -e  # exit on error

echo "🚀 Updating system..."
sudo apt update -y

echo "☕ Installing Java (required for Jenkins)..."
sudo apt install -y fontconfig openjdk-21-jre
java -version

echo "🔐 Adding Jenkins repository key..."
sudo mkdir -p /etc/apt/keyrings
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key

echo "📦 Adding Jenkins repository..."
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | \
  sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

echo "🔄 Updating package list..."
sudo apt update -y

echo "⚙️ Installing Jenkins..."
sudo apt install -y jenkins

echo "▶️ Starting and enabling Jenkins..."
sudo systemctl enable jenkins
sudo systemctl start jenkins

echo "📊 Jenkins status:"
sudo systemctl status jenkins --no-pager

# -------------------------------
# Terraform Installation
# -------------------------------

echo "🔐 Adding HashiCorp GPG key..."
wget -O - https://apt.releases.hashicorp.com/gpg | \
sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

echo "📦 Adding Terraform repository..."
echo "deb [arch=$(dpkg --print-architecture) \
signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com \
$(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list

echo "🔄 Updating package list..."
sudo apt update -y

echo "🌍 Installing Terraform..."
sudo apt install -y terraform

echo "📦 Terraform version:"
terraform -version

# -------------------------------
# Final Info
# -------------------------------

echo "✅ Setup Complete!"

echo "👉 Access Jenkins at:"
echo "http://<your-server-ip>:8080"

echo "🔑 Get initial admin password:"
echo "sudo cat /var/lib/jenkins/secrets/initialAdminPassword"
````

---

