# React JS Installation Guide

<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/a/a7/React-icon.svg" width="150"/>
</p>

---

# Author Table

| Author | Created On | Version | Last Updated | L0 Reviewer | L1 Reviewer | L2 Reviewer |
|--------|------------|---------|---------------|-------------|-------------|-------------|
| Sahil | 29-08-26 | v1.0 | | | | |

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

    cd my-app

---

## Step 6: Start the Development Server

Run the React application locally.

    npm start

---

# 6. Verification

## Step 1: Check Node.js Version

Verify that Node.js has been installed successfully.

    node -v

Expected output:

    v18.x.x

---

## Step 2: Check npm Version

Verify the npm installation.

    npm -v

Expected output:

    9.x.x

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

## Step 3: Test Node.js

Run a simple Node.js command to confirm that the runtime is working.

    node -e "console.log('Node.js is working')"

Expected output:

    Node.js is working

---

# 8. Maintenance

## 8.1 Upgrade npm

npm can be upgraded when required.

    sudo npm install -g npm@latest

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

# 12. FAQs

### Q1. Is React JS free to use?

Yes, React JS is free and open-source.

### Q2. What is npm?

npm (Node Package Manager) is used to install and manage JavaScript packages, including React and its dependencies.

### Q3. How can I check whether Node.js is installed?

Use:

    node -v

### Q4. How can I create a new React application?

Use:

    npx create-react-app my-app

### Q5. How can I run the React application?

Use:

    npm start

---

# 13. Contact Information

| Name | Email ID |
|------|----------|
| Sahil | |

---

# 14. References

| Resource | Description |
|----------|--------------|
| [React Official Website](https://react.dev/) | Official React JS website |
| [Node.js Official Website](https://nodejs.org/) | Official Node.js website |
| [npm Documentation](https://docs.npmjs.com/) | Official npm documentation |
| [Create React App Documentation](https://create-react-app.dev/) | Official Create React App documentation |

---
