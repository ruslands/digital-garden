## **CLI**
| Command          | Description                                          | Common Options                                        |
| ---------------- | ---------------------------------------------------- | ----------------------------------------------------- |
| `terraform init` | Initializes a working directory, downloads providers | `-upgrade`, `-backend-config=KEY=VALUE`               |
| `terraform plan` | Creates an execution plan for changes                | `-out=FILE`, `-var 'key=value'`, `-detailed-exitcode` |

## **Multi-Stage Build**

A **multi-stage build** is a Docker feature that allows you to use multiple `FROM` statements in a single `Dockerfile`. Each `FROM` instruction begins a new stage of the build, and you can selectively copy artifacts from one stage to another, **discarding intermediate layers**.

This lets you **separate your build environment from your runtime environment**, resulting in **smaller, more secure, and production-ready images**.

---

### ✅ Why Use Multi-Stage Builds?

#### Problem:
When building applications like Vue.js apps, you need:
- Node.js and tools (`npm`, `vite`, etc.) to build the app.
- But once built, you only need to serve static files (`dist/` folder).

If you bundle all build tools into the final image → **larger image size**, **security risks**, and **unnecessary bloat**.

#### Solution:
Use **multi-stage builds**:
1. **Builder stage**: Use a full Node.js image to install deps and build the app.
2. **Runtime stage**: Use a minimal image (e.g., `alpine`, or even `nginx`) to serve the built files.

Only the **final stage** is shipped in production.

---

### 🧱 Example: Vue App with Multi-Stage Build

```Dockerfile
# ===== Stage 1: Build =====
FROM node:22-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# ===== Stage 2: Serve =====
FROM nginx:alpine

COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 3000
CMD ["nginx", "-g", "daemon off;"]
```

---

### 🔗 Key Part: `COPY --from=builder`

```Dockerfile
COPY --from=builder /app/dist /usr/share/nginx/html
```

This copies only the **built output** (`dist/`) from the first stage into the lightweight `nginx` container. All Node.js tools, source code, and `node_modules` are **left behind**.

---

### ✅ Benefits

| Benefit | Explanation |
|-------|-------------|
| 🔽 **Smaller Image Size** | Final image doesn’t include dev dependencies or Node.js itself (if not needed). |
| 🔒 **Improved Security** | Less software in the container = fewer attack vectors. |
| 🚀 **Faster Deployments** | Smaller images pull and start faster. |
| 🧼 **Clean Separation** | Build-time vs runtime concerns are clearly separated. |

---

### 🆚 Without Multi-Stage

You’d end up with a single image that has:
- Node.js
- `node_modules` (hundreds of MB)
- Source code
- Build tools

Even though you only need to **serve static files**.

---

### 💡 Tip: Name Your Stages

Use `AS <name>` to name stages (like `AS builder`), so you can reference them clearly:
```Dockerfile
COPY --from=builder /app/dist ./dist
```

---

### Summary

> **Multi-stage builds = Build your app in one container, run it in another — smaller, cleaner, better.**

It’s considered a **best practice** for modern Dockerized applications, especially frontend apps like Vue, React, etc.