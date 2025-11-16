# Ví dụ
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./

# Debug thêm thông tin
RUN node -v && npm -v && ls -la && \
    npm ci || (echo "===== NPM CI FAILED, DUMP LOG =====" && cat /root/.npm/_logs/*-debug-0.log && exit 1)

COPY . .

RUN npm run build
