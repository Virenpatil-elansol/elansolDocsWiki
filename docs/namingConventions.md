# 📚 **Elansol Naming Conventions** 

This guide provides a comprehensive set of naming conventions for databases, entities, interfaces, types, enums, files, and folders. Consistent naming ensures code clarity, maintainability, and improved collaboration. ✨

---

## 📖 General Guidelines
- Use `snake_case` for database objects (tables, columns, indexes, etc.).
- Avoid **reserved keywords** (e.g., `user`, `table`, `index`).
- Be **descriptive but concise** in naming.
- Prefer **singular nouns** for table names (e.g., `user`, not `users`).

---

## 🗃️ Database Naming Conventions

### **Tables**
- **Format**: `snake_case`
- **Example**: `user_profile`, `order_history`

### **Columns**
- **Format**: `snake_case`
- Prefix columns with the **table name** for clarity in joins and queries.
- **Example**: `user_id`, `created_at`, `first_name`, `email_address`

### **Primary Keys**
- **Format**: `id` or `[table]_id`
- **Example**: `id` (for single table), `user_id` (for shared context)

### **Foreign Keys**
- **Format**: `[referenced_table]_id`
- **Example**: `user_id`, `product_id`

### **Indexes**
- **Format**: `idx_[table]_[column]`
- **Example**: `idx_user_email`

### **Timestamps**
Standard column names for timestamps:
- `created_at`: Creation timestamp
- `updated_at`: Last modification timestamp
- `deleted_at`: Soft delete timestamp (optional)

---

## 🛠️ Entity Naming Conventions

### **Entity Names (Classes/Models)**
- **Format**: `PascalCase`
- Represent domain objects or database tables.
- Use **singular nouns** for each entity.
- **Example**: `UserProfile`, `OrderHistory`

### **Entity Properties**
- **Format**: `camelCase`
- Match the corresponding database column name in code.
- **Example**: `userId`, `createdAt`, `firstName`

---

## 🖍️ Interface Naming Conventions

### **Interfaces**
- **Format**: `PascalCase`, optionally prefixed with `I` (common in TypeScript).
- Define the shape of objects or types.
- **Example**: `IUserProfile`, `IOrderHistory`

### **Interface Properties**
- **Format**: `camelCase`
- **Example**:  
```typescript
interface IUserProfile {
  userId: string;
  firstName: string;
  emailAddress: string;
}
```

---

## 🧩 Types and Enums Naming Conventions

### **Types**
- **Format**: `PascalCase`
- Use descriptive names; avoid abbreviations.
- **Example**:  
```typescript
type UserStatus = "active" | "inactive" | "banned";
```

### **Enums**
- **Format**: `PascalCase` for the enum name, `UPPER_SNAKE_CASE` for its values.
- **Example**:  
```typescript
enum OrderStatus {
  PENDING = "PENDING",
  SHIPPED = "SHIPPED",
  DELIVERED = "DELIVERED",
}
```

---

## 🗂️ File Naming Conventions

### **Files**
- **Format**: `kebab-case`
- Separate words with hyphens. Avoid spaces or underscores.
- **Example**: `user-profile.ts`, `order-history.model.ts`

### **Folders**
- **Format**: `kebab-case`
- Group related files logically.
- **Example**:  
```
/models
  - user-profile.ts
  - order-history.ts
/controllers
  - user-controller.ts
```

### **Component Files (React)**
- **Format**:  
  - Main component file: `PascalCase`  
  - Auxiliary files: `kebab-case`  
- **Example**:  
  - `UserProfile.tsx`  
  - `user-profile.styles.ts`  
  - `user-profile.test.ts`  

---

## 🚀 Example Workflow

### **Database Schema**  
```sql
CREATE TABLE user_profile (
  id SERIAL PRIMARY KEY,
  user_id INT NOT NULL,
  first_name VARCHAR(50) NOT NULL,
  last_name VARCHAR(50),
  email_address VARCHAR(100) UNIQUE NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

### **Entity (TypeScript Model)**  
```typescript
class UserProfile {
  id: number;
  userId: number;
  firstName: string;
  lastName?: string;
  emailAddress: string;
  createdAt: Date;
  updatedAt: Date;
}
```

### **Interface**  
```typescript
interface IUserProfile {
  id: number;
  userId: number;
  firstName: string;
  lastName?: string;
  emailAddress: string;
  createdAt: Date;
  updatedAt: Date;
}
```

### **Type Definition**  
```typescript
type UserRole = "admin" | "editor" | "viewer";
```

### **File Organization**  
```
/src
  /models
    - user-profile.ts
  /interfaces
    - user-profile.interface.ts
  /types
    - user-role.type.ts
  /services
    - user-profile.service.ts
  /controllers
    - user-controller.ts
```

---

By following these conventions, you’ll create a clean, maintainable, and scalable codebase. 🛡️