FROM node:20-alpine AS builder

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app

COPY package.json package-lock.json* ./
RUN chown -R appuser:appgroup /app

USER appuser

RUN npm install
COPY --chown=appuser:appgroup . .
RUN npm run build

FROM nginx:stable-alpine

# Create required directories and fix permissions for nginx user
RUN mkdir -p /usr/share/nginx/buffer /usr/share/nginx/log /run \
 && chown -R nginx:nginx /usr/share/nginx /usr/share/nginx/buffer /usr/share/nginx/log /run /var/cache/nginx \
 && mkdir -p /var/cache/nginx && chown -R nginx:nginx /var/cache/nginx

COPY --from=builder /app/dist /usr/share/nginx/html


EXPOSE 80

HEALTHCHECK --interval=30s --timeout=5s --start-period=30s --retries=5 \
  CMD wget --quiet --tries=1 --spider http://localhost:80 || exit 1

USER nginx

CMD ["nginx", "-g", "daemon off;"]