# Use a lightweight Node.js image
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

RUN npm install

# Copy the rest of the app code
COPY . .

# Expose backend port
EXPOSE 3000

# Start the backend
CMD ["npm", "start"]

# docker run -p 3000:3000 --env-file .env quickshow-be
