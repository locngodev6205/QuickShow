# ===== Build stage =====
FROM node:18-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build


# ===== Run stage (Nginx) =====
FROM nginx:alpine

# Copy file build từ Vite sang thư mục Nginx
COPY --from=build /app/dist /usr/share/nginx/html

# Copy cấu hình nginx cho SPA
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

# docker run -p 8080:80 quickshow-fe
