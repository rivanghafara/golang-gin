FROM golang:1.14-alpine AS builder

WORKDIR /go/src/app

# Install git and get golang gin from github
RUN apk update && apk add --no-cache git
RUN go get -u github.com/golang/dep/cmd/dep
RUN go get -u github.com/gin-gonic/gin

# Copy the entire project and build it
# This layer is rebuilt when a file changes in the project directory
RUN dep init
RUN dep ensure -vendor-only

EXPOSE 9090

COPY . $WORKDIR


# This results in a single layer image
FROM alpine:latest
RUN apk update && apk upgrade && \
	apk --no-cache --update add ca-certificates tzdata && \
	mkdir /app && mkdir golang-gin
WORKDIR /app
EXPOSE 9090
COPY --from=builder /go/src/app /app
CMD /app/engine