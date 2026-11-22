FROM golang:1.22-alpine AS builder

WORKDIR /usr/src/app/

RUN --mount=type=cache,target=/go/pkg/mod \
    --mount=type=cache,target=/root/.cache/go-build \
    mkdir -p /root/.cache/go-build
    

COPY go.mod go.sum ./

RUN go mod download

COPY . .

RUN go build -o product-catalog .

FROM alpine AS release

WORKDIR /usr/src/app/

COPY ./products/ ./products/

COPY --from=builder /usr/src/app/product-catalog .

ENV PRODUCT_CATALOG_PORT=8081

ENTRYPOINT ["./product-catalog"]

