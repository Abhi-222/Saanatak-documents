# Common Stack | Applications | React JS | Installation Guide

---

# Author Table

| Author | Created On | Version | Last Updated  | L0 Reviewer | L1 Reviewer    | L2 Reviewer |
|--------|------------|---------|---------------|-------------|----------------|-------------|
| Sahil  | 28-08-26   | v1.0    |  28-08-26     | `Divya M`   | `Aayush Verma` | `Mahesh Kumar / Varun` |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Purpose](#2-purpose)
3. [What is React JS](#3-what-is-react-js)
4. [Why React JS is Required](#4-why-react-js-is-required)
5. [Key Features of React JS](#5-key-features-of-react-js)
6. [Prerequisites](#6-prerequisites)
7. [System Requirements](#7-system-requirements)
8. [React JS Installation](#8-react-js-installation)
   - [8.1 Linux (Ubuntu / Debian)](#81-linux-ubuntu--debian)
   - [8.2 Windows](#82-windows)
   - [8.3 macOS](#83-macos)
9. [Verification](#9-verification)
10. [Environment Configuration](#10-environment-configuration)
11. [Maintenance](#11-maintenance)
12. [Troubleshooting](#12-troubleshooting)
13. [Quick Commands](#13-quick-commands)
14. [Conclusion](#14-conclusion)
15. [Contact Information](#15-contact-information)
16. [References](#16-references)

---

# 1. Introduction

React JS is an open-source JavaScript library maintained by Meta, used for building fast and interactive user interfaces, primarily for single-page applications (SPAs).

This document provides a basic procedure to install and configure a React JS application on **Linux, Windows, and macOS**.

---

# 2. Purpose

The purpose of this document is to provide a simple procedure to:

- Install Node.js and npm on Linux, Windows, or macOS
- Create a new React application
- Install project dependencies
- Verify the React installation
- Run the React development server
- Perform basic maintenance and troubleshooting

---

# 3. What is React JS

React JS is a component-based JavaScript library used to build user interfaces. Instead of manipulating the browser's DOM directly, React uses a **Virtual DOM** to track changes and update only the parts of the actual DOM that have changed, improving performance.

Applications built with React are structured as a tree of components — small, independent, and reusable pieces of UI (buttons, forms, cards, etc.) that manage their own logic and rendering, and can be composed together to build complex interfaces.

---

# 4. Why React JS is Required

React JS is required in modern application development because it addresses several common front-end challenges:

- **Faster UI updates** — the Virtual DOM minimizes expensive direct DOM operations.
- **Reusability** — components can be reused across different parts of an application, reducing duplication.
- **Maintainability** — breaking UI into components makes large applications easier to manage and debug.
- **Strong ecosystem** — a large community, extensive libraries, and tooling support (React Router, Redux, Next.js, etc.).
- **Industry adoption** — widely used, making it easier to hire talent and find support/resources.

---

# 5. Key Features of React JS

| **Feature** | **Description** |
| ----------- | ---------------- |
| Virtual DOM | Maintains a lightweight copy of the real DOM in memory and updates only changed elements, improving rendering performance. |
| Component-Based Architecture | UI is broken into independent, reusable components, making development modular and easier to maintain. |
| JSX (JavaScript XML) | Allows writing HTML-like syntax within JavaScript, making UI code more readable and intuitive. |
| One-Way Data Binding | Data flows in a single direction (parent to child), making the application more predictable and easier to debug. |
| Hooks | Functions like `useState` and `useEffect` allow functional components to manage state and lifecycle behavior without needing class components. |

---

# 6. Prerequisites

Before starting the installation, ensure the following are available:

- A computer running **Linux (Ubuntu/Debian), Windows 10/11, or macOS**
- Terminal (Linux/macOS) or Command Prompt/PowerShell (Windows) access
- Internet connectivity
- Administrator privileges (`sudo` on Linux/macOS, Administrator on Windows)
- Basic command-line knowledge

---

# 7. System Requirements

| Requirement | Minimum Recommendation |
|-------------|-------------------------|
| RAM | 4 GB or higher |
| Disk Space | 5 GB or higher |
| Operating System | Ubuntu / Debian, Windows 10 or later, or macOS 11 or later |
| Internet | Required |
| Privileges | `sudo` (Linux/macOS) or Administrator (Windows) |

### Important Ports

| Port | Description |
|------|-------------|
| 22 | SSH access to a remote Linux server (not applicable for local Windows/macOS setups) |
| 3000 | Default React development server port |
| 443 | HTTPS communication |

---

# 8. React JS Installation

## 8.1 Linux (Ubuntu / Debian)

### Step 1: Update Package Repository

Update the package information before installing Node.js.

```bash
sudo apt update
```

<details>
<summary><strong>Screenshot - Package repository updated</strong></summary>

<img width="740" height="225" alt="Screenshot 2026-09-04 at 6 27 19 PM" src="https://github.com/user-attachments/assets/a47be994-98f3-4bc5-80d0-b58e354b8f5a" />

</details>

---

### Step 2: Install Node.js

Install Node.js, which includes npm (Node Package Manager).

```bash
sudo apt install nodejs -y
```

<details>
<summary><strong>Screenshot - Node.js installed</strong></summary>

<img width="1046" height="371" alt="Screenshot 2026-09-04 at 6 27 40 PM" src="https://github.com/user-attachments/assets/19cacc15-586e-4a16-ad11-f0f6bbe60a4b" />

</details>

---

### Step 3: Install npm (if not already installed)

Install npm separately if it was not installed along with Node.js.

```bash
sudo apt install npm -y
```

<details>
<summary><strong>Screenshot - npm installed</strong></summary>

<img width="1046" height="575" alt="Screenshot 2026-09-04 at 6 28 51 PM" src="https://github.com/user-attachments/assets/720efa93-ad78-4484-949f-a3a769678acb" />

</details>

---

### Step 4: Create a New React Application

```bash
npx create-react-app my-app
```

<details>
<summary><strong>Screenshot - React application created</strong></summary>

<img width="936" height="464" alt="Screenshot 2026-09-04 at 6 31 18 PM" src="https://github.com/user-attachments/assets/babd5127-95c5-4cf9-b64c-d0c0a7b248ea" />

</details>

---

### Step 5: Navigate to the Project Directory

```bash
cd my-app
```

<details>
<summary><strong>Screenshot - Inside the project directory</strong></summary>

<img width="1376" height="62" alt="Screenshot 2026-09-04 at 7 43 36 PM" src="https://github.com/user-attachments/assets/f3a98ae5-16fc-4979-b1c3-5a5fc9ec3ee8" />

</details>

---

### Step 6: Start the Development Server

```bash
npm start
```

<details>
<summary><strong>Screenshot - Development server started</strong></summary>

<img width="466" height="209" alt="Screenshot 2026-09-04 at 6 32 03 PM" src="https://github.com/user-attachments/assets/a92f876a-6181-42ec-8ca2-472bb128d377" />

</details>

---

## 8.2 Windows

### Step 1: Download and Install Node.js

Download the **LTS installer** for Windows from [nodejs.org](https://nodejs.org), then run it and follow the setup wizard. This installs Node.js and npm together (and adds them to your `PATH`).

<details>
<summary><strong>Screenshot - Node.js installer (Windows)</strong></summary>

<img width="622" height="97" alt="PLACEHOLDER - add screenshot" src="" />

</details>

---

### Step 2: Open Command Prompt or PowerShell

Open **Command Prompt** or **PowerShell** to run the remaining commands.

---

### Step 3: Create a New React Application

```powershell
npx create-react-app my-app
```

<details>
<summary><strong>Screenshot - React application created (Windows)</strong></summary>

<img width="622" height="97" alt="PLACEHOLDER - add screenshot" src="" />

</details>

---

### Step 4: Navigate to the Project Directory

```powershell
cd my-app
```

---

### Step 5: Start the Development Server

```powershell
npm start
```

<details>
<summary><strong>Screenshot - Development server started (Windows)</strong></summary>

<img width="622" height="97" alt="PLACEHOLDER - add screenshot" src="" />

</details>

---

## 8.3 macOS

### Step 1: Install Node.js

Either download the macOS installer from [nodejs.org](https://nodejs.org), or install via **Homebrew**:

```bash
brew install node
```

<details>
<summary><strong>Screenshot - Node.js installed (macOS)</strong></summary>

<img width="622" height="97" alt="PLACEHOLDER - add screenshot" src="" />

</details>

---

### Step 2: Open Terminal

Open the **Terminal** app to run the remaining commands.

---

### Step 3: Create a New React Application

```bash
npx create-react-app my-app
```

<details>
<summary><strong>Screenshot - React application created (macOS)</strong></summary>

<img width="622" height="97" alt="PLACEHOLDER - add screenshot" src="" />

</details>

---

### Step 4: Navigate to the Project Directory

```bash
cd my-app
```

---

### Step 5: Start the Development Server

```bash
npm start
```

<details>
<summary><strong>Screenshot - Development server started (macOS)</strong></summary>

<img width="622" height="97" alt="PLACEHOLDER - add screenshot" src="" />

</details>

---

# 9. Verification

## Step 1: Check Node.js, npm, and npx Versions

Verify the Node.js, npm, and npx installations. This command works the same in a Linux/macOS terminal and in Windows Command Prompt or PowerShell.

```bash
node --version
npm --version
npx --version
```

<details>
<summary><strong>Screenshot - Node.js, npm, and npx version output</strong></summary>

<img width="371" height="108" alt="Screenshot 2026-09-04 at 6 29 54 PM" src="https://github.com/user-attachments/assets/e6ea6a83-69ce-4645-a3bc-ce7fbd58db9a" />

</details>

---

## Step 2: Verify React Application is Running

Open a browser and navigate to:

    http://localhost:3000

**Note:** If the app is running on a remote server (e.g., an EC2 instance), replace `localhost` with the server's public IP address instead.

<details>
<summary><strong>Screenshot - React app running in browser</strong></summary>

<img width="1376" height="777" alt="Screenshot 2026-09-04 at 7 35 56 PM" src="https://github.com/user-attachments/assets/1c6a20df-49bc-4cbf-857d-ea9e65f11e73" />

</details>

---

# 10. Environment Configuration

## Step 1: Check Node.js Path

| Platform | Command |
|----------|---------|
| Linux / macOS | `which node` |
| Windows | `where node` |

<details>
<summary><strong>Screenshot - Node.js executable path</strong></summary>

<img width="622" height="97" alt="PLACEHOLDER - add screenshot" src="" />

</details>

---

## Step 2: Locate Node.js Installation

The `whereis` command displays common locations related to Node.js on Linux. On macOS this may return limited results; on Windows, use `where node` (Step 1) or check the default install path `C:\Program Files\nodejs\`.

```bash
whereis node
```

<details>
<summary><strong>Screenshot - Node.js installation locations (Linux)</strong></summary>

<img width="622" height="97" alt="Screenshot 2026-09-04 at 6 33 22 PM" src="https://github.com/user-attachments/assets/1f64c3e4-fefd-4e50-8239-ac21e88224bf" />

</details>

---

# 11. Maintenance

## Step 2: Install React Packages

Additional packages can be installed using npm — this works identically on Linux, Windows, and macOS.

```bash
npm install <package-name>
```

<details>
<summary><strong>Screenshot - Package installed via npm</strong></summary>

<img width="505" height="273" alt="Screenshot 2026-09-04 at 6 48 22 PM" src="https://github.com/user-attachments/assets/de8bd39d-80a6-47e7-b38f-13cbb7ddbcd5" />

</details>

## Step 3: Build for Production

Create an optimized production build of the React application.

```bash
npm run build
```

<details>
<summary><strong>Screenshot - Production build output</strong></summary>

<img width="601" height="455" alt="Screenshot 2026-09-04 at 6 48 54 PM" src="https://github.com/user-attachments/assets/9c69e4ea-7da8-4045-bd4c-cb4abbd84a10" />

</details>

---

# 12. Troubleshooting

| Issue | Possible Cause | Solution |
|-------|-----------------|----------|
| `node: command not found` (Linux/macOS) | Node.js is not installed | Install Node.js using `apt` (Linux) or `brew`/installer (macOS) |
| `'node' is not recognized as an internal or external command` (Windows) | Node.js is not installed, or not added to `PATH` | Reinstall Node.js and ensure "Add to PATH" is checked during setup |
| `npm: command not found` | npm is missing | Install npm separately, or reinstall Node.js (npm ships with it) |
| `npx: command not found` | npx is missing or npm is outdated | Upgrade npm to the latest version |
| `Permission denied` (Linux/macOS) | Insufficient permissions | Use `sudo`, or avoid global installs by using a Node version manager (e.g., `nvm`) |
| `EACCES` errors on global npm installs (macOS) | npm's default global directory requires elevated permissions | Use `nvm` to manage Node.js instead of a system-wide install |
| Port 3000 already in use | Another process is using the port | Stop the conflicting process or run on a different port |
| Package installation fails | Network or registry issue | Check connectivity and npm registry settings |

---

# 13. Quick Commands

| Task | Linux | Windows | macOS |
|------|-------|---------|-------|
| Update repositories | `sudo apt update` | — | — |
| Install Node.js | `sudo apt install nodejs -y` | Download LTS installer from [nodejs.org](https://nodejs.org) | `brew install node` |
| Install npm | `sudo apt install npm -y` | Included with Node.js installer | Included with Node.js installer |
| Check Node.js version | `node -v` | `node -v` | `node -v` |
| Check npm version | `npm -v` | `npm -v` | `npm -v` |
| Check npx version | `npx -v` | `npx -v` | `npx -v` |
| Create React app | `npx create-react-app my-app` | `npx create-react-app my-app` | `npx create-react-app my-app` |
| Start development server | `npm start` | `npm start` | `npm start` |
| Build for production | `npm run build` | `npm run build` | `npm run build` |
| Find Node.js path | `which node` | `where node` | `which node` |
| Locate Node.js | `whereis node` | `where node` | `which node` |
| Install a package | `npm install <package-name>` | `npm install <package-name>` | `npm install <package-name>` |

---

# 14. Conclusion

React JS can be installed on Linux, Windows, or macOS by first setting up Node.js and npm — either through the system package manager (Linux), the official installer (Windows), or Homebrew/the official installer (macOS).

After installation, the setup should be verified by checking Node.js, npm, and npx versions and confirming that the development server runs successfully, ensuring the system is ready for React application development regardless of platform.

---

# 15. Contact Information

| Name | Email ID |
|------|----------|
| Sahil | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co)|

---

# 16. References

| Resource | Description |
|----------|--------------|
| [React Official Website](https://react.dev/) | Official React JS website |
| [Node.js Official Website](https://nodejs.org/) | Official Node.js website (Windows/macOS/Linux installers) |
| [npm Documentation](https://docs.npmjs.com/) | Official npm documentation |
| [Create React App Documentation](https://create-react-app.dev/) | Official Create React App documentation |
| [Homebrew](https://brew.sh/) | Package manager used for installing Node.js on macOS |
