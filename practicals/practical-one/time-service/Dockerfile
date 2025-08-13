FROM golang:1.24.2-alpine AS builder

WORKDIR /app

# Copy module files for dependency caching
COPY time-service/go.mod time-service/go.sum ./
RUN go mod download

# Copy service source
COPY time-service/ ./

# Copy shared proto files
COPY proto ./proto

# Build the binary
RUN CGO_ENABLED=0 GOOS=linux go build -o /app/server main.go

# Final lightweight image
FROM alpine:latest
WORKDIR /app
COPY --from=builder /app/server .
EXPOSE 50052
CMD ["/app/server"]
