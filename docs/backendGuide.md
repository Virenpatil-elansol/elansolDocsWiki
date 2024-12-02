# 🚀 **Backend Guide for Elansol Projects**

This guide provides a comprehensive overview of how to set up and work with the **Elansol Backend** project. Follow these steps to ensure efficient development and adherence to best practices.

---

## 🎯 Getting Started with the Elansol Backend

### **1. Cloning the GitHub Repository**

Clone the project repository using SSH:

```bash
git clone git@github.com:elansol/elansol-backend.git
```

Ensure your SSH key is set up for GitHub. For detailed instructions on creating an SSH key, refer to this [guide](https://www.google.com).

---

### **2. Setting Up the Project**

Run the following commands in your terminal:

1. Navigate to the project directory:
   ```bash
   cd elansol-backend
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the development server with **Nodemon** for automatic restarts:
   ```bash
   npm run dev
   ```

---

## 📁 Folder Structure of the Project

The project is organized as follows:

```plaintext
/elansol-backend
├── /src
│   ├── /config          # Configuration files
│   │   └── database.config.ts
│   ├── /controllers     # Express route controllers
│   │   └── user.controller.ts
│   ├── /entities        # TypeORM entities
│   │   └── user.entity.ts
│   ├── /middlewares     # Express middlewares
│   │   └── auth.middleware.ts
│   ├── /repositories    # Data access layer (DAL)
│   │   └── user.repository.ts
│   ├── /services        # Business logic layer
│   │   └── user.service.ts
│   ├── /routes          # Route definitions
│   │   └── user.routes.ts
│   ├── /utils           # Utility functions
│   │   └── logger.util.ts
│   └── server.ts        # Entry point of the application
├── /migrations          # Database migration scripts
├── /tests               # Unit and integration tests
├── .env                 # Environment variables
├── package.json         # Project metadata and dependencies
└── tsconfig.json        # TypeScript configuration
```

---

## 🛠️ Libraries Used in the Project

Here’s an overview of the main tools and libraries integrated into the Elansol Backend:

1. **[Express.js](https://expressjs.com/)**  
   A minimalist web framework for building APIs and web applications.

2. **[TypeORM](https://typeorm.io/)**  
   An ORM for TypeScript/JavaScript, supporting various SQL databases.

3. **[Nodemon](https://nodemon.io/)**  
   Automatically restarts the server when file changes are detected.

4. **[dotenv](https://github.com/motdotla/dotenv)**  
   Loads environment variables from a `.env` file into `process.env`.

5. **[Jest](https://jestjs.io/)**  
   A testing framework for unit and integration tests.

6. **[class-validator](https://github.com/typestack/class-validator)**  
   Validates class properties using decorators.

7. **[class-transformer](https://github.com/typestack/class-transformer)**  
   Transforms plain objects into class instances and vice versa.

---

## 📡 Connecting to SQL Server

### **1. Install Required Packages**

Install the necessary packages for TypeORM and SQL Server:

```bash
npm install typeorm reflect-metadata mssql
```

### **2. Create a Database Configuration File**

In `src/config/database.config.ts`:

```typescript
import { DataSource } from "typeorm";
import * as dotenv from "dotenv";

dotenv.config();

const AppDataSource = new DataSource({
  type: "mssql",
  host: process.env.DB_HOST,
  port: Number(process.env.DB_PORT),
  username: process.env.DB_USER,
  password: process.env.DB_PASS,
  database: process.env.DB_NAME,
  synchronize: true, // Set to false in production
  logging: false,
  entities: [__dirname + "/../entities/*.ts"],
  migrations: [__dirname + "/../migrations/*.ts"],
  subscribers: [],
});

export default AppDataSource;
```

### **3. Update the `.env` File**

Add database connection details:

```plaintext
DB_HOST=your_sql_server_host
DB_PORT=1433
DB_USER=your_username
DB_PASS=your_password
DB_NAME=your_database_name
```

### **4. Initialize the Database Connection**

In `src/server.ts`:

```typescript
import "reflect-metadata";
import AppDataSource from "./config/database.config";
import express from "express";

const app = express();
const PORT = process.env.PORT || 3000;

AppDataSource.initialize()
  .then(() => {
    console.log("Database connected successfully");
    app.listen(PORT, () => {
      console.log(`Server is running on http://localhost:${PORT}`);
    });
  })
  .catch((error) => console.log("Database connection error:", error));
```

---

## 📚 Using the Libraries

### **1. Express.js Example**

In `src/routes/user.routes.ts`, define a simple route:

```typescript
import { Router } from "express";
import UserController from "../controllers/user.controller";

const router = Router();

router.get("/users", UserController.getAllUsers);
router.post("/users", UserController.createUser);

export default router;
```

### **2. TypeORM Example**

In `src/controllers/user.controller.ts`, interact with the database:

```typescript
import { Request, Response } from "express";
import { User } from "../entities/user.entity";
import AppDataSource from "../config/database.config";

const userRepository = AppDataSource.getRepository(User);

class UserController {
  static async getAllUsers(req: Request, res: Response) {
    const users = await userRepository.find();
    res.json(users);
  }

  static async createUser(req: Request, res: Response) {
    const newUser = userRepository.create(req.body);
    await userRepository.save(newUser);
    res.status(201).json(newUser);
  }
}

export default UserController;
```

### **3. Using `class-validator`**

In `src/entities/user.entity.ts`:

```typescript
import { Entity, PrimaryGeneratedColumn, Column } from "typeorm";
import { IsEmail, IsNotEmpty } from "class-validator";

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  @IsNotEmpty()
  name: string;

  @Column()
  @IsEmail()
  email: string;
}
```

### **4. Data Access Layer Example**

In `src/repositories/user.repository.ts`:

```typescript
import { User } from "../entities/user.entity";
import AppDataSource from "../config/database.config";

class UserRepository {
  private userRepository = AppDataSource.getRepository(User);

  async findAll() {
    return await this.userRepository.find();
  }

  async create(userData: Partial<User>) {
    const user = this.userRepository.create(userData);
    return await this.userRepository.save(user);
  }
}

export default new UserRepository();
```

### **5. Service Layer Example**

In `src/services/user.service.ts`:

```typescript
import UserRepository from "../repositories/user.repository";
import { User } from "../entities/user.entity";

class UserService {
  async getAllUsers(): Promise<User[]> {
    return await UserRepository.findAll();
  }

  async createUser(userData: Partial<User>): Promise<User> {
    return await UserRepository.create(userData);
  }
}

export default new UserService();
```

---

By following this guide, you'll be ready to set up, develop, and maintain **Elansol Backend** projects with confidence. Happy coding! 🎉
