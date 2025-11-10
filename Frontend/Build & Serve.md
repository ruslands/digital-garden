всегда есть два варианта первый это поднять сервер, который будет раздавать статику, например, Node.js и второй вариант это поднять Nginx, который будет также просто раздавать статику

Here's a **Dockerfile** for a Vue.js application using **Node.js 22**, building the app and serving the `dist` folder on **port 3000**:

```Dockerfile
# Use Node.js 22 as the base image
FROM node:22-alpine AS builder

# Set working directory
WORKDIR /app

# Copy package.json and package-lock.json (if available)
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the source code
COPY . .

# Build the Vue app
RUN npm run build

# Production stage: use a lightweight server to serve the static files
FROM node:22-alpine

# Install a simple HTTP server (e.g., serve)
RUN npm install -g serve

# Set working directory
WORKDIR /app

# Copy the built dist folder from the builder stage
COPY --from=builder /app/dist ./dist

# Expose port 3000
EXPOSE 3000

# Serve the static files on port 3000
CMD ["serve", "-s", "dist", "-p", "3000"]
```

---

### ✅ How to Use

1. Save this as `Dockerfile` in your project root (same directory as `package.json`).
2. Build the image:
   ```bash
   docker build -t vue-app .
   ```
3. Run the container:
   ```bash
   docker run -p 3000:3000 vue-app
   ```
4. Open your browser at `http://localhost:3000`

---

### 🔍 Notes

- Uses **multi-stage build** to keep the final image small.
- `node:22-alpine` is lightweight; you can replace with `node:22` if you need a full OS.
- Assumes your Vue app builds to the `dist` folder (default for Vue CLI and Vite).
- Uses the `serve` package to serve static files — lightweight and production-ready for simple apps.
