# Copyright 2024 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM --platform=$BUILDPLATFORM google-go.pkg.dev/golang:1.24.0@sha256:f0ed15de6a2c1d1e595b8b3dfaf2dc6af66a38c1891591c4903bd3e1b2e76518 AS buildbase
ARG TARGETOS
ARG TARGETARCH
WORKDIR /app
COPY . ./

FROM buildbase AS appbase

ENV GOEXPERIMENT=noboringcrypto
ENV CGO_ENABLED=0
ENV GOFIPS140=latest
ENV GOOS=${TARGETOS}
ENV GOARCH=${TARGETARCH}
RUN go build -mod=vendor -o go-synthetic ./examples/instrumentation/go-synthetic

FROM gcr.io/distroless/static-debian12:nonroot
COPY --from=appbase /app/go-synthetic /bin/go-synthetic
ENTRYPOINT ["/bin/go-synthetic"]
