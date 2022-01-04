# build-caddy-server

Turnkey solution for building [Caddy](https://caddyserver.com/) web server as a static Linux binary on `golang:1-alpine` by yourself.

This should help you get started with using Caddy without relying on the pre-built binaries.

**NOT TO BE CONFUSED** with [Docker images containing Caddy binary](https://github.com/mholt/caddy/wiki/Docker-Containers) which you can `docker run` in a production environment.

## Usage

    rpm --import https://mirror.go-repo.io/centos/RPM-GPG-KEY-GO-REPO
    curl -s https://mirror.go-repo.io/centos/go-repo.repo | tee /etc/yum.repos.d/go-repo.repo
    yum install golang-1.14 git -y
    mkdir -p ~/go/{bin,pkg,src}
    echo 'export GOPATH="$HOME/go"' >> ~/.bashrc
    echo 'export PATH="$PATH:${GOPATH//://bin:}/bin"' >> ~/.bashrc

    git clone https://github.com/klever1988/build-caddy-server-v1 && cd build-caddy-server-v1
    export GO111MODULE=on
    go mod tidy
    ./build.sh
    file caddy

### Volumes

* `/output`: Mount a directory to save the Caddy binary (_e.g._ `$(pwd)`)

### Environment Variables

* `VERSION`: Specify which version of Caddy to checkout and build.  The latest commit from `master` is used by default. _E.g._ you may want to use `docker run -e VERSION=1.0.0 ...` (this translates to `go get .../caddy@1.0.0` under the hood)
* `TELEMETRY`: Specify `true` or `false` depending on whether or not you want to enable the [telemetry](https://caddyserver.com/docs/telemetry) feature
* `PLUGINS`: See below

## Plugins

As an experimental feature, three plugins [`ratelimit`](https://caddyserver.com/docs/http.ratelimit), [`ipfilter`](https://caddyserver.com/docs/http.ipfilter), and [`prometheus`](https://caddyserver.com/docs/http.prometheus) are enabled by default.
If you want to specify which plugins to enable, list their repo paths in the `PLUGINS` env var, delimited by commas (`,`). You can omit `github.com/` if paths begin with it. _E.g._ `docker run -e PLUGINS= ...`)

## Links

* https://github.com/nodakai/build-caddy-server
* https://hub.docker.com/r/nodakai/build-caddy-server/
