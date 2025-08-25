# Development Phase

The **development phase** in DevSecOps ensures that code is both **secure** and meets application requirements before deployment.  
DevSecOps emphasizes integrating **security practices and tools throughout development**, instead of treating security as an afterthought.  

---

## Connecting SonarLint Plugin to Visual Studio Code

SonarLint helps analyze code inside **VS Code**. You can connect it either to a **SonarQube server** (self-hosted) or **SonarCloud** (cloud-based).

---

### 🔹 Connect SonarLint to SonarQube Server

1. Install **Visual Studio Code**.  
2. Open VS Code → go to **Extensions** (`Ctrl+Shift+X` on Windows/Linux or `Cmd+Shift+X` on macOS).  
3. Search for **"SonarLint"** and install the extension by **SonarSource**.  
4. Open the project you want to analyze.  
5. Go to: **Analyze → Manage SonarQube Connections**.  
6. Click **Connect** and enter your **SonarQube server URL, username, and password**.  
7. Once connected, SonarLint will link to the SonarQube server.  
8. Click **Analyze** to start scanning.  
9. Issues will appear in the **SonarLint panel**.  

---

### 🔹 Connect SonarLint to SonarCloud

1. Go to [SonarCloud.io](https://sonarcloud.io).  
2. Navigate to **My Account → Security → Generate Tokens**.  
3. Enter a token name → click **Generate**.  
4. In VS Code, follow steps **1–5** from the SonarQube section.  
5. Instead of username/password, enter the **token** generated in step 3.  
6. SonarLint will connect to SonarCloud.  
7. Click **Analyze** to start scanning.  
8. Issues will appear in the **SonarLint panel**.  

---

# Version Control Phase

The **version control phase** is an integral part of the DevSecOps workflow, enabling teams to track and manage changes to the codebase and easily revert to previous versions if necessary.
It allows for the integration of **security testing and automated checks** within the version control system to ensure that vulnerabilities are identified and addressed as soon as they are introduced.

---

## Git Secret

Git-secret is a command-line tool that allows you to store and manage secrets, such as passwords and private keys, in a Git repository.

In GitLab, Git-secret can be integrated into a repository by adding a GitLab CI/CD pipeline that runs the git-secret command on each commit or merge request.
If sensitive data is detected, the pipeline can be configured to fail the build or merge request, preventing the sensitive data from being committed.

---

### Integrating Git Secret with GitLab  

**1. Install Git Secrets** 
- Make sure **Git** is installed on your system.  
- Download the latest version of Git Secrets from [GitHub](https://github.com/awslabs/git-secrets).  
- Extract the downloaded archive and navigate to the extracted folder in the command line.  

Run the following command to install Git Secrets:  
```bash
sudo make install   # For Linux and macOS
```
For Windows:

Run the provided `install.ps1` PowerShell script.
This will copy the files to `%USERPROFILE%/.git-secrets` by default and add the directory to the PATH.

**2. Initialize Git Secrets**

Navigate to your local repository and run:
```bash
git secrets --install
```

This will add a .gitsecrets file to your repository.

**3. Add Secrets**
```bash
git secrets --add <secret>
```

Run the above command for each secret you want to protect.

**4. Scan Repository**
```bash
git secrets --scan
```

This will check your repository for any accidentally committed secrets.

**5. Integrate with GitLab**

- Go to your GitLab repository → Settings → Repository.
- Scroll to Protected branches.
- Select the branch you want to protect → click Protect.
- Under Merge request approvals, check Prevent merges and set at least 1 approval.
- Under Protect this branch, check Require pipelines to succeed before merging.
- Now, GitLab will run the pipeline before merging, and Git Secrets will scan for sensitive data.

---

# Pre-Build Phase   

The **pre-build phase** in DevSecOps refers to the activities that take place before the code is built or compiled.  

During this phase, **SAST (Static Application Security Testing) tools** such as **SonarQube**, **RIPS**, etc., are used to analyze source code for vulnerabilities early in the development lifecycle.  

---

## How to Create a Virtual Machine (Server) on GCP  

1. In the **Google Cloud Console**, on the project selector page, select or create a project.  
2. Enable the **Compute Engine API**.  
3. In the console, click **Create an instance**.  
4. Give your server a **name**.  
5. Under machine configuration, choose:  
   - **E2 Series**  
   - **e2-medium** machine type  
6. In the **Boot disk** section, click **Change**:  
   - Disk size: **10GB**  
   - OS: **Ubuntu** → **20.04 LTS**  
7. Click **Select**.  
8. In the **Firewall** section, select:  
   - **Allow HTTP traffic**  
   - **Allow HTTPS traffic**  
9. Click **Create**.  
10. Create firewall rules:  
    - Navigate to **VPC Network → Firewall**.  
11. Make ephemeral IP addresses static:  
    - Navigate to **VPC Network → IP addresses**.  
12. To connect to the VM:  
    - Go to **VM Instances** page.  
    - Click **SSH** in the row of the instance you want to connect to.  

---

## Jenkins  

### How to Install Jenkins on Ubuntu Server  

**Prerequisites:**  
- A fresh Ubuntu installation  
- A user with administrative privileges  

**1. Update your system:**  
```bash
sudo apt-get update
```
**2. Add Jenkins repository:**
```bash
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
```

**3. Install Jenkins:**
```bash
sudo apt-get update
sudo apt-get install jenkins
```

**4. Start Jenkins service:**
```bash
sudo systemctl start jenkins
```

**5. Enable Jenkins at startup:**
```bash
sudo systemctl enable jenkins
```

**6. Check Jenkins status:**
```bash
sudo systemctl status jenkins
```

✔️ The output should show that Jenkins is running.

**7. Access Jenkins:**

-Open a browser → http://<your-server-ip>:8080

- Complete the setup:  
  - Unlock Jenkins with admin password
  - Install suggested plugins
  - Configure security

**8. Secure Jenkins with Firewall (UFW):**
```bash
sudo ufw allow 8080
```
By default Jenkins runs on port 8080.

---

### How to Create a Pipeline in Jenkins with GitHub Integration  

**1. Install Necessary Plugins**  
Go to **Manage Jenkins → Manage Plugins → Available** and install:  
- **Pipeline** plugin  
- **GitHub** plugin  
- **Strict Crumb Issuer** plugin  

**2. Create a New Pipeline Project**  
- Click **New Item**  
- Select **Pipeline**  
- Enter project name → Click **OK**  

**3. Configure the Pipeline** 
- Scroll to the **Pipeline** section  
- Select **Pipeline script from SCM**  
- Choose **Git** as SCM  
- Enter your **GitHub repository URL**  
- (Optional) Specify the branch  

**4. Add GitHub Credentials**  
If your repository is private:  
- Go to **Credentials → Add**  
- Enter GitHub **username & token/password**  

**5. Save Pipeline Configuration**  


**6. Run the Pipeline** 
- On the project page → Click **Build Now**  
- Jenkins will automatically:  
  - Fetch code from GitHub  
  - Run the defined pipeline steps
  
---

## SonarQube 

SonarQube can be used to automatically scan code as part of a continuous integration or continuous delivery pipeline, ensuring that code quality issues are detected and addressed early in the development process.

It provides a **web-based dashboard** that displays code quality metrics, issues, and trends over time.  

---

### 🚀 How to Install SonarQube on a Server

**1. Install OpenJDK**  
> SonarQube v9.9 LTS requires **Java 17 runtime**.

```bash
# SSH to your Ubuntu server as a non-root user with sudo access
sudo apt-get install openjdk-11-jdk -y     # For older versions
sudo apt install openjdk-17-jdk -y         # For v9.9+
```

**2. Install and Configure PostgreSQL**

Add the PostgreSQL repository:
```bash
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
```

Add the PostgreSQL signing key:
```bash
wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -
```

Install PostgreSQL:
```bash
sudo apt install postgresql postgresql-contrib -y
```

Enable and start PostgreSQL:
```bash
sudo systemctl enable postgresql
sudo systemctl start postgresql
```

Change default PostgreSQL password:
```bash
sudo passwd postgres
```

Switch to postgres user:
```bash
su - postgres
```

Create SonarQube user:
```bash
createuser sonar
```

Log into PostgreSQL:
```bash
psql
```

Set password and database:
```bash
ALTER USER sonar WITH ENCRYPTED password 'my_strong_password';
CREATE DATABASE sonarqube OWNER sonar;
GRANT ALL PRIVILEGES ON DATABASE sonarqube TO sonar;
\q
```

Return to non-root sudo user:

exit

**3. Download and Install SonarQube**

Install zip utility:
```bash
sudo apt-get install zip -y
```

Download SonarQube (replace `<VERSION_NUMBER>`):
```bash
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-<VERSION_NUMBER>.zip
```

Unzip and move files:
```bash
sudo unzip sonarqube-<VERSION_NUMBER>.zip
sudo mv sonarqube-<VERSION_NUMBER> /opt/sonarqube
```

**4. Add SonarQube User & Permissions**
```bash
sudo groupadd sonar
sudo useradd -d /opt/sonarqube -g sonar sonar
sudo chown sonar:sonar /opt/sonarqube -R
```

**5. Configure SonarQube**

Edit configuration file:
```bash
sudo nano /opt/sonarqube/conf/sonar.properties
```

Update DB credentials:
```bash
sonar.jdbc.username=sonar
sonar.jdbc.password=my_strong_password
sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube
```

Edit sonar.sh file:
```bash
sudo nano /opt/sonarqube/bin/linux-x86-64/sonar.sh
```

Find and update:
```bash
RUN_AS_USER=sonar
```
**6. Setup Systemd Service**

Create service file:
```bash
sudo nano /etc/systemd/system/sonar.service
```

Paste configuration:
```bash
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking
ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop
User=sonar
Group=sonar
Restart=always
LimitNOFILE=65536
LimitNPROC=4096

[Install]
WantedBy=multi-user.target
```

Enable and start service:
```bash
sudo systemctl enable sonar
sudo systemctl start sonar
sudo systemctl status sonar
```

**7. Modify Kernel System Limits**
```bash
sudo nano /etc/sysctl.conf
```

Add:
```bash
vm.max_map_count=262144
fs.file-max=65536
ulimit -n 65536
ulimit -u 4096
```

Reboot to apply changes:
```bash
sudo reboot
```
**8. Access SonarQube Web Interface**

Open in browser:
```bash
http://<SERVER_IP>:9000
```

Default login:

- **Username**: `admin`

- **Password**: `admin`

On first login, you’ll be prompted to change the password.

---

### How to Integrate SonarQube with Jenkins

 **1. Install the SonarQube Plugin**
- In Jenkins, go to **Manage Jenkins → Manage Plugins → Available**  
- Search for **SonarQube Scanner** and install it.

**2. Configure the SonarQube Server in Jenkins**
- Navigate to **Manage Jenkins → Configure System**  
- Scroll to **SonarQube servers → Add SonarQube**  
- Enter:
  - A **name** for your server
  - The **SonarQube URL**
  - The **SonarQube access token**

**3. Configure the SonarQube Scanner Tool**
- Download & install **SonarQube Scanner** on your Jenkins server.  
- In Jenkins, go to **Manage Jenkins → Global Tool Configuration**  
- Add the SonarQube Scanner tool.

**4. Create a SonarQube Project**
- In the **SonarQube web interface**, create a new project.  
- Generate a **project token** for authentication.

**5. Configure the Jenkins Pipeline**
1. Add a webhook to your GitHub repository:  
```bash
http://your_jenkins_ip:8080/github-webhook/
```
2. In your GitHub repo, create a file called **`Jenkinsfile`** with the following content:

```groovy
node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def scannerHome = tool 'SonarScanner';
    withSonarQubeEnv() {
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
}
```
Make sure:
-The SonarScanner tool name matches what you configured in Jenkins.
-If multiple servers exist, specify the correct one in the script.

**6. Run the Jenkins Pipeline**

-Run the pipeline in Jenkins.

-The code will be analyzed and results sent to SonarQube.

-View detailed reports in the SonarQube web interface.

