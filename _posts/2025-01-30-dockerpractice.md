---

layout: single
title:  "Docker A부터 Z까지!(실습편)"
categories: Development
tag: [docker]
toc: true
toc_sticky: true
toc_label: 목차
toc_icon: "fas fa-water"
author_profile: false
typora-root-url: ../
search: true
use_math: true
sidebar:
    nav: "counts"
---

이론편 다 보고 왔는가?

이 글은 도커에 대한 이론적인 바탕이 있다는 가정하에 바로 도커 실습에 들어갈 것이다.

# 도커 작동 상태 확인

### docker version

* 도커의 작동 상태를 확인할 수 있는 명령어
* Client부와 Server부로 분리되어 출력이 나오는 것을 확인할 수 있다.
* 호스트 OS에서 도커가 실행되고 있지 않다면, 아래와 같은 정상적인 응답이 나오지 않는다.

```bash
Last login: Thu Jan 30 20:36:06 on ttys000
➜  ~ docker version
Client:
Version:           27.4.0
API version:       1.47
Go version:        go1.22.10
Git commit:        bde2b89
Built:             Sat Dec  7 10:35:43 2024
OS/Arch:           darwin/arm64
Context:           desktop-linux

Server: Docker Desktop 4.37.1 (178610)
Engine:
  Version:          27.4.0
  API version:      1.47 (minimum version 1.24)
  Go version:       go1.22.10
  Git commit:       92a8393
  Built:            Sat Dec  7 10:38:33 2024
  OS/Arch:          linux/arm64
  Experimental:     false
containerd:
  Version:          1.7.21
  GitCommit:        472731909fa34bd7bc9c087e4c27943f9835f111
runc:
  Version:          1.1.13
  GitCommit:        v1.1.13-0-g58aa920
docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
➜  ~
```

### docker info

* docker version 보다 조금 더 상세한 정보가 출력된다.
* 클라이언트의 버전과 설치되어 있는 플러그인들의 종류를 확인할 수 있다.
* 서버 응답의 경우 현재 실행 중인 컨테이너의 개수, 이미지의 개수, 사용 중인 플러그인, 도커가 실행 중인 시스템의 OS 타입, CPU 아키텍쳐, CPU 코어, 메모리를 확인할 수 있다.

  ```bash
  ➜  ~ docker info
  Client:
  Version:    27.4.0
  Context:    desktop-linux
  Debug Mode: false
  Plugins:
    ai: Ask Gordon - Docker Agent (Docker Inc.)
      Version:  v0.5.1
      Path:     /Users/cream1nbbang/.docker/cli-plugins/docker-ai
    buildx: Docker Buildx (Docker Inc.)
      Version:  v0.19.2-desktop.1
      Path:     /Users/cream1nbbang/.docker/cli-plugins/docker-buildx
    compose: Docker Compose (Docker Inc.)
      Version:  v2.31.0-desktop.2
      Path:     /Users/cream1nbbang/.docker/cli-plugins/docker-compose
    debug: Get a shell into any image or container (Docker Inc.)
      Version:  0.0.37
      Path:     /Users/cream1nbbang/.docker/cli-plugins/docker-debug
    desktop: Docker Desktop commands (Beta) (Docker Inc.)
      Version:  v0.1.0
      Path:     /Users/cream1nbbang/.docker/cli-plugins/docker-desktop
    dev: Docker Dev Environments (Docker Inc.)
      Version:  v0.1.2
      Path:     /Users/cream1nbbang/.docker/cli-plugins/docker-dev
    extension: Manages Docker extensions (Docker Inc.)
      Version:  v0.2.27
      Path:     /Users/cream1nbbang/.docker/cli-plugins/docker-extension
    feedback: Provide feedback, right in your terminal! (Docker Inc.)
      Version:  v1.0.5
      Path:     /Users/cream1nbbang/.docker/cli-plugins/docker-feedback
    init: Creates Docker-related starter files for your project (Docker Inc.)
      Version:  v1.4.0
      Path:     /Users/cream1nbbang/.docker/cli-plugins/docker-init
    sbom: View the packaged-based Software Bill Of Materials (SBOM) for an image (Anchore Inc.)
      Version:  0.6.0
      Path:     /Users/cream1nbbang/.docker/cli-plugins/docker-sbom
    scout: Docker Scout (Docker Inc.)
      Version:  v1.15.1
      Path:     /Users/cream1nbbang/.docker/cli-plugins/docker-scout
  
  Server:
  Containers: 1
    Running: 1
    Paused: 0
    Stopped: 0
  Images: 4
  Server Version: 27.4.0
  Storage Driver: overlayfs
    driver-type: io.containerd.snapshotter.v1
  Logging Driver: json-file
  Cgroup Driver: cgroupfs
  Cgroup Version: 2
  Plugins:
    Volume: local
    Network: bridge host ipvlan macvlan null overlay
    Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
  CDI spec directories:
    /etc/cdi
    /var/run/cdi
  Swarm: inactive
  Runtimes: io.containerd.runc.v2 runc
  Default Runtime: runc
  Init Binary: docker-init
  containerd version: 472731909fa34bd7bc9c087e4c27943f9835f111
  runc version: v1.1.13-0-g58aa920
  init version: de40ad0
  Security Options:
    seccomp
    Profile: unconfined
    cgroupns
  Kernel Version: 6.10.14-linuxkit
  Operating System: Docker Desktop
  OSType: linux
  Architecture: aarch64
  CPUs: 10
  Total Memory: 7.654GiB
  Name: docker-desktop
  ID: 9e8cf031-938c-4b10-8b05-5a52ac09980a
  Docker Root Dir: /var/lib/docker
  Debug Mode: false
  HTTP Proxy: http.docker.internal:3128
  HTTPS Proxy: http.docker.internal:3128
  No Proxy: hubproxy.docker.internal
  Labels:
    com.docker.desktop.address=unix:///Users/cream1nbbang/Library/Containers/com.docker.docker/Data/docker-cli.sock
  Experimental: false
  Insecure Registries:
    hubproxy.docker.internal:5555
    127.0.0.0/8
  Live Restore Enabled: false
  
  WARNING: daemon is not using the default seccomp profile
  ➜  ~
  ```

# 메뉴얼 확인

### docker --help

* 
