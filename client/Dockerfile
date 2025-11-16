# ========== STAGE 1: BUILD ==========
FROM node:20 AS build

WORKDIR /app

# Chỉ copy file khai báo dep trước để tối ưu cache
COPY package*.json ./

# Cài dep theo lockfile
RUN npm ci

# Copy source code
COPY . .

# Build Vite
RUN npm run build

# ========== STAGE 2: RUNTIME ==========
FROM nginx:alpine


COPY nginx.conf /etc/nginx/conf.d/nginx.conf

# Copy build output từ stage build sang
COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
