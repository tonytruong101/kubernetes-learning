# Build stage
FROM node:14.9.0-alpine as build

# Set the working directory
WORKDIR /app

# Copy the package.json and install dependencies
COPY package.json yarn.lock ./
RUN yarn install

# Copy the rest of the application code
COPY . .

# Build the application
RUN yarn build

# Deploy stage
FROM nginx:1.19.2-alpine as deploy

# Copy the built assets from the build stage
COPY --from=build /app/build /usr/share/nginx/html

# Copy the nginx configuration
COPY nginx.conf /etc/nginx/nginx.conf

# Expose the application port
EXPOSE 80

# Run the nginx server
CMD ["nginx", "-g", "daemon off;"]

