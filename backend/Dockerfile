
FROM node:20-alpine

# Create and set the working directory
WORKDIR /usr/src/app

# Copy only package files first to leverage Docker cache for dependencies
COPY package*.json ./

# Install dependencies (use npm ci for clean installs)
RUN npm install --production

# Copy the rest of the application
COPY . .

# Change ownership of app files
RUN chown -R node:node /usr/src/app

# Switch to a non-root user
USER node

# Expose application port
EXPOSE 3000

# Add a health check (example: check HTTP status)
HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
  CMD wget --quiet --tries=1 --spider http://localhost:5000 

# Start the application
CMD ["node", "index.js"]