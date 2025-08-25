# Development Phase in DevSecOps

The **development phase** in DevSecOps refers to writing and testing code for a software application.  
It includes:

- Writing code  
- Building and testing software  
- Integrating **security testing and validation** into the process  

The goal is to ensure that code is **secure and functional** before deployment.  

DevSecOps (Development, Security, and Operations) emphasizes integrating security practices and tools **throughout development** rather than treating security as an afterthought.  

---

## IDE Plugins in DevSecOps

IDE (**Integrated Development Environment**) plugins extend the functionality of an IDE.  
Examples include:

- Code generation  
- Code formatting  
- Debugging  
- Source control integration  
- Testing  
- Language support  
- Code analysis and security  
- Database management  
- Project management  

Plugins can be installed and managed directly within the IDE, allowing developers to customize and extend their environment.

---

## Connecting SonarLint Plugin to Visual Studio Code

SonarLint integrates with **VS Code** to analyze code and detect issues. You can connect it to either **SonarQube** or **SonarCloud**.

---

### 🔹 Connecting to a SonarQube Server

1. Install **Visual Studio Code**.  
2. Open VS Code → go to **Extensions** (`Ctrl+Shift+X` or `Cmd+Shift+X` on macOS).  
3. Search for **"SonarLint"** and install the extension by **SonarSource**.  
4. Open your project in VS Code.  
5. Go to: **Analyze → Manage SonarQube Connections**.  
6. Click **Connect**.  
7. Enter your **SonarQube server URL, username, and password**.  
8. Once connected, SonarLint will link to the server.  
9. Click **Analyze** to start scanning your code.  
10. Issues will appear in the **SonarLint panel** inside VS Code.  

---

### 🔹 Connecting to a SonarCloud Server

1. Go to [SonarCloud.io](https://sonarcloud.io).  
2. Navigate to: **My Account → Security → Generate Tokens**.  
3. Enter a token name → click **Generate**.  
4. In VS Code, follow steps **1–5** above.  
5. Instead of username/password, enter the **token** generated in step 3.  
6. Once connected, SonarLint will link to your **SonarCloud** account.  
7. Click **Analyze** to start scanning.  
8. Issues will appear in the **SonarLint panel**.  

---

✅ With this setup, every time you write or edit code in VS Code, SonarLint will help enforce secure coding practices.
