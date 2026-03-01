# Building the binary of the App
FROM golang:1.19 AS build

WORKDIR /go/src/tasky
COPY go.mod go.sum ./
RUN go mod download

# IMPORTANT: copy everything (controllers/, models/, etc.)
COPY . .

RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o /go/src/tasky/tasky

FROM alpine:3.17.0 AS release
WORKDIR /app
COPY --from=build /go/src/tasky/tasky .
COPY --from=build /go/src/tasky/assets ./assets

# Copy the required file into the final runtime image
COPY --from=build /go/src/tasky/wizexercise.txt /app/wizexercise.txt

EXPOSE 8080
ENTRYPOINT ["/app/tasky"]

