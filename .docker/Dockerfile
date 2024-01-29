FROM golang:1.21.6-alpine3.19 as builder
WORKDIR /app
COPY ../go.mod go.sum ./
COPY internal/httpclient/go.* ./internal/httpclient/
RUN go mod download
COPY .. .
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build --ldflags "-s -w" -o hydra .

FROM alpine:3.19 AS ship
COPY --from=builder /app/hydra /usr/bin/hydra
EXPOSE 4444 4445
ENTRYPOINT ["hydra"]
CMD ["serve", "all"]
