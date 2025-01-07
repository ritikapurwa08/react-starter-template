Here’s a **continuous README** for your repository, explaining how to set up the project and add the `users` table with Convex, including the provided code snippet for user creation with email, password, and additional fields:

---

# 🚀 Modern Full-Stack React App with shadcn UI, Convex, and Bun

This repository is a starter template for building modern, full-stack React applications with **shadcn UI**, **Convex** for backend, and **Bun** for blazing-fast development. It includes authentication, user management, and more.

---

## 🛠️ **Setup Instructions**

### 1. **Clone the Repository**

```bash
git clone <repo-url>
cd <repo-folder>
```

### 2. **Install Dependencies**

```bash
bun install
```

### 3. **Set Up Convex**

1. Sign up at [Convex](https://convex.dev) and create a new project.
2. Install the Convex CLI:
   ```bash
   bun add convex
   ```
3. Log in to Convex:
   ```bash
   npx convex auth login
   ```
4. Push your schema to Convex:
   ```bash
   npx convex push
   ```

---

## 🗂️ **Add Users Table with Convex**

### 1. **Define the `users` Table Schema**

In your Convex schema file (`convex/schema.ts`), define the `users` table with the required fields:

```typescript
import { defineSchema, defineTable } from "convex/server";
import { v } from "convex/values";

export default defineSchema({
  users: defineTable({
    email: v.string(),
    name: v.string(),
    userName: v.string(),
    contactEmail: v.optional(v.string()),
    role: v.union(v.literal("admin"), v.literal("user"), v.literal("proUser")),
    age: v.optional(v.number()),
    mobileNumber: v.optional(v.number()),
    address: v.optional(v.string()),
    customProfilePicture: v.string(),
    profileImageStorageId: v.optional(v.id("_storage")),
    following: v.array(v.string()),
    followers: v.array(v.string()),
    securityQuestions: v.array(v.string()),
    updatedAt: v.number(),
    lastPasswordUpdate: v.optional(v.number()),
    likedBlogs: v.array(v.string()),
    savedBlogs: v.array(v.string()),
  }).index("by_email", ["email"]),
});
```

### 2. **Set Up Authentication**

Create a file `convex/auth.ts` and add the following code to handle user authentication and profile creation:

```typescript
import { convexAuth } from "@convex-dev/auth/server";
import { Password } from "@convex-dev/auth/providers/Password";
import { DataModel, Id } from "./_generated/dataModel";

const CustomPassword = Password<DataModel>({
  profile(params) {
    return {
      email: params.email as string,
      name: params.name as string,
      userName: params.userName as string,
      contactEmail: params.contactEmail as string | undefined,
      role: params.role as "admin" | "user" | "proUser",
      age: params.age as number | undefined,
      mobileNumber: params.mobileNumber as number | undefined,
      address: params.address as string | undefined,
      customProfilePicture: params.customProfilePicture as string,
      profileImageStorageId: params.profileImageStorageId as
        | Id<"_storage">
        | undefined,
      following: [],
      followers: [],
      securityQuestions: [],
      updatedAt: params.updatedAt as number,
      lastPasswordUpdate: undefined,
      likedBlogs: [],
      savedBlogs: [],
    };
  },
});

export const { auth, signIn, signOut, store, isAuthenticated } = convexAuth({
  providers: [CustomPassword],
});
```

### 3. **Push Changes to Convex**

Push your updated schema and authentication logic to Convex:

```bash
npx convex push
```

---

## 🚀 **Run the Project**

1. Start the development server:
   ```bash
   bun run dev
   ```
2. Open your browser and navigate to `http://localhost:5173`.

---

## 🧩 **Import Aliases**

To keep your code clean, configure import aliases in `tsconfig.json`:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

Now you can use aliases like `@/components/Button` instead of relative paths.

---

## 🌟 **Features**

- 🎨 **shadcn UI**: Beautiful, customizable components.
- ⚡ **Convex**: Real-time database and serverless functions.
- 🔐 **Authentication**: Secure user authentication.
- 🚀 **Bun**: Blazing-fast runtime and package manager.

---

Let’s build something amazing! 🚀✨

---

Feel free to customize this further to match your repository's specifics! Let me know if you need additional tweaks. 😊
