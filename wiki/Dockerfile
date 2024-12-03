##STEP1
FROM golang:1.22 as builder

RUN mkdir -p /wiki

WORKDIR /wiki/

RUN go mod init wiki && go mod tidy 

COPY . /wiki/

#Be attention to your archicteture (mine it's arm64)
RUN CGO_ENABLED=0 GOOS=linux GOARCH=arm64 go build -o main ./main.go

EXPOSE 8080

CMD ["./main" ]

#STEP2
FROM alpine:latest

#Set the working directory
WORKDIR /wiki

#Copy the built executable from the builder stage
COPY --from=builder /wiki/main .
COPY --from=builder /wiki/edit.html /wiki/view.html ./

# Add a user for running the application
RUN adduser -S -D -H -h /wiki wiki && chown -R wiki: /wiki && apk add curl 

# Switch to the new user
USER wiki

# Command to run the executable
CMD ["./main"]