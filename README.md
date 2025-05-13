## ✅ Objective

Set up a workflow that:
- Automatically builds a Node.js app when you push code to GitHub.
- Logs the build output to a `build.log` file.
- Prepares you for storing these logs permanently in future tasks.

---

## 🛠️ Step 1: Set Up Prerequisites

Make sure the following are installed and working on your Windows system:

### 1.1 Install Git
- Go to [git-scm.com](https://git-scm.com) and download the Windows installer.
- Run the installer with **default options**. Make sure to choose **“Use Git from the Windows Command Prompt.”**
- Open **PowerShell** (Win + S → PowerShell → Enter).
- Verify Git installation:

```bash
git --version
```

You should see something like: `git version 2.x.x`.

---

### 1.2 Install Node.js

* Go to [nodejs.org](https://nodejs.org) and download the **LTS version** (e.g., 16.x or 18.x).
* Install it using the default options.
* Verify installation:

```bash
node --version
npm --version
```

You should see output like: `v16.x.x` for Node, `8.x.x` for npm.

---

### 1.3 Install Visual Studio Code

* Download from [code.visualstudio.com](https://code.visualstudio.com/).
* Run the installer with default settings.
* Open it to confirm: Press Win + S → Visual Studio Code → Enter.

---

### 1.4 Create a GitHub Account (if you don't have one)

* Go to [github.com](https://github.com) and sign up.
* Verify your email (check inbox/spam folder).

---

## 📁 Step 2: Create a New GitHub Repository

1. Log in to GitHub.
2. Click the **"+" icon** (top-right) → **New repository**.
3. Set the name to: `devops-practical`
4. Choose: **Public**, check **"Add a README file"**, then click **Create repository**.

---

## 💻 Step 3: Clone the Repository Locally

Open **PowerShell**:

```bash
mkdir C:\Projects
cd C:\Projects
git clone https://github.com/YOUR-USERNAME/devops-practical.git
cd devops-practical
code .
```

> `code .` opens the project in VS Code.

---

## 🌐 Step 4: Create a Sample Node.js App

### 4.1 Initialize the Node.js Project

```bash
npm init -y
```

This creates a `package.json` file with default values.

---

### 4.2 Install Express

```bash
npm install express
```

This adds Express to your project and generates a `node_modules` folder and a `package-lock.json` file.

---

### 4.3 Create `app.js`

In **VS Code**:

* Right-click the Explorer sidebar → New File → name it `app.js`
* Paste the code below:

```javascript
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello, DevOps Practical!');
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

> This creates a basic web server that responds with a message at `http://localhost:3000`.

---

### 4.4 Add a Start Script

In `package.json`, update the "scripts" section:

```json
"scripts": {
  "start": "node app.js",
  "test": "echo \"Error: no test specified\" && exit 1"
}
```

Now you can run the app using:

```bash
npm start
```

Check in your browser: [http://localhost:3000](http://localhost:3000)

Press `Ctrl + C` in PowerShell to stop the server.

---

### 4.5 Set Up `.gitignore`

Exclude unnecessary files:

```bash
echo node_modules/ > .gitignore
echo .env >> .gitignore
```

---

### 4.6 Commit and Push to GitHub

```bash
git add .
git commit -m "Add sample Node.js app"
git push origin main
```

---

## ⚙️ Step 5: Set Up GitHub Actions (CI/CD Pipeline)

### 5.1 Create GitHub Actions Folder

```bash
mkdir .github/workflows
```

In VS Code, you’ll now see `.github/workflows`.

---

### 5.2 Create the Workflow File

Inside `.github/workflows`, create `ci.yml` and paste:

```yaml
name: CI Pipeline

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '16'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test || echo "No tests defined" | tee build.log

      - name: Save build logs
        run: |
          echo "Build completed at $(date)" >> build.log
```

### 💡 Why These Steps?

| Step                     | Purpose                                          |            |                                                                |
| ------------------------ | ------------------------------------------------ | ---------- | -------------------------------------------------------------- |
| `on: push`               | Triggers workflow on every push to `main` branch |            |                                                                |
| `runs-on: ubuntu-latest` | Uses a Linux VM to execute the job               |            |                                                                |
| `checkout`               | Downloads your repo code into the runner         |            |                                                                |
| `setup-node`             | Installs Node.js (v16) on the VM                 |            |                                                                |
| `npm install`            | Installs your project’s dependencies             |            |                                                                |
| \`npm test               |                                                  | echo ...\` | Prevents the job from failing; creates `build.log` if no tests |
| `echo ... >> build.log`  | Appends build completion time to log file        |            |                                                                |

---

## 🧪 Step 6: Add a Dummy Test Script

Edit `package.json`, and update:

```json
"scripts": {
  "start": "node app.js",
  "test": "echo 'Running tests...' && exit 0"
}
```

This allows the test step to pass even without real tests.

---

## ✅ Step 7: Commit and Push Workflow

```bash
git add .
git commit -m "Add CI pipeline with build log saving"
git push origin main
```

---

## 📊 Step 8: Verify Workflow on GitHub

1. Go to your GitHub repo.
2. Click the **Actions** tab.
3. Look for the **“CI Pipeline”** run.
4. Click into it → View **build** job steps.
5. Look for:

   * `Run tests`: confirms `build.log` was created.
   * `Save build logs`: confirms log was appended.

> 📝 Note: The `build.log` file is **not saved in your repository** yet—it exists only temporarily in the GitHub Actions runner (a virtual machine). In **Task 22**, you will configure it to persist as an artifact.

---

## 📁 Project Structure

```
devops-practical/
│
├── app.js
├── package.json
├── .gitignore
├── .github/
│   └── workflows/
│       └── ci.yml
└── node_modules/ (ignored by git)
```

---

## 🎉 Done!

You now have a working:

* Node.js app
* GitHub repository
* CI pipeline via GitHub Actions
* Build log saved as `build.log` (temporary for now)

---

Need help with the next step (Task 22: Saving logs as artifacts)? Just ask!

```

---

Let me know if you want me to generate this file directly or help you upload it to your repo!
```
