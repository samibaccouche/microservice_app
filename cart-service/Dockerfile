# STAGE 1: Build the binary
FROM golang:1.24-alpine AS builder

# Install git (often needed for fetching Go dependencies)
RUN apk add --no-cache git

# Set the working directory
WORKDIR /app

# Copy the dependency files first (for better caching)
COPY go.mod go.sum ./
RUN go mod download

# Copy the rest of the source code
COPY . .

# Build the app into a static binary named 'main'
# We disable CGO to ensure the binary is portable
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

# STAGE 2: Create the final lightweight image
FROM alpine:latest

# Install CA certificates (needed for secure outbound calls)
RUN apk --no-cache add ca-certificates

WORKDIR /root/

# Copy only the compiled binary from the builder stage
COPY --from=builder /app/main .

# Expose the port we found in main.go
EXPOSE 8085

# Run the binary
CMD ["./main"]
