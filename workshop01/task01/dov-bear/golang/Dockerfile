FROM golang:1.19.4-alpine3.17 AS builder 
WORKDIR /app

ADD go.mod .
ADD go.sum .
ADD main.go .

RUN go build -o main main.go

# start a 2nd container 
FROM alpine:3.17
WORKDIR /app

COPY --from=builder /app/main main
COPY public public
COPY templates templates

ENV PORT=3000 INSTANCE_NAME="dov bear golang"
EXPOSE ${PORT}


HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
	CMD curl -s -f http://localhost:${PORT}/healthz || exit 1 


ENTRYPOINT ["/app/main"]
