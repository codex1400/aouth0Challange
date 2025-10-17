FROM golang:1.23-alpine AS builder

# Set environment variables for static build and module caching
ENV CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64

WORKDIR /app

# Copy go.mod and go.sum first for dependency caching
COPY ../../go.mod ../../go.sum ./
RUN go mod download

# Copy the rest of the source code
COPY . .

# Build the Go binary
RUN go build -trimpath -ldflags="-s -w" -o /demo ./main.go

# ---------- Final Stage ----------
FROM scratch

# Copy the statically built binary from builder stage
COPY --from=builder /app/demo /demo

# Set entrypoint
ENTRYPOINT ["/demo"]
