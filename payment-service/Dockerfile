# Stage 1: Build the Go binary
FROM golang:1.24-alpine AS builder

WORKDIR /app

# Copy dependency files first to leverage Docker cache
COPY go.mod go.sum ./
RUN go mod download

# Copy the rest of the source code
COPY . .

# Build a statically linked binary named 'main'
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

# Stage 2: Final lightweight image
FROM alpine:latest

# Install CA certificates for Stripe/External API communication
RUN apk --no-cache add ca-certificates

WORKDIR /root/

# Copy only the binary from the builder
COPY --from=builder /app/main .

# Your main.go uses port 8082
EXPOSE 8082

CMD ["./main"]
