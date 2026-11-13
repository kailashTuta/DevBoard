# Stage 1 — build the app into static files
FROM node:20.19.2-alpine3.22 AS build
WORKDIR /app
COPY package*.json ./
RUN apk add --no-cache --upgrade apk-tools && apk upgrade --no-cache && npm ci --legacy-peer-deps --no-audit
COPY . .
RUN npm run build

# Stage 2 — small image that only serves the build (no nginx)
FROM node:20.19.2-alpine3.22
WORKDIR /app
RUN apk add --no-cache --upgrade apk-tools && apk upgrade --no-cache && npm install -g vite@^6.4.18 && npm cache clean --force
COPY --from=build /app/dist ./dist
COPY vite.preview.config.js ./vite.config.js
EXPOSE 4173
CMD ["vite", "preview", "--host", "0.0.0.0", "--port", "4173"]
