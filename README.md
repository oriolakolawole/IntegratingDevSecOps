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

# Pre-Build Phase in DevSecOps  

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

Open a browser → http://<your-server-ip>:8080

Complete the setup:

Unlock Jenkins with admin password

Install suggested plugins

Configure security

**8. Secure Jenkins with Firewall (UFW):**

sudo ufw allow 8080


By default Jenkins runs on port 8080.
