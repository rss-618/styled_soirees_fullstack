# Use the lightweight node image from official Docker Hub
FROM node:23-alpine AS build-stage
# set the working directory
WORKDIR /app
# Copy the package files to the working directory in the container
COPY package*.json .
# Install the project dependencies
RUN npm install
# Copy the rest of the project files to the container
COPY . .
# Build the Vue.js application to the production mode to dist folder
RUN npm run build
COPY cert.pem /app/
# Use the lightweight node image from the previous stage for production deployment
FROM node:23-alpine AS production-stage
# set the working directory
WORKDIR /app
# Install http-server
RUN npm install -g http-server
# Copy the build files generated in the prev step to the current working directory
COPY --from=build-stage /app/dist /app
COPY --from=build-stage /app/cert.pem /app/
COPY --from=build-stage /app/key.pem /app/
# Expose the port 443
EXPOSE 443
# Serve it from http-server 
# CMD ["ls"]
CMD ["http-server", "-S", "-C", "cert.pem", "-K", "key.pem", "-p", "443"]
# --proxy - to handle routing correctly by serving index.html for all routes.
# -c-1: Disables caching by setting the cache TTL to -1