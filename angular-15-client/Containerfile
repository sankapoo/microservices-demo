# Stage 1: Build the Angular application
FROM registry.access.redhat.com/ubi8/nodejs-18 AS build

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package.json package-lock.json ./

# Install dependencies with elevated permissions
USER root
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the application
RUN npm run build --prod

# Verify the build output
RUN ls -la /app/dist/angular-15-crud

# Stage 2: Serve the application using Nginx
FROM registry.access.redhat.com/ubi9/nginx-120

# Copy the built Angular app from the previous stage
COPY --from=build /app/dist/angular-15-crud /usr/share/nginx/html

# Copy the Nginx configuration file
COPY nginx.conf /etc/nginx/nginx.conf

# Expose port 8081
EXPOSE 8081

# Start Nginx server
CMD ["nginx", "-g", "daemon off;"]

