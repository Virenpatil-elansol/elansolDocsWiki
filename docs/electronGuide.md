# 🌟 **Elansol Native App Developer Guide** 🌟

Welcome to the **Elansol Native App Developer Guide** – a **private and exclusive resource** for **Elansol Software Developers**. This guide is tailored to help you build high-performance native desktop applications using **ElectronJS**, **React**, and **TypeScript**, focusing on best practices and internal standards for Elansol projects.

This document will guide you through setting up your environment, implementing inter-process communication (IPC), and leveraging advanced techniques to build feature-rich applications that align with Elansol’s development practices.

---

## 📜 **Navigation Menu**

- **🏠 [Home](index.md):** Understand the fundamentals of building native apps with **ElectronJS**, **React**, and **TypeScript**.
- **⚙️ [Setting Up](setup.md):** Instructions to set up your development environment according to Elansol’s requirements.
- **🖥️ [Main Process Guide](mainProcess.md):** Learn how to work with Electron’s main process for system-level operations.
- **🎨 [Renderer Process Guide](rendererProcess.md):** Discover how to create engaging UIs with React and TypeScript in the renderer process.
- **🔄 [IPC Communication](ipcCommunication.md):** Master communication between the main and renderer processes, including examples of handling file access, CPU usage, and more.
- **⚡ [PreloaderJS Integration](preloaderJS.md):** Integrate PreloaderJS to efficiently manage preloading tasks and data fetching.

---

## 🔐 **Elansol Developer-Only Resource**

This guide is a confidential resource intended solely for internal use by the Elansol development team. Please refrain from sharing or distributing this material outside the company.

---

## 🛠️ **Setting Up Your Development Environment**

Follow these steps to set up your environment, ensuring compliance with Elansol’s internal development standards:

1. **Install Node.js and npm**  
   Download and install Node.js from [nodejs.org](https://nodejs.org/), which will also install npm (node package manager).

2. **Initialize Your Project**

   ```bash
   mkdir my-electron-app
   cd my-electron-app
   npm init -y
   ```

3. **Install Required Dependencies**

   ```bash
   npm install electron react react-dom typescript
   ```

4. **Initialize TypeScript**

   ```bash
   npx tsc --init
   ```

5. **Install PreloaderJS**

   ```bash
   npm install preloaderjs
   ```

6. **Follow Internal Config Standards**  
   Use the pre-defined templates and configuration files shared by the Elansol team for consistent setups.

---

## 📁 **Example Folder Structure**

Ensure your project follows this recommended structure for better organization and maintainability:

```plaintext
/my-electron-app
  /src
    /main            # Main process files (Electron)
      index.ts       # Electron entry point
      systemUtils.ts # System API interactions (file, CPU, etc.)
    /renderer        # Renderer process files (React)
      App.tsx        # Main React component
      CpuUsageComponent.tsx # CPU usage display component
      FilesList.tsx  # Directory file listing component
    /assets          # Static files like images and icons
    /styles          # Global styles (CSS/SCSS)
  /public
    index.html       # HTML template for Electron
  /node_modules
  tsconfig.json      # TypeScript configuration
  package.json       # Project dependencies and scripts
```

---

## 🖥️ **Main Process (ElectronJS)**

The **main process** manages the app lifecycle and interacts with the operating system.

### **CPU Usage Example**

```typescript
import { ipcMain } from "electron";
import os from "os";

const getCpuUsage = () => {
  const cpuInfo = os.cpus();
  return cpuInfo.map((cpu) => {
    const total = Object.values(cpu.times).reduce((a, b) => a + b, 0);
    const usage = ((total - cpu.times.idle) / total) * 100;
    return usage.toFixed(2);
  });
};

ipcMain.handle("get-cpu-usage", () => {
  return getCpuUsage();
});
```

**Note**: Modify these examples as needed to suit your specific project requirements.

---

## 🎨 **Renderer Process (React)**

The **renderer process** handles the user interface and interacts with the main process through IPC.

### **Using IPC for CPU Usage**

```tsx
import React, { useEffect, useState } from "react";

const CpuUsageComponent = () => {
  const [cpuUsage, setCpuUsage] = useState<string[]>([]);

  useEffect(() => {
    const fetchCpuUsage = async () => {
      const usage = await window.electron.invoke("get-cpu-usage");
      setCpuUsage(usage);
    };
    fetchCpuUsage();
  }, []);

  return (
    <div>
      <h2>CPU Usage</h2>
      <ul>
        {cpuUsage.map((usage, index) => (
          <li key={index}>
            Core {index + 1}: {usage}%
          </li>
        ))}
      </ul>
    </div>
  );
};

export default CpuUsageComponent;
```

---

## 🌐 **Fetching Data from Endpoints**

For external data, use `fetch` or Axios to make API calls securely.

### **Using Fetch Example**

```tsx
import React, { useEffect, useState } from "react";

const ApiDataComponent = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      const response = await fetch("https://api.example.com/data");
      setData(await response.json());
    };
    fetchData();
  }, []);

  return (
    <div>
      <h2>Fetched Data</h2>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
};

export default ApiDataComponent;
```

---

## 🔄 **IPC Communication: PreloaderJS Integration**

Use PreloaderJS for efficient preloading of tasks in the main process and share data with the renderer process via IPC.

```typescript
import { ipcMain } from "electron";
import { Preloader } from "preloaderjs";

ipcMain.handle("start-preload", async () => {
  const preloader = new Preloader();
  preloader.addTask("getCpuUsage", getCpuUsage);
  const results = await preloader.start();
  return results;
});
```

---

## 🚀 **Conclusion**

By following this guide, Elansol developers can build reliable, efficient native desktop applications. This guide is designed exclusively for **internal use** to ensure that all apps adhere to Elansol’s standards. Always keep your project updated and aligned with the latest internal templates and practices.

Happy coding!
