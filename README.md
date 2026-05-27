# Jenkins Docker Kubernetes CI/CD Project

## Tools Used
- Jenkins
- Docker
- Kubernetes
- Flask
- AWS EC2

## CI/CD Flow
GitHub → Jenkins → Docker → Kubernetes

## Run Application Locally
```bash
pip install -r requirements.txt
python src/app.py
```

## Build Docker Image
```bash
docker build -t flask-devops-app .
```

## Run Docker Container
```bash
docker run -p 5000:5000 flask-devops-app
```

## Kubernetes Deployment
```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

## Verify Deployment
```bash
kubectl get pods
kubectl get services
minikube service flask-service --url
```

---

## EC2 Setup Commands

### Update System
```bash
sudo apt update
```

### Install Docker
```bash
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

### Install Java
```bash
sudo apt install openjdk-17-jdk -y
```

### Install Jenkins
```bash
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

### Allow Jenkins to Use Docker
```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

### Install kubectl
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

### Install Minikube
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube start --driver=docker
```
