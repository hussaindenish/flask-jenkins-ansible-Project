## Project: CI/CD Pipeline with Jenkins and Ansible on One EC2 Server/Local_Server

This project demonstrates how to set up a complete CI/CD pipeline on a **single EC2 server** using:

* ✅ Jenkins (automation server)
* ✅ Ansible (configuration management)
* ✅ Flask (Python web app)
* ✅ GitHub (source code)

> We will **automate the build, configuration, and deployment** of a Flask app using Jenkins + Ansible. Everything runs on the same EC2 instance.

---

## 🧠 What Is CI/CD?

### CI/CD stands for:

* **CI = Continuous Integration**: Automatically test and build your app when someone pushes code to GitHub.
* **CD = Continuous Deployment**: Automatically deploy the updated app when the build passes.

---

## 📦 Project Structure

```
flask-jenkins-ansible-Project/
├── app.py               # Simple Python Flask web app
├── requirements.txt     # Python dependencies
├── Jenkinsfile          # Jenkins pipeline instructions
├── inventory.ini        # Ansible inventory file (localhost)
└── deploy.yml           # Ansible playbook to deploy the app
```

---

## 🌐 How It Works (Step-by-Step)

### 🧱 1. Setup One EC2 Instance

* Launch a single EC2 instance (Ubuntu 22.04).
* Install required tools: Python3, pip, Git, Jenkins, Ansible.
* Install Java 17+ for Jenkins.

### 🔧 2. Install and Configure Jenkins

* Install Jenkins and start the service.
* Open Jenkins at `http://<your-ec2-ip>:8080`.
* Unlock Jenkins using the initial admin password (`/var/lib/jenkins/secrets/initialAdminPassword`).
* Install recommended plugins.
* Create an admin user.

### 🐍 3. Create a Simple Flask App

**app.py**

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Jenkins + Ansible + Flask CI/CD!"

if __name__ == '__main__':
    app.run(host='0.0.0.0')
```

**requirements.txt**

```
flask
```

### ⚙️ 4. Write Jenkinsfile (CI/CD Pipeline Logic)

**Jenkinsfile**

```groovy
pipeline {
    agent any
    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/YOUR_USERNAME/flask-jenkins-ansible-Project.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip3 install -r requirements.txt'
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                sh 'ansible-playbook -i inventory.ini deploy.yml'
            }
        }
    }
}
```

This file tells Jenkins:

1. Clone the project repo.
2. Install dependencies.
3. Run Ansible to deploy the app.

### 📜 5. Ansible Inventory File

**inventory.ini**

```
localhost ansible_connection=local
```

Tells Ansible: we are running everything on the same EC2 (localhost).

### 🔧 6. Ansible Playbook

**deploy.yml**

```yaml
- name: Deploy Flask App
  hosts: localhost
  tasks:
    - name: Stop existing app (if running)
      shell: pkill -f app.py || true

    - name: Run Flask app in background
      shell: nohup python3 app.py &
```

This playbook:

* Stops any running app.
* Restarts the app in the background.

### 🧪 7. Push Project to GitHub

```bash
git init
git add .
git commit -m "Initial CI/CD project"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/flask-jenkins-ansible-Project.git
git push -u origin main
```

🔐 Make sure you push using a GitHub personal access token (PAT), not a password.

---

## 🧪 8. Create Jenkins Pipeline Job

* Go to Jenkins dashboard ➡️ New Item ➡️ "Pipeline".
* Name: `Flask-CI-CD`
* Choose: **Pipeline**
* In pipeline config:

  * GitHub repo: `https://github.com/YOUR_USERNAME/flask-jenkins-ansible-Project.git`
  * Script path: `Jenkinsfile`

---

## 🚀 9. Run Jenkins Job

* Click "Build Now".
* Jenkins:

  * Clones the code
  * Installs Flask
  * Runs Ansible
  * Launches Flask app

Visit: `http://<your-ec2-ip>:5000`
🎉 You should see: `Hello from Jenkins + Ansible + Flask CI/CD!`

---

## 🧹 Optional Clean-Up

If needed, stop the Flask app:

```bash
pkill -f app.py
```

---

## 📚 Bonus Tips

* Want to see logs? Use `journalctl -u jenkins` or `/var/log/jenkins/`.
* App not running? Check port `5000` is open in EC2 Security Group.
* Don’t forget to allow Jenkins (port 8080) and Flask (port 5000) in your firewall rules.

---

## 🧠 Why This Project Is Useful

* Learn **CI/CD basics** using real tools.
* Understand how Jenkins, Ansible, and GitHub work together.
* Deploy an app without manually touching servers.

---
