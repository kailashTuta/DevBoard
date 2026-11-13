# Build stage
FROM golang:1.24-alpine3.22 AS builder

WORKDIR /app

# Download dependencies
COPY go.mod go.sum ./
RUN apk add --no-cache --upgrade apk-tools && apk upgrade --no-cache && go mod download

# Build application
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o devboard-backend .

# Runtime stage
FROM alpine:3.22

WORKDIR /app

COPY --from=builder /app/devboard-backend ./devboard-backend

EXPOSE 8080

# Run as non-root
RUN apk add --no-cache --upgrade apk-tools && apk upgrade --no-cache && adduser -D -H appuser
USER appuser

ENTRYPOINT ["./devboard-backend"]
