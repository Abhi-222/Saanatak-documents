# Common Stack | React JS | Installation Guide
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/a/a7/React-icon.svg" width="150"/>
</p>

---

# Author Table

| Author | Created On | Version | Last Updated | L0 Reviewer | L1 Reviewer | L2 Reviewer |
|--------|------------|---------|---------------|-------------|-------------|-------------|
| Sahil | 28-08-26 | v1.0 |28-08-26| `Diviya M`| `Aayush Verma` | `Mahesh Kumar / Varun` |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Purpose](#2-purpose)
3. [Prerequisites](#3-prerequisites)
4. [System Requirements](#4-system-requirements)
5. [React JS Installation](#5-react-js-installation)
6. [Verification](#6-verification)
7. [Environment Configuration](#7-environment-configuration)
8. [Maintenance](#8-maintenance)
9. [Troubleshooting](#9-troubleshooting)
10. [Quick Commands](#10-quick-commands)
11. [Conclusion](#11-conclusion)
12. [FAQs](#12-faqs)
13. [Contact Information](#13-contact-information)
14. [References](#14-references)

---

# 1. Introduction

React JS is an open-source JavaScript library maintained by Meta, used for building fast and interactive user interfaces, primarily for single-page applications (SPAs).
This document provides a basic procedure to install and configure a React JS application on a Linux system.

---

# 2. Purpose

The purpose of this document is to provide a simple procedure to:

- Install Node.js and npm on Linux
- Create a new React application
- Install project dependencies
- Verify the React installation
- Run the React development server
- Perform basic maintenance and troubleshooting

---

# 2. What is React JS

React JS is a component-based JavaScript library used to build user interfaces. Instead of manipulating the browser's DOM directly, React uses a **Virtual DOM** to track changes and update only the parts of the actual DOM that have changed, improving performance.

Applications built with React are structured as a tree of components — small, independent, and reusable pieces of UI (buttons, forms, cards, etc.) that manage their own logic and rendering, and can be composed together to build complex interfaces.

---

# 3. Why React JS is Required

React JS is required in modern application development because it addresses several common front-end challenges:

- **Faster UI updates** — the Virtual DOM minimizes expensive direct DOM operations.
- **Reusability** — components can be reused across different parts of an application, reducing duplication.
- **Maintainability** — breaking UI into components makes large applications easier to manage and debug.
- **Strong ecosystem** — a large community, extensive libraries, and tooling support (React Router, Redux, Next.js, etc.).
- **Industry adoption** — widely used, making it easier to hire talent and find support/resources.

---

# 4. Key Features of React JS

| **Feature** | **Description** |
| ----------- | ---------------- |
| Virtual DOM | Maintains a lightweight copy of the real DOM in memory and updates only changed elements, improving rendering performance. |
| Component-Based Architecture | UI is broken into independent, reusable components, making development modular and easier to maintain. |
| JSX (JavaScript XML) | Allows writing HTML-like syntax within JavaScript, making UI code more readable and intuitive. |
| One-Way Data Binding | Data flows in a single direction (parent to child), making the application more predictable and easier to debug. |
| Hooks | Functions like `useState` and `useEffect` allow functional components to manage state and lifecycle behavior without needing class components. |
| Rich Ecosystem | Supported by a large number of libraries and tools such as React Router (routing), Redux/Context API (state management), and Next.js (server-side rendering). |
| Declarative UI | Developers describe what the UI should look like for a given state, and React handles the DOM updates automatically. |
| Cross-Platform Support | With React Native, the same concepts can be extended to build mobile applications. |

---

# 3. Prerequisites

Before starting the installation, ensure the following are available:

- Linux system such as Ubuntu or Debian
- Terminal access
- Internet connectivity
- Sudo privileges
- Basic Linux command-line knowledge

---

# 4. System Requirements

| Requirement | Minimum Recommendation |
|-------------|-------------------------|
| RAM | 4 GB or higher |
| Disk Space | 5 GB or higher |
| Operating System | Ubuntu / Debian / Linux |
| Internet | Required |
| Privileges | Sudo access |

### Important Ports

| Port | Description |
|------|-------------|
| 22 | SSH access to Linux server |
| 3000 | Default React development server port |
| 443 | HTTPS communication |

---

# 5. React JS Installation

## Step 1: Update Package Repository

Update the package information before installing Node.js.

    sudo apt update

---

## Step 2: Install Node.js

Install Node.js, which includes npm (Node Package Manager).

    sudo apt install nodejs -y

---

## Step 3: Install npm (if not already installed)

Install npm separately if it was not installed along with Node.js.

    sudo apt install npm -y

---

## Step 4: Create a New React Application

Use `npx` to create a new React project using Create React App.

    npx create-react-app my-app

---

## Step 5: Navigate to the Project Directory

Move into the newly created project folder.

---

## Step 6: Start the Development Server

Run the React application locally.

    
    cd my-app
    npm start

---

# 6. Verification

## Step 1: Check Node.js Version

Verify that Node.js has been installed successfully.

    node -v

---

## Step 2: Check npm Version

Verify the npm installation.

    npm -v

---

## Step 3: Verify React Application is Running

Open a browser and navigate to:

    http://localhost:3000

Expected output: The default React welcome page is displayed.

---

# 7. Environment Configuration

## Step 1: Check Node.js Path

Use the following command to identify the Node.js executable location.

    which node

---

## Step 2: Locate Node.js Installation

The `whereis` command displays common locations related to Node.js.

    whereis node

---

# 8. Maintenance

## 8.2 Install React Packages

Additional packages can be installed using npm.

    npm install <package-name>

Example:

    npm install axios

## 8.3 Build for Production

Create an optimized production build of the React application.

    npm run build

---

# 9. Troubleshooting

| Issue | Possible Cause | Solution |
|-------|-----------------|----------|
| `node: command not found` | Node.js is not installed | Install Node.js using apt |
| `npm: command not found` | npm is missing | Install npm separately |
| `npx: command not found` | npx is missing or npm is outdated | Upgrade npm to the latest version |
| `Permission denied` | Insufficient permissions | Use sudo privileges |
| Port 3000 already in use | Another process is using the port | Stop the conflicting process or run on a different port |
| Package installation fails | Network or registry issue | Check connectivity and npm registry settings |

---

# 10. Quick Commands

| Task | Command |
|------|---------|
| Update repositories | `sudo apt update` |
| Install Node.js | `sudo apt install nodejs -y` |
| Install npm | `sudo apt install npm -y` |
| Check Node.js version | `node -v` |
| Check npm version | `npm -v` |
| Create React app | `npx create-react-app my-app` |
| Start development server | `npm start` |
| Build for production | `npm run build` |
| Find Node.js path | `which node` |
| Locate Node.js | `whereis node` |
| Install a package | `npm install <package-name>` |

---

# 11. Conclusion

React JS can be installed easily on Linux by first setting up Node.js and npm.

After installation, the setup should be verified by checking Node.js and npm versions and confirming that the development server runs successfully, ensuring the system is ready for React application development.

---

# 13. Contact Information

| Name | Email ID |
|------|----------|
| Sahil | `sahil.butola.snaatak@mygurukulam.co`|

---

# 14. References

| Resource | Description |
|----------|--------------|
| [React Official Website](https://react.dev/) | Official React JS website |
| [Node.js Official Website](https://nodejs.org/) | Official Node.js website |
| [npm Documentation](https://docs.npmjs.com/) | Official npm documentation |
| [Create React App Documentation](https://create-react-app.dev/) | Official Create React App documentation |

---
