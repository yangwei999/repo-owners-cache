FROM openeuler/openeuler:23.03 as BUILDER
RUN dnf update -y && \
    dnf install -y golang && \
    go env -w GOPROXY=https://goproxy.cn,direct

MAINTAINER zengchen1024<chenzeng765@gmail.com>

# build binary
WORKDIR /go/src/github.com/opensourceways/repo-owners-cache
COPY . .
RUN cd server && GO111MODULE=on CGO_ENABLED=0 go build -a -o server .

# copy binary config and utils
FROM openeuler/openeuler:22.03
RUN dnf -y update && \
    dnf in -y shadow && \
    groupadd -g 1000 repo-owners-cache && \
    useradd -u 1000 -g repo-owners-cache -s /bin/bash -m repo-owners-cache

USER repo-owners-cache

COPY  --chown=repo-owners-cache --from=BUILDER /go/src/github.com/opensourceways/repo-owners-cache/server/server /opt/app/repo-owners-cache

ENTRYPOINT ["/opt/app/repo-owners-cache"]
