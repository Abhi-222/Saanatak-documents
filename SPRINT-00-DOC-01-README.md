# Common Stack | Applications | React JS | Installation Guide

---

# Author Table

| Author | Created On | Version | Last Updated  | L0 Reviewer | L1 Reviewer    | L2 Reviewer |
|--------|------------|---------|---------------|-------------|----------------|-------------|
| Sahil  | 28-08-26   | v1.0    |  07-09-26     | `Divya M`   | `Aayush Verma` | `Mahesh Kumar / Varun` |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is React JS](#2-what-is-react-js)
3. [Why React JS is Required](#3-why-react-js-is-required)
4. [Key Features of React JS](#4-key-features-of-react-js)
5. [Prerequisites and System Requirements](#5-prerequisites-and-system-requirements)
6. [React JS Installation and Verification](#6-react-js-installation-and-verification)
7. [Quick Commands](#7-quick-commands)
8. [Troubleshooting](#8-troubleshooting)
9. [Use Cases](#9-use-cases)
10. [Best Practices](#10-best-practices)
11. [Conclusion](#11-conclusion)
12. [Contact Information](#12-contact-information)
13. [References](#13-references)

---

# 1. Introduction

React JS is a free, open-source JavaScript library created and maintained by Meta. It's used to build fast, interactive user interfaces — especially for single-page applications (SPAs), where the page updates smoothly without needing to reload. This document explains the basic steps to install and set up a React JS application on a Linux system.

---

# 2. What is React JS

ReactJS is a JavaScript library for building user interfaces out of reusable components. It's especially known for making single-page applications (SPAs) fast and efficient by updating only the parts of the page that change.

---

# 3. Why React JS is Required

React JS is required in modern application development because it addresses several common front-end challenges:

- **Scalability** – React supports scalable frontend development through reusable, modular components that make large codebases easier to manage.
- **Strong Ecosystem** – It's backed by a mature ecosystem of libraries, such as Redux for state management and React Router for navigation.
- **Complex UI Handling** – React excels at managing complex, interactive user interfaces with ease, thanks to its efficient rendering (Virtual DOM).
- **End-to-End Coverage** – Its ecosystem covers rendering, routing, testing, and debugging, making it a complete solution for frontend development.
-   
---

# 4. Key Features of React JS

- **Virtual DOM** – React keeps a lightweight copy of the actual webpage in memory. When something changes, it only updates that specific part instead of reloading the whole page — making apps faster.
- **Component-Based Architecture** – The UI is built from small, independent pieces called components. Each one can be reused and maintained separately, like building with blocks.
- **JSX (JavaScript XML)** – Lets you write HTML-like code directly inside JavaScript, so building and reading UI code feels more natural.
- **One-Way Data Binding** – Data flows in a single direction — from parent components to child components. This makes it easier to track where data comes from and fix bugs.
- **Hooks** – Special functions like useState and useEffect let simple (functional) components handle data and behavior over time, without needing more complex class-based components.
  
---

# 5. Prerequisites and System Requirements

| Requirement | Minimum |
|-------------|-------------------------|
| Operating System | Any Linux distribution (Ubuntu, Debian, Fedora, and similar) |
| Sudo Privileges | Required for system-level installation |
| RAM | 1 GB (2 GB+ recommended for smoother builds) |
| Disk Space | ~1 GB free (more as `node_modules` and build output grow) |


### Important Ports

| Port | Description |
|------|-------------|
| 22 | SSH access to Linux server |
| 3000 | Default React development server port |

---

# 6. React JS Installation and Verification

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

Use `npx` to create a new React project using Create React App.

```bash
npx create-react-app my-app
```
<details>
<summary><strong>Screenshot - React application created</strong></summary>

<img width="936" height="464" alt="Screenshot 2026-09-04 at 6 31 18 PM" src="https://github.com/user-attachments/assets/babd5127-95c5-4cf9-b64c-d0c0a7b248ea" />

</details>

---

### Step 5: Navigate to the Project Directory

Move into the newly created project folder.

```bash
cd my-app
```

<details>
<summary><strong>Screenshot - Inside the project directory</strong></summary>

<img width="1376" height="62" alt="Screenshot 2026-09-04 at 7 43 36 PM" src="https://github.com/user-attachments/assets/f3a98ae5-16fc-4979-b1c3-5a5fc9ec3ee8" />

</details>

---

### Step 6: Start the Development Server

Run the React application locally.

```bash
npm start
```

<details>
<summary><strong>Screenshot - Development server started</strong></summary>

<img width="466" height="209" alt="Screenshot 2026-09-04 at 6 32 03 PM" src="https://github.com/user-attachments/assets/a92f876a-6181-42ec-8ca2-472bb128d377" />

</details>

---

### Step 7: Check Node.js, npm, and npx Versions

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

### Step 8: Verify React Application is Running

Open a browser and navigate to:

    http://<ec2-public-ip-address>:3000

<details>
<summary><strong>Screenshot - React app running in browser</strong></summary>

<img width="1376" height="777" alt="Screenshot 2026-09-04 at 7 35 56 PM" src="https://github.com/user-attachments/assets/1c6a20df-49bc-4cbf-857d-ea9e65f11e73" />

</details>

---

# 7. Quick Commands

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

--

# 8. Troubleshooting

| Issue | Possible Cause | Solution |
|-------|-----------------|----------|
| Port 3000 already in use| Another process is using the port | Stop the conflicting process or run on a different port |
| Package installation fails | Network or registry issue | Check connectivity and npm registry settings |

---
# 9. Use Cases

- **Single-Page Applications (SPAs)** – Apps like Gmail or Trello where the page updates instantly without full reloads (e.g., dashboards, admin panels).
- **E-Commerce Platforms** – Product listings, carts, and filters that need to update in real time as users browse and interact.
- **Social Media Feeds** – Dynamic, constantly updating content like Instagram or Facebook feeds, where new posts and likes appear without refreshing.
- **Progressive Web Apps (PWAs)** – Web apps that behave like native mobile apps, offering offline support and fast load times.
- **Cross-Platform Mobile Apps** – Using React Native (React's mobile counterpart) to build iOS and Android apps from a shared codebase.

---

# 10. Best Practices

- **Keep Components Small and Focused** – Each component should do one thing well, making it easier to test, reuse, and debug.
- **Use Meaningful Naming Conventions** – Name components and variables clearly (e.g., UserProfile instead of Comp1) so the code is self-explanatory.
- **Avoid Unnecessary Re-renders** – Use tools like React.memo or useMemo to prevent components from re-rendering when their data hasn't changed.
- **Organize Files by Feature, Not Type** – Group related components, styles, and logic together (e.g., a UserProfile folder) instead of scattering them across generic components/, styles/ folders.
- **Write Reusable and Testable Code** – Break logic into custom hooks and small functions so they can be tested independently and reused across the app.

---

# 11. Conclusion

React JS can be easily installed on Linux by first setting up Node.js and npm. Once installed, you can confirm everything is working by checking the Node.js and npm versions and making sure the development server starts up correctly. This means your system is fully ready to start building React applications.

---

# 12. Contact Information

| Name | Email ID |
|------|----------|
| Sahil | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co)|

---

# 13. References

| Resource | Description |
|----------|--------------|
| [React Official Website](https://react.dev/) | Official React JS website |
| [Node.js Official Website](https://nodejs.org/) | Official Node.js website |
| [npm Documentation](https://docs.npmjs.com/) | Official npm documentation |
| [Create React App Documentation](https://create-react-app.dev/) | Official Create React App documentation |
