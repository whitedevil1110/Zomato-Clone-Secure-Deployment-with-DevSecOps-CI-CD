# 🍽️ **Zomato Clone: Secure Deployment with DevSecOps CI/CD**  

![Zomato Clone](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*X_hm5iF0NRjbOZHB6RQIFA.jpeg)  

🚀 **A full-stack Zomato Clone with DevSecOps integration for a secure and scalable deployment.**  

This **Zomato Clone: Secure Deployment with DevSecOps CI/CD** repository is a **full-stack food delivery application** inspired by Zomato. It includes a **React.js frontend** and integrates DevSecOps practices to ensure a secure and automated deployment pipeline.  

## 🔹 **Key Features:**  

✅ **User-friendly UI** – Interactive menu, checkout, and order tracking.  
✅ **Secure DevOps Pipeline** – CI/CD automation with security checks.  
✅ **Optimized Deployment** – Uses containerization & cloud hosting.  s
✅ **React.js-Based Frontend** – Built with Create React App.  
✅ **Production-Ready** – Includes build optimizations & best practices.  

> This repo is a great **learning project** for those interested in **DevSecOps, CI/CD, and secure web app deployment**. 🚀

## 📸 **Project Screenshots**  

| Home Page | Menu View | Checkout Page |  
|-----------|----------|---------------|  
| ![](https://miro.medium.com/v2/resize:fit:750/format:webp/1*xVxk3tSbk9yA6hel60t13g.png) | ![](https://miro.medium.com/v2/resize:fit:750/format:webp/1*KOwp6K2sOcSmDyk9Axnvhw.png) | ![](https://miro.medium.com/v2/resize:fit:750/format:webp/1*t1x_F_qwHI6anvRHS59OxA.png) |  

---

## 🛠️ **Getting Started**  

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

### 🔧 **Prerequisites**  

Ensure you have **Node.js** and **npm** installed.  

```sh
node -v   # Check Node.js version
npm -v    # Check npm version
```

### 📦 **Installation**  

Clone the repository and install dependencies:  

```sh
git clone https://github.com/whitedevil1110/Zomato-Clone-Secure-Deployment-with-DevSecOps-CI-CD.git
cd zomato-clone  
npm install  
```

## Steps:-
 - Step 1 — Launch an Ubuntu(22.04) T2 Large Instance

 - Step 2 — Install Jenkins, Docker and Trivy. Create a Sonarqube Container using Docker.

 - Step 3 — Install Plugins like JDK, Sonarqube Scanner, Nodejs, and OWASP Dependency Check.

 - Step 4 — Create a Pipeline Project in Jenkins using a Declarative Pipeline

 - Step 5 — Install OWASP Dependency Check Plugins

 - Step 6 — Docker Image Build and Push

 - Step 7 — Deploy the image using Docker

 - Step 8 — Terminate the AWS EC2 Instances.

Now, let’s get started and dig deeper into each of these steps:-

### Step 1:Launch an Ubuntu(22.04) T2 Large Instance
 - Launch an AWS T2 Large Instance. Use the image as Ubuntu. You can create a new key pair or use an existing one. Enable HTTP and HTTPS settings in the Security Group and open all ports (not best case to open all ports but just for learning purposes it’s okay).

https://miro.medium.com/v2/resize:fit:736/1*7B50QPeIbJ-EbQh7E-wOdQ.png

### Step 2 — Install Jenkins, Docker and Trivy
## 2A — To Install Jenkins
 - Connect to your console, and enter these commands to Install Jenkins
```bash
vim jenkins.sh
```
 - Copy

``` bash
sudo apt update -y
#sudo apt upgrade -y
wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | tee /etc/apt/keyrings/adoptium.asc
echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list
sudo apt update -y
sudo apt install temurin-17-jdk -y
/usr/bin/java --version
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
                  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
                  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
                              /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update -y
sudo apt-get install jenkins -y
sudo systemctl start jenkins
sudo systemctl status jenkins
```
```bash
sudo chmod 777 jenkins.sh
./jenkins.sh    # this will installl jenkins
```
 - Once Jenkins is installed, you will need to go to your AWS EC2 Security Group and open Inbound Port 8080, since Jenkins works on Port 8080.

 - Now, grab your Public IP Address

```bash
<EC2 Public IP Address:8080>
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
[](https://miro.medium.com/v2/resize:fit:736/1*EpiSV85C0SEvChz5zm4rIw.png)

 - Unlock Jenkins using an administrative password and install the suggested plugins.

[](https://miro.medium.com/v2/resize:fit:736/1*oFUO_Cga52FT8bscLry3_Q.png)

 - Jenkins will now get installed and install all the libraries.

[](https://miro.medium.com/v2/resize:fit:736/1*L0XlriJskwbydfaJHRcbCw.png)

 - Create a user click on save and continue.

 - Jenkins Getting Started Screen.

[](https://miro.medium.com/v2/resize:fit:736/1*OpoGxd6FwcZeZWZKBHdlfQ.png)

### 2B — Install Docker

```bash
sudo apt-get update
sudo apt-get install docker.io -y
sudo usermod -aG docker $USER   #my case is ubuntu
newgrp docker
sudo chmod 777 /var/run/docker.sock
```

 - After the docker installation, we create a sonarqube container (Remember to add 9000 ports in the security group).

```bash
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
```

[](https://miro.medium.com/v2/resize:fit:736/1*gHXat-rdGvFXGYASgPQblw.png)

 - Now our sonarqube is up and running

[](https://miro.medium.com/v2/resize:fit:736/1*1nZijc9tTcLWN9-SieDGiw.png)

 - Enter username and password, click on login and change password

```bash
username admin
password admin
```

[](https://miro.medium.com/v2/resize:fit:736/1*fzoWe4G78pLSt3I-tiFJXA.png)

 - Update New password, This is Sonar Dashboard.

[](https://miro.medium.com/v2/resize:fit:736/1*6b0ljtihR81StlDV3a3d_Q.png)

### 2C — Install Trivy

```bash
vim trivy.sh
```

```bash
sudo apt-get install wget apt-transport-https gnupg lsb-release -y
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy -y
```

 - Next, we will log in to Jenkins and start to configure our Pipeline in Jenkins

### Step 3 — Install Plugins like JDK, Sonarqube Scanner, NodeJs, OWASP Dependency Check
## 3A — Install Plugin
 - Goto Manage Jenkins →Plugins → Available Plugins →

 - Install below plugins

 - 1 → Eclipse Temurin Installer (Install without restart)

 - 2 → SonarQube Scanner (Install without restart)

 - 3 → NodeJs Plugin (Install Without restart)

[](https://miro.medium.com/v2/resize:fit:736/1*OQlTC716djHfaS3AwDOfdA.png)

### 3B — Configure Java and Nodejs in Global Tool Configuration
 - Goto Manage Jenkins → Tools → Install JDK(17) and NodeJs(16)→ Click on Apply and Save

[](image-1.png)

[](image-2.png)

### 3C — Create a Job
 - Create a job as Zomato Name, select pipeline and click on ok.

[](image-3.png)

### Step 4 — Configure Sonar Server in Manage Jenkins
 - Grab the Public IP Address of your EC2 Instance, Sonarqube works on Port 9000, so <Public IP>:9000. Goto your Sonarqube Server. Click on Administration → Security → Users → Click on Tokens and Update Token → Give it a name → and click on Generate Token

[](https://miro.medium.com/v2/resize:fit:736/1*Ziil8QHxn94AdCIsauURrg.png)

 - click on update Token

[](https://miro.medium.com/v2/resize:fit:736/1*IgOyVcZ9nPjXNNGOpneISw.png)

 - Create a token with a name and generate

[](https://miro.medium.com/v2/resize:fit:736/1*uAkoy85MXN3dTWbn5Gkjuw.png)

 - copy Token

 - Goto Jenkins Dashboard → Manage Jenkins → Credentials → Add Secret Text. It should look like this.

[](https://miro.medium.com/v2/resize:fit:736/1*E-ccRm-XBqKSTUZm225hoA.png)

 - You will this page once you click on create

[](https://miro.medium.com/v2/resize:fit:736/1*4i_R7hLnBay1HgrMEkkHMA.png)

 - Now, go to Dashboard → Manage Jenkins → System and Add like the below image.

[](https://miro.medium.com/v2/resize:fit:736/1*L-jn869O3XQ3UQcA9dKcSQ.png)

 - Click on Apply and Save

 - The Configure System option is used in Jenkins to configure different server

 - Global Tool Configuration is used to configure different tools that we install using Plugins

 - We will install a sonar scanner in the tools.

[](https://miro.medium.com/v2/resize:fit:736/1*HE_NnYldri714mObLqKxcQ.png)

 - In the Sonarqube Dashboard add a quality gate also

 - Administration → Configuration → Webhooks

[](https://miro.medium.com/v2/resize:fit:736/1*lYImJVM_K4c3xBTrT9YzHA.png)

 - Click on Create

[](https://miro.medium.com/v2/resize:fit:736/1*Xq0O7dtRS6moajyVdZ2OJA.png)

 - Add details

```bash
#in url section of quality gate
 <http://jenkins-public-ip:8080>/sonarqube-webhook/
```

[](https://miro.medium.com/v2/resize:fit:736/1*NWbqEH2whidmGvvIrEuTyQ.png)

 - Let’s go to our Pipeline and add the script in our Pipeline Script.

```bash
pipeline{
    agent any
    tools{
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                git branch: 'main', url: 'https://github.com/mudit097/Zomato-Clone.git'
            }
        }
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=zomato \
                    -Dsonar.projectKey=zomato '''
                }
            }
        }
        stage("quality gate"){
           steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token' 
                }
            } 
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
    }
}
```
 - Click on Build now, you will see the stage view like this.

[](https://miro.medium.com/v2/resize:fit:736/1*zPT6BjUUCT_xG3APTmsSLw.png)

 - To see the report, you can go to Sonarqube Server and go to Projects.

[](https://miro.medium.com/v2/resize:fit:736/1*zPT6BjUUCT_xG3APTmsSLw.png)

 - You can see the report has been generated and the status shows as passed. You can see that there are 1.3k lines. To see a detailed report, you can go to issues.

### Step 5 — Install OWASP Dependency Check Plugins
 - GotoDashboard → Manage Jenkins → Plugins → OWASP Dependency-Check. Click on it and install it without restart.

[](image-4.png)

 - First, we configured the Plugin and next, we had to configure the Tool

 - Goto Dashboard → Manage Jenkins → Tools →

[](image-5.png)

 - Click on Apply and Save here.

 - Now go configure → Pipeline and add this stage to your pipeline and build.

```bash

        stage('OWASP FS SCAN') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs . > trivyfs.txt"
            }
        }
```
 - The stage view would look like this,

[](https://miro.medium.com/v2/resize:fit:736/1*ASvOScJgoezWNQBDiulONQ.png)

 - You will see that in status, a graph will also be generated and Vulnerabilities.

[](https://miro.medium.com/v2/resize:fit:736/1*HKjI3tzLa_D4NLob4nr0mw.png)

---


### 🚀 **Running the App**  

Start the development server:  

```sh
npm start  
```

🔗 Open [http://localhost:3000](http://localhost:3000) in your browser.  

---

## 📜 **Available Scripts**  

| Command | Description |  
|---------|------------|  
| `npm start` | Runs the app in development mode |  
| `npm test` | Launches the test runner |  
| `npm run build` | Builds the app for production |  
| `npm run eject` | Ejects the default configuration (irreversible) |  

For more details, check the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).  

---

## 🚢 **Deployment**  

### 🔥 **Build for Production**  

```sh
npm run build  
```

📂 The app is built in the `build/` folder, ready for deployment.  

### 🌍 **Hosting**  

You can deploy this project on:  

- **Vercel** ([Docs](https://vercel.com/docs))  
- **Netlify** ([Docs](https://docs.netlify.com/))  
- **AWS Amplify** ([Docs](https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html))  

---

## 📚 Learn More  

- 🏗️ [React Documentation](https://reactjs.org/)  
- ⚡ [Code Splitting](https://facebook.github.io/create-react-app/docs/code-splitting)  
- 🔍 [Analyzing Bundle Size](https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size)  
- 📲 [Making a Progressive Web App](https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app)  

---

## 🤝 **Contributing**  

Contributions are welcome! If you'd like to improve this project, feel free to submit a pull request.  

---

## **Hit the Star!** ⭐

**If you find this repository helpful and plan to use it for learning, please give it a star. Your support is appreciated!**

---

## 🛠️ **Author & Community**  

This project is crafted by **[Whitedevil](https://github.com/whitedevil1110)** 💡.  
I’d love to hear your feedback! Feel free to share your thoughts.  

---

### 📧 **Connect with me:**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?style=for-the-badge&logo=linkedin&logoColor=white)](www.linkedin.com/in/shlok-khopkar-172694215) [![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/whitedevil1110)  [![Telegram](https://img.shields.io/badge/Telegram-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)]() [![Dev.to](https://img.shields.io/badge/Dev.to-0A0A0A?style=for-the-badge&logo=dev.to&logoColor=white)]() [![Hashnode](https://img.shields.io/badge/Hashnode-2962FF?style=for-the-badge&logo=hashnode&logoColor=white)]()  

---

### 📢 **Stay Connected**  
---

### 🎉 **Happy Coding!** 🚀  
