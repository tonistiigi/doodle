FROM --platform=$BUILDPLATFORM golang:1.12-alpine AS builder
RUN apk add --no-cache git
WORKDIR /project
COPY cheers.go go.* .
RUN go mod download

ARG TARGETOS
ARG TARGETARCH
# much simpler with tonistiigi/xx
ENV GOOS=$TARGETOS GOARCH=$TARGETARCH
RUN CGO_ENABLED=0 go build -ldflags '-extldflags "-static"' -o cheers cheers.go

FROM scratch AS release-linux
COPY --from=builder /project/cheers /cheers
ENTRYPOINT ["/cheers"]

FROM  mcr.microsoft.com/windows/nanoserver:1809 AS release-windows
COPY --from=builder /project/cheers /cheers.exe
ENTRYPOINT ["\\cheers.exe"]

FROM release-$TARGETOS
