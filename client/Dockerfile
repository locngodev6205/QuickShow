# # ========== STAGE 1: BUILD ==========
# FROM node:20 AS build

# WORKDIR /app

# # Chỉ copy file khai báo dep trước để tối ưu cache
# COPY package*.json ./

# # Cài dep theo lockfile
# RUN npm ci

# # Copy source code
# COPY . .

# COPY .env.production .env.production

# # Build Vite
# RUN npm run build

# # ========== STAGE 2: RUNTIME ==========
# FROM nginx:alpine


# COPY nginx.conf /etc/nginx/conf.d/default.conf

# # Copy build output từ stage build sang
# COPY --from=build /app/dist /usr/share/nginx/html

# EXPOSE 80

# CMD ["nginx", "-g", "daemon off;"]

# # docker run -p 8080:80 quickshow-fe


# ===== Stage 1: Build Vite app =====
FROM node:20 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:alpine
RUN apk add --no-cache gettext

COPY --from=builder /app/dist /usr/share/nginx/html

# Copy nginx.conf của bạn nhưng dùng như TEMPLATE
COPY nginx.conf /etc/nginx/default.conf.template

CMD ["/bin/sh", "-c", "envsubst '$APP_ALB_DNS' < /etc/nginx/default.conf.template > /etc/nginx/conf.d/default.conf && nginx -g 'daemon off;'"]

