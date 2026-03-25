# 1. Builder Stage
FROM golang:1.24-alpine AS builder

# Install git if your dependencies need it
RUN apk add --no-cache git
WORKDIR /app

# Copy dependency files and download
COPY go.mod go.sum ./
RUN go mod download

# Copy source code and build the binary
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

# 2. Final Stage
FROM alpine:latest

# Install certificates for HTTPS (necessary if you talk to Mongo/Twilio/External APIs)


WORKDIR /root/

# Copy only the compiled binary from the builder stage
COPY --from=builder /app/main .

# Expose the port your code listens on (check main.go!)
EXPOSE 8083

CMD ["./main"]
