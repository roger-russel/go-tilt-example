# syntax=docker/dockerfile:1.0-experimental
FROM golang:1.16-alpine AS builder
WORKDIR /api
RUN apk add --no-cache git

COPY go.mod go.sum /
RUN go mod download

RUN --mount="type=bind,src=.,target=/api" CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o /bin/api -ldflags="-s -w" ./cmd/api

FROM alpine as runner
WORKDIR /go/bin
COPY --from=builder /bin/api .
EXPOSE 8080
ENTRYPOINT /go/bin/api
