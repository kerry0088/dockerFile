FROM golang:1.25-alpine AS builder

RUN apk add --no-cache \
    git \
    make

WORKDIR /src

RUN git clone \
    https://github.com/metatube-community/metatube-sdk-go.git \
    .

RUN make development


FROM alpine:latest

RUN apk add --no-cache \
    ca-certificates \
    tzdata

COPY --from=builder /src/build/metatube-server /metatube-server

ENV GIN_MODE=release
ENV PORT=8080
ENV TOKEN=""
ENV DSN=""
ENV REQUEST_TIMEOUT=""
ENV DB_MAX_IDLE_CONNS=0
ENV DB_MAX_OPEN_CONNS=0
ENV DB_PREPARED_STMT=0
ENV DB_AUTO_MIGRATE=0

EXPOSE 8080

ENTRYPOINT ["/metatube-server"]