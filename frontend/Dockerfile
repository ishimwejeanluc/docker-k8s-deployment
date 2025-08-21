# Build stage
FROM node:20-alpine AS builder

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

# Production stage
FROM nginx:stable-alpine

# Copy built assets from builder
COPY --from=builder /usr/src/app/dist /usr/share/nginx/html



# Copy custom nginx config if you have one
# COPY nginx.conf /etc/nginx/nginx.conf


# Expose port 80 for web traffic
EXPOSE 80

# Healthcheck for Nginx
HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
  CMD wget --quiet --tries=1 --spider http://localhost:80 || exit 1