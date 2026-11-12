FROM golang:1.22-alpine AS builder

WORKDIR /app

# Download deps first for better layer caching.
COPY go.mod go.sum ./
RUN go mod download

# Build the binary.
COPY . .
RUN CGO_ENABLED=0 go build -o devboard-backend .

FROM dhi.io/golang:1.22-alpine3.20

COPY --from=builder /app/devboard-backend /devboard-backend

EXPOSE 8080

USER nonroot

ENTRYPOINT ["./devboard-backend"]
