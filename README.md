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
- The SonarScanner tool name matches what you configured in Jenkins.
- If multiple servers exist, specify the correct one in the script.

**6. Run the Jenkins Pipeline**

- Run the pipeline in Jenkins.

- The code will be analyzed and results sent to SonarQube.

- View detailed reports in the SonarQube web interface.

---

### Connecting SonarQube with GitLab

Integrating **SonarQube** with **GitLab** allows you to run code analysis on your GitLab repository and display the results in the SonarQube interface.

**1. Set Up SonarQube**
- Install SonarQube on your machine (download from [SonarQube](https://www.sonarqube.org/)) or use a hosted version.  


**2. Create a Project in SonarQube**
- Log into the SonarQube web interface.  
- Click **"Create Project"** and follow the setup steps.  


**3. Generate a Token**
- In the SonarQube web interface, go to **Security** → **Generate Token**.  
- Save the token securely, you’ll need it for integration.  


**4. Integrate SonarQube with GitLab**
- Go to your GitLab project → **Settings** → **Integrations**.  
- Add the **SonarQube service**.  
- Enter:
  - **SonarQube URL**
  - **Generated token**

**5. Create GitLab CI/CD Pipeline**
In your GitLab repository, create a new file named **`.gitlab-ci.yml`** with the following content:

```yaml
sonarqube:
  image: sonarqube:7.9-community
  script:
    - sonar-scanner
  variables:
    SONAR_TOKEN: "YOUR_TOKEN"
    SONAR_HOST_URL: "http://your-sonarqube-url"
```
➡️ Replace:

- `YOUR_TOKEN` → with your SonarQube token

- `http://your-sonarqube-url` → with your SonarQube instance URL

**6. Configure Pipeline Trigger**

- Go to your GitLab project → Settings → CI/CD.

- Configure the pipeline to run on every push event

---

# Post Build Phase

The post-build phase in DevSecOps refers to the activities that occur after the application has been built and before it is deployed to production. This phase is critical for ensuring the application's security and typically focuses on testing and validation.

---

## How to install Apache, Mysql, PHP on Ubuntu Server
### Installing Apache and Updating Firewall

Update package manager:

```bash
sudo apt update
```
Install Apache:
```bash
sudo apt install apache2
```

Adjust your firewall settings to allow HTTP traffic:
```bash
sudo ufw allow in "Apache"
sudo ufw status
```

Verify by visiting your server’s public IP in a browser:
```bash
http://your_server_ip
```
👉 You should see Apache2 default page.

### Installing Mysql

Install MySQL:
```bash
sudo apt install mysql-server
```

Run security script:
```bash
sudo mysql_secure_installation
```

If you see error like:
```bash
Failed! Error: SET PASSWORD has no significance...
```

Run:
```bash
sudo mysql
```

Inside MySQL:
```bash
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
exit
```

Then rerun:
```bash
sudo mysql_secure_installation
```

Test login:
```bash
sudo mysql
# OR
mysql -u root -p
```

Exit:
```bash
exit
```
### Installing PHP

Install PHP:
```bash
sudo apt install php libapache2-mod-php php-mysql
```

Check PHP version:
```bash
php -v
```

Example output:
```bash
PHP 8.1.2 (cli) (built: Mar  4 2022 18:13:46) (NTS)
```
### Creating a Virtual Host for your Website

Create directory:
```bash
sudo mkdir /var/www/your_domain
sudo chown -R $USER:$USER /var/www/your_domain
```

Create config file:
```bash
sudo nano /etc/apache2/sites-available/your_domain.conf
```

Paste config:
```bash
<VirtualHost *:80>
    ServerName your_domain
    ServerAlias www.your_domain 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/your_domain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Enable site:
```bash
sudo a2ensite your_domain
sudo a2dissite 000-default
sudo apache2ctl configtest
sudo systemctl reload apache2
```

Create test HTML:
```bash
nano /var/www/your_domain/index.html

<html>
  <head>
    <title>your_domain website</title>
  </head>
  <body>
    <h1>Hello World!</h1>
    <p>This is the landing page of <strong>your_domain</strong>.</p>
  </body>
</html>
```

### Testing PHP Processing on Your Web Server
Create PHP test file:
```bash
nano /var/www/your_domain/info.php
```

Paste:
```bash
<?php
phpinfo();
```

Access:
```bash
http://server_domain_or_IP/info.php
```

Remove after test:
```bash
sudo rm /var/www/your_domain/info.php
```
---

### Creating SSH Login for Root User

Switch to root:
```bash
sudo su
```

Edit sshd config:
```bash
vi /etc/ssh/sshd_config
```


Update:
```bash
PasswordAuthentication yes
PermitRootLogin yes
```

Save and restart SSH:
```bash
systemctl restart sshd
```
Set root password:
```bash
passwd
```
---

### How to Automatically Deploy your Website From Github
Prerequisites
- Online remote repository (GitHub/Bitbucket)
- Local git repo
- Cloud server (AWS, GCP, Rackspace, etc.)
- Apache with `/var/www/html/`

**On Local Machine**

Add deployment script:
```bash
git add deploy.php
git commit -m 'Added the git deployment script'
git push -u origin master
```

**On Your Server**

Install Git and check version:
```bash
git --version
```

Setup Git:
```bash
git config --global user.name "Server"
git config --global user.email "server@server.com"
```

Setup SSH for Apache:
```bash
sudo mkdir /var/www/.ssh
sudo chown -R apache:apache /var/www/.ssh/
sudo -Hu apache ssh-keygen -t rsa
sudo cat /var/www/.ssh/id_rsa.pub
```
**On GitHub**
1. Add SSH key to your GitHub account:
- https://github.com/settings/ssh
- Create a new key
- Paste the deploy key you generated on the server

2. Create webhook:
Go to Settings > Webhooks
- Add:
```bash
http://server.com/deploy.php
```
**On the Server**

Clone repo:
```bash
sudo chown -R www-data:www-data /var/www/html
sudo -Hu www-data git clone git@github.com:you/server.git /var/www/html
```
Now your website auto-deploys from GitHub.

## OWASP ZAP

**OWASP ZAP (Zed Attack Proxy)** is a popular open-source web application security scanner. It can be used to find security vulnerabilities in web applications automatically.

---

### INTEGRATING OWASP ZAP INTO DEVSECOPS WORKFLOW

1. **Install OWASP ZAP**  
   The first step is to install OWASP ZAP on the system where it will be run. This can be done by downloading the appropriate version from the [official website](https://owasp.org/www-project-zap/) and following the installation instructions.

2. **Configure OWASP ZAP**  
   Once installed, configure OWASP ZAP to suit your needs. This can include setting up authentication, proxy settings, and scan policies. You can also create custom scripts to automate certain tasks, such as logging in to a website before scanning.

3. **Integrate with the build process**  
   Integrate OWASP ZAP into your build process by running a scan as part of the build pipeline. This can be done using tools such as Jenkins or Travis CI.  
   Example: Create a Jenkins job that runs a scan on the application using OWASP ZAP before deploying it to the next environment.

4. **Automate the scan**  
   Automate the scan using the command-line interface of OWASP ZAP. This allows you to run scans as part of a script or continuous integration process.  
   Example: Create a script that starts the scan and waits for it to complete before moving on to the next step in the pipeline.

5. **Analyse the results**  
   After completing the scan, analyse the results to identify vulnerabilities. The results can be exported in different formats, such as **XML**, **JSON**, and **HTML**.

6. **Integrate with issue tracking system**  
   Integrate the results of the scan with an issue tracking system, such as **JIRA**, so any identified vulnerabilities can be tracked and fixed.  
   Export the results from OWASP ZAP in a format that can be imported into the issue tracking system.

---

### HOW TO ADD OWASP ZAP SCAN TO YOUR JENKINS PIPELINE

#### How to run OWASP ZAP Docker Image

**Requirements**
You need Docker installed on your server.  
👉 [Click here for Docker installation instructions](https://docs.docker.com/get-docker/)  

You can install Docker image with OWASP ZAP pre-installed using the following command:
```bash
$ docker pull owasp/zap2docker-stable
```
Run ZAP in Headless Mode:
```bash
$ docker run -u zap -p 8080:8080 -i owasp/zap2docker-stable zap.sh -daemon -host 0.0.0.0 -port 8080
```
**Create SSH Login For Root User In Your ZAP Server**

Edit the SSH configuration file:
```bash
# vi /etc/ssh/sshd_config
```

Change the following line:
```bash
PubKeyAuthentication no  =>  PubKeyAuthentication yes
```

Save and exit with:
```bash
:wq
```
**Create an SSH Key on Your ZAP Server**

Generate a new key:
```bash
$ ssh-keygen
```

(Press Enter for all prompts.)

Add your public key to authorized keys:
```bash
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

Copy your private key:
```bash
$ vi ~/.ssh/id_rsa
```

(Copy all the content.)

**Adding ZAP to Your Jenkins Pipeline**

1. In your Jenkins dashboard:
- Go to Manage Jenkins
- Go to Manage Plugins
- Click on Available Plugins
- Search for SSH Agent and install it.

2. Add credentials in Jenkins:
- Go to Manage Jenkins → Manage Credentials → Add Credentials
- Fill the form as follows:
 - Kind: SSH Username with private key
 - Scope: Global (Jenkins, nodes, items, all child items, etc)
 - ID: yourcredentialid
 - Username: root
 - Private key: Paste the private key you copied earlier
 - Passphrase: Password of your root server
 - Click Create

3. Modify your Jenkinsfile in your repository by adding this stage:
```groovy
stage('OWASP ZAP ANALYSIS') {
    sshagent (credentials: ['yourcredentialid']) {
        sh '''
        ssh -o StrictHostKeyChecking=no root@<yourzapserverip> \
        "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://<yourwebserverip>/" || true
        '''
    }
}
```

Save and commit changes to your repository.
Jenkins will now run OWASP ZAP scan as part of your pipeline.

---

## CONTAINER VULNERABILITY SCANNING WITH TRIVY

### What is Trivy?
**Trivy** is an open-source vulnerability scanner for container images and other software packages.  
It helps developers and security teams identify vulnerabilities in their container images and dependencies, so they can remediate issues before exploitation.

---

### How to Integrate Trivy with Jenkins

 **1. Install Trivy in your server**
```bash
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy
```

OR install with cURL:
```bash
curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin v0.18.3
```
**2. Verify installation**
```bash
trivy --version
```
**3. Install Jenkins Plugin**

Install the HTML Publisher Plugin from Jenkins Plugin Manager.

**4. Add Trivy Scan Stage to Jenkinsfile**
```groovy
stage('Scan') {
    steps {
        // Install trivy
        sh 'curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin v0.18.3'
        sh 'curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/html.tpl > html.tpl'

        // Scan all vuln levels and export HTML report
        sh 'mkdir -p reports'
        sh 'trivy filesystem --ignore-unfixed --vuln-type os,library --format template --template "@html.tpl" -o reports/nodejs-scan.html ./nodejs'

        publishHTML target: [
            allowMissing: true,
            alwaysLinkToLastBuild: true,
            keepAll: true,
            reportDir: 'reports',
            reportFiles: 'nodejs-scan.html',
            reportName: 'Trivy Scan',
            reportTitles: 'Trivy Scan'
        ]

        // Scan again and fail if CRITICAL vulnerabilities exist
        sh 'trivy filesystem --ignore-unfixed --vuln-type os,library --exit-code 1 --severity CRITICAL ./nodejs'
    }
}
```
---
# Monitoring and Reporting Phase

## ARCHERYSEC

ArcherySec provides a centralized platform for identifying, tracking, and managing vulnerabilities across an organization's entire infrastructure.  
It can be integrated with popular issue tracking and collaboration tools such as **JIRA** and **Slack**, enabling security teams to quickly identify and remediate vulnerabilities.

---

### How to Install ArcherySec on a Server

```bash
$ git clone https://github.com/archerysec/archerysec.git
$ cd archerysec
$ NAME=User EMAIL=user@user.com PASSWORD=admin@123A bash setup.sh
$ ./run.sh
```
Once installed, navigate to:
👉 `http://<your_ip_address>:8000`

---
### How to Connect ArcherySec to Jira

1. Open your ArcherySec dashboard and navigate to the **Settings** menu.  
2. Click on **Integrations** in the dropdown menu and then click on the **Add** button next to **Jira** integration.  
3. In the **Add Integration** modal, enter the following information:  
   - **Name**: Give a name to the Jira integration.  
   - **Base URL**: Enter the base URL of your Jira instance.  
   - **Username**: Enter your Jira username.  
   - **Password**: Enter your Jira API token or password.  
4. Click on the **Test Connection** button to verify the connection to your Jira instance.  
5. Once the connection is successful, click on the **Save** button to save the Jira integration settings.  
6. Navigate to the **Projects** menu and select the project for which you want to create a Jira issue.  
7. In the project dashboard, click on the **Issues** tab and then click on the **Create Issue** button.  
8. In the **Create Issue** modal, fill in the necessary details of the issue, such as the summary, description, and issue type.  
9. In the **Details** section, select the **Jira** integration from the **Integration** dropdown menu.  
10. Fill in the relevant fields for the Jira issue, such as the project, assignee, and priority.  
11. Click on the **Create Issue** button to create the Jira issue.  
12. You can now view the Jira issue details in the **Issues** tab of your ArcherySec project dashboard.  

---

### How to Connect ArcherySec to OWASP ZAP

1. Open your ArcherySec dashboard and navigate to the **Settings** menu.  
2. Click on **Integrations** in the dropdown menu and then click on the **Add** button next to **ZAP** integration.  
3. In the **Add Integration** modal, enter the following information:  
   - **Name**: Give a name to the ZAP integration.  
   - **Base URL**: Enter the base URL of your ZAP instance.  
   - **API Key**: Enter the API key for your ZAP instance.  
4. Click on the **Test Connection** button to verify the connection to your ZAP instance.  
5. Once the connection is successful, click on the **Save** button to save the ZAP integration settings.  
6. Open your OWASP ZAP application and navigate to the **Tools** menu.  
7. Click on **Options** in the dropdown menu and then click on the **API** tab.  
8. In the **API** tab, enable the following options:  
   - **Enable API**: Check this box to enable the ZAP API.  
   - **Allow remote clients to connect**: Check this box to allow ArcherySec to connect to the ZAP API.  
9. Click on the **Save** button to save the API settings.  
10. Navigate to the **Projects** menu in your ArcherySec dashboard and select the project for which you want to scan with ZAP.  
11. In the project dashboard, click on the **Scan** tab and then click on the **Add Scan** button.  
12. In the **Add Scan** modal, select **ZAP** as the scanner and enter the necessary details, such as the target URL and scan options.  
13. Click on the **Save** button to save the scan settings.  
14. Click on the **Start Scan** button to start the ZAP scan.  
15. Once the scan is complete, you can view the results in the **Issues** tab of your project dashboard.

## JIRA

Jira is a popular project management and issue tracking software developed by Atlassian. It is designed to help teams of all sizes manage their projects and track their progress through various stages of development.



### HOW TO CREATE A PROJECT AND ISSUE TICKETS IN JIRA

**Creating a Project**
1. Log in to your Jira account and navigate to the Jira dashboard.  
2. Click on the **"Create project"** button located in the left sidebar of the dashboard.  
3. On the "Create Project" page, select the project type you want to create (e.g., Scrum, Kanban, etc.).  
4. Enter the project details such as **project name, project key, project lead, and project description**.  
5. Select the appropriate project template (if available) or leave it as **None**.  
6. Click on the **"Create"** button to create your project.  


**Creating an Issue**
1. Once you have created your project, navigate to the project page by clicking on the project name from the Jira dashboard.  
2. Click on the **"Create"** button located in the left sidebar of the project page.  
3. On the "Create Issue" page, select the issue type you want to create (e.g., **Bug, Task, Story**, etc.).  
4. Enter the issue details such as **summary, description, priority, and assignee**.  
5. Add any additional details you want, such as **attachments or comments**.  
6. Click on the **"Create"** button to create your issue.  

**Assigning an Issue**
1. Once you have created your issue, it will be added to the project backlog or board depending on the project type you have created.  
2. To assign an issue to someone, open the issue by clicking on the **issue key**.  
3. Click on the **"Assign"** button located in the issue details section.  
4. Select the user you want to assign the issue to from the list of available users.  
5. Click on the **"Assign"** button to assign the issue to the selected user.  

