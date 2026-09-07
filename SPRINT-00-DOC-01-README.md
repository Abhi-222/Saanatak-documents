# Common Stack | Applications | React JS | Installation Guide

---

# Author Table

| Author | Created On | Version | Last Updated  | L0 Reviewer | L1 Reviewer    | L2 Reviewer |
|--------|------------|---------|---------------|-------------|----------------|-------------|
| Sahil  | 28-08-26   | v1.0    |  04-09-26     | `Divya M`   | `Aayush Verma` | `Mahesh Kumar / Varun` |

---

# Table of Contents

1. [Introduction](#1-introduction)
3. [What is React JS](#3-what-is-react-js)
4. [Why React JS is Required](#4-why-react-js-is-required)
5. [Key Features of React JS](#5-key-features-of-react-js)
6. [Prerequisites](#6-prerequisites)
7. [System Requirements](#7-system-requirements)
8. [React JS Installation](#8-react-js-installation)
9. [Verification](#9-verification)
12. [Troubleshooting](#12-troubleshooting)
13. [Quick Commands](#13-quick-commands)
14. [Conclusion](#14-conclusion)
15. [Contact Information](#15-contact-information)
16. [References](#16-references)

---

# 1. Introduction

React JS is an open-source JavaScript library maintained by Meta, used for building fast and interactive user interfaces, primarily for single-page applications (SPAs).
This document provides a basic procedure to install and configure a React JS application on a Linux system.

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

- Linux system such as Ubuntu or Debian
- Terminal access
- Sudo privileges
- Basic Linux command-line knowledge

---

# 7. System Requirements

This SOP has no strict OS version requirement — Node.js runs on virtually any Linux distribution. The table below reflects the bare minimum needed to install Node.js/npm and run a React development server, not a strict production sizing.

| Requirement | Minimum |
|-------------|-------------------------|
| RAM | 1 GB (2 GB+ recommended for smoother builds) |
| Disk Space | ~1 GB free (more as `node_modules` and build output grow) |
| Operating System | Any Linux distribution (Ubuntu, Debian, Fedora, and similar) |

### Important Ports

| Port | Description |
|------|-------------|
| 22 | SSH access to Linux server |
| 3000 | Default React development server port |
| 443 | HTTPS communication |

---

# 8. React JS Installation

## Step 1: Update Package Repository

Update the package information before installing Node.js.

```bash
sudo apt update
```

<details>
<summary><strong>Screenshot - Package repository updated</strong></summary>

<img width="740" height="225" alt="Screenshot 2026-09-04 at 6 27 19 PM" src="https://github.com/user-attachments/assets/a47be994-98f3-4bc5-80d0-b58e354b8f5a" />

</details>

---

## Step 2: Install Node.js

Install Node.js, which includes npm (Node Package Manager).

```bash
sudo apt install nodejs -y
```

<details>
<summary><strong>Screenshot - Node.js installed</strong></summary>

<img width="1046" height="371" alt="Screenshot 2026-09-04 at 6 27 40 PM" src="https://github.com/user-attachments/assets/19cacc15-586e-4a16-ad11-f0f6bbe60a4b" />

</details>

---

## Step 3: Install npm (if not already installed)

Install npm separately if it was not installed along with Node.js.

```bash
sudo apt install npm -y
```

<details>
<summary><strong>Screenshot - npm installed</strong></summary>

<img width="1046" height="575" alt="Screenshot 2026-09-04 at 6 28 51 PM" src="https://github.com/user-attachments/assets/720efa93-ad78-4484-949f-a3a769678acb" />

</details>

---

## Step 4: Create a New React Application

Use `npx` to create a new React project using Create React App.

```bash
npx create-react-app my-app
```
<details>
<summary><strong>Screenshot - React application created</strong></summary>

<img width="936" height="464" alt="Screenshot 2026-09-04 at 6 31 18 PM" src="https://github.com/user-attachments/assets/babd5127-95c5-4cf9-b64c-d0c0a7b248ea" />

</details>

---

## Step 5: Navigate to the Project Directory

Move into the newly created project folder.

```bash
cd my-app
```

<details>
<summary><strong>Screenshot - Inside the project directory</strong></summary>

<img width="1376" height="62" alt="Screenshot 2026-09-04 at 7 43 36 PM" src="https://github.com/user-attachments/assets/f3a98ae5-16fc-4979-b1c3-5a5fc9ec3ee8" />

</details>

---

## Step 6: Start the Development Server

Run the React application locally.

```bash
npm start
```

<details>
<summary><strong>Screenshot - Development server started</strong></summary>

<img width="466" height="209" alt="Screenshot 2026-09-04 at 6 32 03 PM" src="https://github.com/user-attachments/assets/a92f876a-6181-42ec-8ca2-472bb128d377" />

</details>

---

# 9. Verification

## Step 1: Check Node.js, npm, and npx Versions

Verify the Node.js, npm, and npx installations.

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

    http://<ec2-public-ip-address>:3000

<details>
<summary><strong>Screenshot - React app running in browser</strong></summary>

<img width="1376" height="777" alt="Screenshot 2026-09-04 at 7 35 56 PM" src="https://github.com/user-attachments/assets/1c6a20df-49bc-4cbf-857d-ea9e65f11e73" />

</details>

---

# 12. Troubleshooting

| Issue | Possible Cause | Solution |
|-------|-----------------|----------|
| Port 3000 already in use | Another process is using the port | Stop the conflicting process or run on a different port |
| Package installation fails | Network or registry issue | Check connectivity and npm registry settings |

---

# 13. Quick Commands

| Task | Command |
|------|---------|
| Update repositories | `sudo apt update` |
| Install Node.js | `sudo apt install nodejs -y` |
| Install npm | `sudo apt install npm -y` |
| Check Node.js version | `node -v` |
| Check npm version | `npm -v` |
| Check npx version | `npx -v` |
| Create React app | `npx create-react-app my-app` |
| Start development server | `npm start` |

---

# 14. Conclusion

React JS can be installed easily on Linux by first setting up Node.js and npm.

After installation, the setup should be verified by checking Node.js and npm versions and confirming that the development server runs successfully, ensuring the system is ready for React application development.

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
| [Node.js Official Website](https://nodejs.org/) | Official Node.js website |
| [npm Documentation](https://docs.npmjs.com/) | Official npm documentation |
| [Create React App Documentation](https://create-react-app.dev/) | Official Create React App documentation |
