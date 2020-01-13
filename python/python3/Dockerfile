FROM golang:buster AS build
LABEL maintainer="jeff.landis@reckonpoint.com" \
    organization="Reckon Point LLC" \
    description="A command-line tool to perform health-checks for gRPC applications in Kubernetes etc." \
    version="0.3.1" \
    source="https://github.com/reckonpoint/grpc-health-probe/tree/rp-v0.3.1" \
    license="https://github.com/reckonpoint/grpc-health-probe/blob/rp-v0.3.1/LICENSE" \
    readme="https://github.com/reckonpoint/grpc-health-probe/blob/rp-v0.3.1/README.md" \
    contributors="https://github.com/reckonpoint/grpc-health-probe/graphs/contributors" \
    forked-from="https://github.com/grpc-ecosystem/grpc-health-probe"
ENV _GRPC_HEALTH_PROBE_VERSION=0.3.1
ENV PROJECT grpc_health_probe
WORKDIR /src/$PROJECT
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go install -a -tags netgo -ldflags=-w

FROM python:3.7-slim-buster
ENV _PYTHON_VERSION 3.7
COPY --from=build /go/bin/grpc-health-probe /bin/grpc_health_probe
CMD [ "/bin/bash", "-c", "/bin/grpc_health_probe" ]
