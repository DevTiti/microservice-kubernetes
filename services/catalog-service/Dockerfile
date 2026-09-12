
# =========================
# Build stage
# =========================
FROM golang:1.22 AS builder

WORKDIR /build

# Copy dependency files first for Docker layer caching
COPY go.mod go.sum* ./

RUN go mod download

# Copy application source
COPY main.go .

# Build a static Linux binary
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 \
    go build -o catalog-service .

# =========================
# Runtime stage
# =========================
FROM gcr.io/distroless/static-debian12:nonroot

WORKDIR /app

COPY --from=builder /build/catalog-service .

EXPOSE 8082

USER nonroot:nonroot

ENTRYPOINT ["/app/catalog-service"]
