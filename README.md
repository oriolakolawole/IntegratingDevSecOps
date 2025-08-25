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

## Integrating Git Secret with GitLab  

### 1. Install Git Secrets  
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
