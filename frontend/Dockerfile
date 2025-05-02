# Build stage
FROM node:18 AS build

WORKDIR /app

# Install dependencies first for caching
COPY package*.json ./
RUN npm install

# Copy the rest of the app and build
COPY . .
RUN npm run build

# Serve stage
FROM nginx:1.25.1

# Copy custom nginx config
COPY .docker/nginx.conf /etc/nginx/nginx.conf

# Copy the built React app
COPY --from=build /app/build /usr/share/nginx/html

# Include env.js at runtime
COPY env.js /usr/share/nginx/html/env.js

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
