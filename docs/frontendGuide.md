# 🚀 **Frontend Guide for Elansol Projects**

This guide provides a step-by-step introduction to setting up and working with the **KPI Frontend** project. Follow along to get started efficiently and maintain best practices throughout your development journey.

---

## 🎯 Getting Started with the KPI Frontend

### **1. Cloning the GitHub Repository**

Clone the project repository using SSH:

```bash
git clone git@github.com:elansol/KpiPlus-frontend.git
```

Ensure your SSH key is set up for GitHub. For detailed instructions on creating an SSH key, refer to the following guide:  
[Learn about SSH keys](https://www.google.com)

---

### **2. Setting Up the Project**

Run the following commands in your terminal:

1. Install dependencies:
   ```bash
   npm i
   ```
2. Start the development server:
   ```bash
   npm start
   ```
3. Launch Storybook for component development and documentation:
   ```bash
   npm run storybook
   ```

---

## 📁 Folder Structure of the Project

The project is organized into the following main directories:

- **`src/`**  
  Contains the core application source code, including:

  - React components
  - Redux slices for state management
  - Routes
  - Utilities and helper functions

- **`public/`**  
  Contains static assets like HTML files, images, and manifest configurations.

- **`.storybook/`**  
  Configuration files for **Storybook**, which provides a sandbox environment for UI component development.

---

## 🛠️ Libraries Used in the Project

Here’s a breakdown of the key tools and libraries integrated into the KPI Frontend:

1. **[Storybook](https://storybook.js.org/)**

   - A development environment for building, testing, and documenting UI components in isolation.

2. **[Material-UI](https://mui.com/)**

   - A modern React UI library for creating accessible, responsive, and customizable design systems.

3. **[React Redux Toolkit](https://redux-toolkit.js.org/)**

   - The official and recommended way to manage state in React apps.

4. **[React Router](https://reactrouter.com/)**

   - A powerful library for implementing dynamic routing and navigation.

5. **[Axios](https://axios-http.com/)**

   - A promise-based HTTP client for handling API requests with ease.

6. **[direnv](https://direnv.net/)**
   - A shell extension for automatically loading environment variables based on the current directory.

---

By following this guide, you'll be well-equipped to set up, develop, and contribute to the KPI Frontend project seamlessly. Happy coding! 🎉
