# 1. Builder Stage
FROM golang:1.24-alpine AS builder

RUN apk add --no-cache git
WORKDIR /app

# Copy dependency files first
COPY go.mod go.sum ./
RUN go mod download

# Copy source code and build
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

# 2. Final Stage
FROM alpine:latest

RUN apk --no-cache add ca-certificates

WORKDIR /root/

# Copy only the binary
COPY --from=builder /app/main .

EXPOSE 8084
CMD ["./main"]
