# Build stage
FROM golang:1.22-alpine AS builder

WORKDIR /app

# Download dependencies
COPY go.mod go.sum ./
RUN go mod download

# Build application
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o devboard-backend .

# Runtime stage
FROM alpine:3.20

WORKDIR /app

# Install CA certificates for HTTPS requests
RUN apk --no-cache add ca-certificates

COPY --from=builder /app/devboard-backend ./devboard-backend

EXPOSE 8080

# Run as non-root
RUN adduser -D -H appuser
USER appuser

ENTRYPOINT ["./devboard-backend"]
