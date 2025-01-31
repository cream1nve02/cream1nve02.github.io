---
layout: single
title:  "Docker A부터 Z까지!(실습편)"
categories: Docker
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

---

# 도커 작동 상태 확인

## docker version

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

## docker info

* docker version 보다 조금 더 상세한 정보가 출력된다.
*  클라이언트의 버전과 설치되어 있는 플러그인들의 종류를 확인할 수 있다.
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
  ➜
```

# 메뉴얼 확인

## docker --help

* Management Commands와 Commands 확인 가능

``` bash
➜  ~ docker --help

Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Common Commands:
  run         Create and run a new container from an image
  exec        Execute a command in a running container
  ps          List containers
  build       Build an image from a Dockerfile
  pull        Download an image from a registry
  push        Upload an image to a registry
  images      List images
  login       Authenticate to a registry
  logout      Log out from a registry
  search      Search Docker Hub for images
  version     Show the Docker version information
  info        Display system-wide information

Management Commands:
  ai*         Ask Gordon - Docker Agent
  builder     Manage builds
  buildx*     Docker Buildx
  compose*    Docker Compose
  container   Manage containers
  context     Manage contexts
  debug*      Get a shell into any image or container
  desktop*    Docker Desktop commands (Beta)
  dev*        Docker Dev Environments
  extension*  Manages Docker extensions
  feedback*   Provide feedback, right in your terminal!
  image       Manage images
  init*       Creates Docker-related starter files for your project
  manifest    Manage Docker image manifests and manifest lists
  network     Manage networks
  plugin      Manage plugins
  sbom*       View the packaged-based Software Bill Of Materials (SBOM) for an image
  scout*      Docker Scout
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Swarm Commands:
  swarm       Manage Swarm

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  import      Import the contents from a tarball to create a filesystem image
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  wait        Block until one or more containers stop, then print their exit codes

Global Options:
      --config string      Location of client config files (default
                           "/Users/cream1nbbang/.docker")
  -c, --context string     Name of the context to use to connect to the
                           daemon (overrides DOCKER_HOST env var and
                           default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket to connect to
  -l, --log-level string   Set the logging level ("debug", "info",
                           "warn", "error", "fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default
                           "/Users/cream1nbbang/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default
                           "/Users/cream1nbbang/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default
                           "/Users/cream1nbbang/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Run 'docker COMMAND --help' for more information on a command.

For more help on how to use Docker, head to https://docs.docker.com/go/guides/
➜  ~
```

## docker container --help

* 컨테이너 명령 다음에 어떤 명령을 입력할 수 있는 지 확인하고 싶을 때 다음과 같이 입력하면 된다.

``` bash
➜  ~ docker container --help

Usage:  docker container COMMAND

Manage containers

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  exec        Execute a command in a running container
  export      Export a container's filesystem as a tar archive
  inspect     Display detailed information on one or more containers
  kill        Kill one or more running containers
  logs        Fetch the logs of a container
  ls          List containers
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  prune       Remove all stopped containers
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  run         Create and run a new container from an image
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker container COMMAND --help' for more information on a command.
➜  ~
```

## docker container run --help

* 컨테이너를 실행할 때 사용할 수 있는 옵션들을 확인할 수 있다.

``` bash
➜  ~ docker container run --help

Usage:  docker container run [OPTIONS] IMAGE [COMMAND] [ARG...]

Create and run a new container from an image

Aliases:
  docker container run, docker run

Options:
      --add-host list                    Add a custom host-to-IP mapping
                                         (host:ip)
      --annotation map                   Add an annotation to the
                                         container (passed through to the
                                         OCI runtime) (default map[])
  -a, --attach list                      Attach to STDIN, STDOUT or STDERR
      --blkio-weight uint16              Block IO (relative weight),
                                         between 10 and 1000, or 0 to
                                         disable (default 0)
      --blkio-weight-device list         Block IO weight (relative device
                                         weight) (default [])
      --cap-add list                     Add Linux capabilities
      --cap-drop list                    Drop Linux capabilities
      --cgroup-parent string             Optional parent cgroup for the
                                         container
      --cgroupns string                  Cgroup namespace to use
                                         (host|private)
                                         'host':    Run the container in
                                         the Docker host's cgroup
                                         namespace
                                         'private': Run the container in
                                         its own private cgroup namespace
                                         '':        Use the cgroup
                                         namespace as configured by the
                                                    default-cgroupns-mode
                                         option on the daemon (default)
      --cidfile string                   Write the container ID to the file
      --cpu-period int                   Limit CPU CFS (Completely Fair
                                         Scheduler) period
      --cpu-quota int                    Limit CPU CFS (Completely Fair
                                         Scheduler) quota
      --cpu-rt-period int                Limit CPU real-time period in
                                         microseconds
      --cpu-rt-runtime int               Limit CPU real-time runtime in
                                         microseconds
  -c, --cpu-shares int                   CPU shares (relative weight)
      --cpus decimal                     Number of CPUs
      --cpuset-cpus string               CPUs in which to allow execution
                                         (0-3, 0,1)
      --cpuset-mems string               MEMs in which to allow execution
                                         (0-3, 0,1)
  -d, --detach                           Run container in background and
                                         print container ID
      --detach-keys string               Override the key sequence for
                                         detaching a container
      --device list                      Add a host device to the container
      --device-cgroup-rule list          Add a rule to the cgroup allowed
                                         devices list
      --device-read-bps list             Limit read rate (bytes per
                                         second) from a device (default [])
      --device-read-iops list            Limit read rate (IO per second)
                                         from a device (default [])
      --device-write-bps list            Limit write rate (bytes per
                                         second) to a device (default [])
      --device-write-iops list           Limit write rate (IO per second)
                                         to a device (default [])
      --disable-content-trust            Skip image verification (default
                                         true)
      --dns list                         Set custom DNS servers
      --dns-option list                  Set DNS options
      --dns-search list                  Set custom DNS search domains
      --domainname string                Container NIS domain name
      --entrypoint string                Overwrite the default ENTRYPOINT
                                         of the image
  -e, --env list                         Set environment variables
      --env-file list                    Read in a file of environment
                                         variables
      --expose list                      Expose a port or a range of ports
      --gpus gpu-request                 GPU devices to add to the
                                         container ('all' to pass all GPUs)
      --group-add list                   Add additional groups to join
      --health-cmd string                Command to run to check health
      --health-interval duration         Time between running the check
                                         (ms|s|m|h) (default 0s)
      --health-retries int               Consecutive failures needed to
                                         report unhealthy
      --health-start-interval duration   Time between running the check
                                         during the start period
                                         (ms|s|m|h) (default 0s)
      --health-start-period duration     Start period for the container
                                         to initialize before starting
                                         health-retries countdown
                                         (ms|s|m|h) (default 0s)
      --health-timeout duration          Maximum time to allow one check
                                         to run (ms|s|m|h) (default 0s)
      --help                             Print usage
  -h, --hostname string                  Container host name
      --init                             Run an init inside the container
                                         that forwards signals and reaps
                                         processes
  -i, --interactive                      Keep STDIN open even if not attached
      --ip string                        IPv4 address (e.g., 172.30.100.104)
      --ip6 string                       IPv6 address (e.g., 2001:db8::33)
      --ipc string                       IPC mode to use
      --isolation string                 Container isolation technology
      --kernel-memory bytes              Kernel memory limit
  -l, --label list                       Set meta data on a container
      --label-file list                  Read in a line delimited file of
                                         labels
      --link list                        Add link to another container
      --link-local-ip list               Container IPv4/IPv6 link-local
                                         addresses
      --log-driver string                Logging driver for the container
      --log-opt list                     Log driver options
      --mac-address string               Container MAC address (e.g.,
                                         92:d0:c6:0a:29:33)
  -m, --memory bytes                     Memory limit
      --memory-reservation bytes         Memory soft limit
      --memory-swap bytes                Swap limit equal to memory plus
                                         swap: '-1' to enable unlimited swap
      --memory-swappiness int            Tune container memory swappiness
                                         (0 to 100) (default -1)
      --mount mount                      Attach a filesystem mount to the
                                         container
      --name string                      Assign a name to the container
      --network network                  Connect a container to a network
      --network-alias list               Add network-scoped alias for the
                                         container
      --no-healthcheck                   Disable any container-specified
                                         HEALTHCHECK
      --oom-kill-disable                 Disable OOM Killer
      --oom-score-adj int                Tune host's OOM preferences
                                         (-1000 to 1000)
      --pid string                       PID namespace to use
      --pids-limit int                   Tune container pids limit (set
                                         -1 for unlimited)
      --platform string                  Set platform if server is
                                         multi-platform capable
      --privileged                       Give extended privileges to this
                                         container
  -p, --publish list                     Publish a container's port(s) to
                                         the host
  -P, --publish-all                      Publish all exposed ports to
                                         random ports
      --pull string                      Pull image before running
                                         ("always", "missing", "never")
                                         (default "missing")
  -q, --quiet                            Suppress the pull output
      --read-only                        Mount the container's root
                                         filesystem as read only
      --restart string                   Restart policy to apply when a
                                         container exits (default "no")
      --rm                               Automatically remove the
                                         container and its associated
                                         anonymous volumes when it exits
      --runtime string                   Runtime to use for this container
      --security-opt list                Security Options
      --shm-size bytes                   Size of /dev/shm
      --sig-proxy                        Proxy received signals to the
                                         process (default true)
      --stop-signal string               Signal to stop the container
      --stop-timeout int                 Timeout (in seconds) to stop a
                                         container
      --storage-opt list                 Storage driver options for the
                                         container
      --sysctl map                       Sysctl options (default map[])
      --tmpfs list                       Mount a tmpfs directory
  -t, --tty                              Allocate a pseudo-TTY
      --ulimit ulimit                    Ulimit options (default [])
  -u, --user string                      Username or UID (format:
                                         <name|uid>[:<group|gid>])
      --userns string                    User namespace to use
      --uts string                       UTS namespace to use
  -v, --volume list                      Bind mount a volume
      --volume-driver string             Optional volume driver for the
                                         container
      --volumes-from list                Mount volumes from the specified
                                         container(s)
  -w, --workdir string                   Working directory inside the
                                         container
➜  ~
```



# Nginx 실행 및 삭제하기

##  docker run -p 80:80 --name hellonginx nginx

* 웹 서버 기능을 제공하는 Nginx 이미지를 다운받고 hellonginx 컨테이너를 생성하여 실행하는 명령어는 다음과 같이 입력할 수 있다.
* nginx는 한 번 실행시키면 사용자가 종료 명령 (Ctrl + C)을 주기 전까지 터미널을 점유하면서 실행 상태를 유지한다.
* 실행 후 웹 브라우저 상에 localhost라고 쳐서 들어가면 Nginx에 접근할 수 있다.

```bash
➜  ~ docker run -p 80:80 --name hellonginx nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
066d623ff8e6: Download complete
49486a4a61a6: Download complete
6d24e34787c7: Download complete
2b39a3d0829e: Download complete
34d83bb3522a: Download complete
7ce705000c39: Download complete
b3e9225c8fca: Download complete
Digest: sha256:0a399eb16751829e1af26fea27b20c3ec28d7ab1fb72182879dcae1cca21206a
Status: Downloaded newer image for nginx:latest
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2025/01/30 23:55:02 [notice] 1#1: using the "epoll" event method
2025/01/30 23:55:02 [notice] 1#1: nginx/1.27.3
2025/01/30 23:55:02 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14)
2025/01/30 23:55:02 [notice] 1#1: OS: Linux 6.10.14-linuxkit
2025/01/30 23:55:02 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2025/01/30 23:55:02 [notice] 1#1: start worker processes
2025/01/30 23:55:02 [notice] 1#1: start worker process 29
2025/01/30 23:55:02 [notice] 1#1: start worker process 30
2025/01/30 23:55:02 [notice] 1#1: start worker process 31
2025/01/30 23:55:02 [notice] 1#1: start worker process 32
2025/01/30 23:55:02 [notice] 1#1: start worker process 33
2025/01/30 23:55:02 [notice] 1#1: start worker process 34
2025/01/30 23:55:02 [notice] 1#1: start worker process 35
2025/01/30 23:55:02 [notice] 1#1: start worker process 36
2025/01/30 23:55:02 [notice] 1#1: start worker process 37
2025/01/30 23:55:02 [notice] 1#1: start worker process 38

```

## docker rm hellonginx

* 만들어진 hellonginx를 제거하기 위해서는 다음과 같은 명령어를 사용한다.
* 해당 컨테이너의 이름이 출력된다면, 정상적으로 제거된 것이다.

``` bash
➜  ~ docker rm hellonginx
hellonginx
➜  ~
```

# Docker 기본 명령어

## docker Image ls

* 현재 로컬 PC에 존재하는 모든 이미지를 조회할 수 있다.

```bash
➜  ~ docker image ls
REPOSITORY                             TAG       IMAGE ID       CREATED        SIZE
ubuntu_gui                             latest    4cd8ada2778e   2 weeks ago    9.73GB
<none>                                 <none>    f9a4fec9d324   3 weeks ago    166MB
portainer/portainer-docker-extension   2.21.5    f57d844d2311   6 weeks ago    391MB
nginx                                  latest    0a399eb16751   2 months ago   280MB
➜  ~
```

* 특정 이름의 이미지만 조회할 수도 있다.
  * REPOSITORY : 이미지의 이름
  * TAG : 이미지의 버전
  * IMAGE ID : 고유한 ID
  * CREATED : 만들어진 날짜
  * SIZE : 용량

```bash
➜  ~ docker image ls nginx
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    0a399eb16751   2 months ago   280MB
➜  ~
```

## docker run -d --name {컨테이너명} {이미지명}

* 컨테이너명을 사용자가 지정하면서 컨테이너를 생성할 수 있다.
* -d : 백그라운드에서 컨테이너를 실행한다.
* 아래 명령어는 nginx 이미지를 백그라운드 형태로 3개의 컨테이너를 생성한다.

```bash
➜  ~ docker run -d --name multinginx1 nginx
6ba9db40f7ff998a048f6937b5a10cd82aff88047845a780e8f6a678c7537b51
➜  ~ docker run -d --name multinginx2 nginx
a045da889d2c621ab2478def39111299057b36e32ce36af28df383643fe0698e
➜  ~ docker run -d --name multinginx3 nginx
c0203737561b745681e2d423f3660147a17623fb0e30f5edaaac451bc944815d
➜  ~
```

## docker ps

* 현재 실행 중인 컨테이너 리스트를 조회하고 싶을 때 사용한다.
* 현재 3개의 컨테이너가 실행중인 것을 확인할 수 있다.

```bash
➜  ~ docker ps
CONTAINER ID   IMAGE     COMMAND                   CREATED         STATUS         PORTS     NAMES
c0203737561b   nginx     "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   80/tcp    multinginx3
a045da889d2c   nginx     "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   80/tcp    multinginx2
6ba9db40f7ff   nginx     "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   80/tcp    multinginx1
➜  ~
```

* 종료된 컨테이너를 모두 확인하고 싶다면, docker ps -a 를 사용한다.

```bash
➜  ~ docker ps -a
CONTAINER ID   IMAGE     COMMAND                   CREATED          STATUS                     PORTS     NAMES
b4f3e70745ac   nginx     "/docker-entrypoint.…"   6 minutes ago    Exited (0) 6 minutes ago             customCmd
6a094bb23363   nginx     "/docker-entrypoint.…"   12 minutes ago   Up 12 minutes              80/tcp    defaultCmd
➜  ~
```



## docker rm -f

* docker rm 명령만으로는 실행 중인 컨테이너를 삭제할 수 없다.
* 실행 중인 컨테이너를 삭제하고 싶을 때에는 -f 옵션을 사용해야 한다.
* 복수의 컨테이너를 한 번에 삭제하는 것도 가능하다.

```bash
 ➜  ~ docker rm multinginx1
Error response from daemon: cannot remove container "/multinginx1": container is running: stop the container before removing or force remove
➜  ~ docker rm -f multinginx1
multinginx1
➜  ~ docker rm -f multinginx2 multinginx3
multinginx2
multinginx3
➜  ~
```

# Docker inspect

도커 이미지의 메타데이터를 덮어 씌어보기 위한 명령어들이다.

## docker image inspect 이미지명

* 아래 명령을 통해 nginx 이미지의 메타데이터를 확인할 수 있다.

* 이미지 아이디, 태그, 생성된 시간, Env, Cmd 필드 확인 가능
* Env : nginx 버전, PATH 환경 변수 확인
* Cmd : nginx -g daemon off

````bash
➜  ~ docker image inspect nginx
[
    {
        "Id": "sha256:0a399eb16751829e1af26fea27b20c3ec28d7ab1fb72182879dcae1cca21206a",
        "RepoTags": [
            "nginx:latest"
        ],
        "RepoDigests": [
            "nginx@sha256:0a399eb16751829e1af26fea27b20c3ec28d7ab1fb72182879dcae1cca21206a"
        ],
        "Parent": "",
        "Comment": "buildkit.dockerfile.v0",
        "Created": "2024-11-26T18:42:08Z",
        "DockerVersion": "27.4.0",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.27.3",
                "NJS_VERSION=0.8.7",
                "NJS_RELEASE=1~bookworm",
                "PKG_RELEASE=1~bookworm",
                "DYNPKG_RELEASE=1~bookworm"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "Image": "",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGQUIT"
        },
        "Architecture": "arm64",
        "Variant": "v8",
        "Os": "linux",
        "Size": 68507108,
        "GraphDriver": {
            "Data": null,
            "Name": "overlayfs"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:6f51610e98184af12bb1cfab40f1ae13928363f1ea9aa19138467f09167e9809",
                "sha256:4262c14e65b24d2d87a97a8436492beb3ae0cbcbd26382d4fecf4aa358c8eb04",
                "sha256:6e2c7c0557ed32915c8243a004dfe3012a768aad48884a4f6a6f8c8afc45d4db",
                "sha256:b0ca2628b7242315658a2db0aebf1aaa9114cd7fe0bc0f295a3c05eaa9b7a93d",
                "sha256:5e4bbfe5d6499f37b5b697090ef1ed7da02df2de2d5be4888e8aff8b331f3a67",
                "sha256:71639f1687ea9c31ae88f43ea09f5d3c6c6923bd33e4e97391df4d34941b221f",
                "sha256:3a08247cbce95b8f5301a4c2397aeaa4ff49222d9a8a400e9228951605763c25"
            ]
        },
        "Metadata": {
            "LastTagTime": "2025-01-30T23:55:01.433892167Z"
        }
    }
]
➜  ~
````



## docker container inspect 컨테이너명

``` bash
➜  ~  docker container inspect defaultCmd
[
    {
        "Id": "6a094bb233637300079ab4269288607930412eabd2aa4b34c797224e5d9a852f",
        "Created": "2025-01-31T01:46:30.90737943Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "nginx",
            "-g",
            "daemon off;"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 21078,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2025-01-31T01:46:30.95778518Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:0a399eb16751829e1af26fea27b20c3ec28d7ab1fb72182879dcae1cca21206a",
        "ResolvConfPath": "/var/lib/docker/containers/6a094bb233637300079ab4269288607930412eabd2aa4b34c797224e5d9a852f/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/6a094bb233637300079ab4269288607930412eabd2aa4b34c797224e5d9a852f/hostname",
        "HostsPath": "/var/lib/docker/containers/6a094bb233637300079ab4269288607930412eabd2aa4b34c797224e5d9a852f/hosts",
        "LogPath": "/var/lib/docker/containers/6a094bb233637300079ab4269288607930412eabd2aa4b34c797224e5d9a852f/6a094bb233637300079ab4269288607930412eabd2aa4b34c797224e5d9a852f-json.log",
        "Name": "/defaultCmd",
        "RestartCount": 0,
        "Driver": "overlayfs",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "bridge",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                25,
                80
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "private",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": null,
            "PidsLimit": null,
            "Ulimits": [],
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware",
                "/sys/devices/virtual/powercap"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": null,
            "Name": "overlayfs"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "6a094bb23363",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.27.3",
                "NJS_VERSION=0.8.7",
                "NJS_RELEASE=1~bookworm",
                "PKG_RELEASE=1~bookworm",
                "DYNPKG_RELEASE=1~bookworm"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "Image": "nginx",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers \u003cdocker-maint@nginx.com\u003e"
            },
            "StopSignal": "SIGQUIT"
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "895903b28a9edc8acabe791ec990d657e506257099f4805a5eeb467f27c4f1f5",
            "SandboxKey": "/var/run/docker/netns/895903b28a9e",
            "Ports": {
                "80/tcp": null
            },
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "e89a1c36d5074fe09ffe675acdebebbd253bfdacc137eb2f9d881070136ae1de",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null,
                    "NetworkID": "0be5482a767f8d499ec894818bb10d24841880dec8d088ee30580a070f704cfe",
                    "EndpointID": "e89a1c36d5074fe09ffe675acdebebbd253bfdacc137eb2f9d881070136ae1de",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "DNSNames": null
                }
            }
        }
    }
]
➜  ~           "Links": null,
                    "Aliases": null,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null,
                    "NetworkID": "0be5482a767f8d499ec894818bb10d24841880dec8d088ee30580a070f704cfe",
                    "EndpointID": "e89a1c36d5074fe09ffe675acdebebbd253bfdacc137eb2f9d881070136ae1de",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "DNSNames": null
                }
            }
        }
    }
]
```



## docker run 이미지명 (실행명령)

* nginx 이미지를 customCmd 이름의 컨테이너로 만들고 실행하면서 사용자가 지정한 명령을 출력을 수행하도록 하였다.
* cat 명령어는 파일의 내용을 출력하고 종료하는 일회성 프로세스이므로, 컨테이너도 실행 후 바로 종료된다.
* 컨테이너는 생성될 때 이미지의 데이터를 복사하여 만들어지기 때문에 실제로 컨테이너의 메타데이터를 바꾼다고 해서 이미지의 메타데이터가 바뀌지는 않는다.

```bash
➜  ~ docker run --name customCmd nginx cat usr/share/nginx/html/index.html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
➜  ~
```



## docker run -environment KEY=VALUE 이미지명

* -environment 대신 -e를 적어도 가능하다.
* ENV Node Color App : 사용자에게 특정 색상의 웹페이지를 응답해준다.
  *  Node.js라는 JavaScript 기반으로 개발된 백엔드 애플리케이션이다.

* 다음은 ENV Node Color App 이미지의 메타데이터이다.

```bash
➜  ~ docker image inspect devwikirepo/envnodecolorapp
[
    {
        "Id": "sha256:982b6f0d4e65f3dc3394ed1057f35a09fb7e4968f48e1658031952988fcaa096",
        "RepoTags": [
            "devwikirepo/envnodecolorapp:latest"
        ],
        "RepoDigests": [
            "devwikirepo/envnodecolorapp@sha256:982b6f0d4e65f3dc3394ed1057f35a09fb7e4968f48e1658031952988fcaa096"
        ],
        "Parent": "",
        "Comment": "buildkit.dockerfile.v0",
        "Created": "2024-01-01T06:21:55.733841921Z",
        "DockerVersion": "27.4.0",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "node",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "3000/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NODE_VERSION=14.21.3",
                "YARN_VERSION=1.22.19",
                "COLOR=red"
            ],
            "Cmd": [
                "npm",
                "start"
            ],
            "ArgsEscaped": true,
            "Image": "",
            "Volumes": null,
            "WorkingDir": "/app",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "arm64",
        "Os": "linux",
        "Size": 342245511,
        "GraphDriver": {
            "Data": null,
            "Name": "overlayfs"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:173621e7addd9a122d7aa8c00c6741b6b4426b7b76d65cd26e33c42defc8f4d9",
                "sha256:cfec405a23bc89639c747f170cc165513ee5689c72bb9f97bf3867b332e12e46",
                "sha256:f3e76aaba7cc466ba4d2e82fc9793e1f31a0aa02cfeb3f0d8f6daee49cb6ec4e",
                "sha256:82da4502ad28ee2d71f5f6706b53a848c9af3cf0a86c9c28cbb4e9d3c295827c",
                "sha256:708291ce05346c36e436ab8b6f65e9cb194ead0fa4be7b7a9737c7b84f768e62",
                "sha256:476197c298e4e088678ca20c8926bf187cbc0966548706119c1a3afcb4cf1ca6",
                "sha256:20649f23472c55c0d1a6f51859a4cdf1c0ee387d1b6e1c6d7dff142e3e510150",
                "sha256:b078da4dfde3335d754012aa1f4999f014a9d92c9d974365a504b40300011030",
                "sha256:4d772e48367e80db04ffe51d27570fa07be0fe9da239ae16c0cc5767054f4742",
                "sha256:2598f020922d939102af4c19dd7d0bdfbad8c610758b90b16eac83cce0891ac8",
                "sha256:e57ecbb84cded9ca13582528286f225e0295c10e0dfd0b8fdf6e54dc67fe4974",
                "sha256:3ede61dfeb2e455c3af3c139df7bb3a69001051e802a2d59854071cab4aa7a8b"
            ]
        },
        "Metadata": {
            "LastTagTime": "2025-01-31T02:18:37.083419668Z"
        }
    }
]
➜  ~
```

* 다음 명령을 통해 기본 컨테이너를 실행해보자.

```bash
➜  ~ docker run -d -p 8000:3000 --name defaultColorApp devwikirepo/envnodecolorapp
0c5b7ef124f28bcf3461e9a8b11a77392709574d0643d98b533ab10869e357f6
➜  ~
```

* 아래 명령을 통해 env 필드를 새롭게 덮어씌운 상태로 새로운 컨테이너를 실행할 수 있다.

```bash
➜  ~ docker run -d -p 8081:3000 --name blueColorApp --env COLOR=blue devwikirepo/envnodecolorapp
a7ccc6e935349149d6e884e8cb431b8e9a963446ec000eea61c8f2d0369aadf5
➜  ~
```

* blueColorApp 컨테이너의 COLOR env가 blue로 변경된 것을 확인할 수 있다.

```bash
➜  ~ docker container inspect blueColorApp
[
    {
        "Id": "a7ccc6e935349149d6e884e8cb431b8e9a963446ec000eea61c8f2d0369aadf5",
        "Created": "2025-01-31T02:23:16.062976297Z",
        "Path": "docker-entrypoint.sh",
        "Args": [
            "npm",
            "start"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 21579,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2025-01-31T02:23:16.108974506Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:982b6f0d4e65f3dc3394ed1057f35a09fb7e4968f48e1658031952988fcaa096",
        "ResolvConfPath": "/var/lib/docker/containers/a7ccc6e935349149d6e884e8cb431b8e9a963446ec000eea61c8f2d0369aadf5/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/a7ccc6e935349149d6e884e8cb431b8e9a963446ec000eea61c8f2d0369aadf5/hostname",
        "HostsPath": "/var/lib/docker/containers/a7ccc6e935349149d6e884e8cb431b8e9a963446ec000eea61c8f2d0369aadf5/hosts",
        "LogPath": "/var/lib/docker/containers/a7ccc6e935349149d6e884e8cb431b8e9a963446ec000eea61c8f2d0369aadf5/a7ccc6e935349149d6e884e8cb431b8e9a963446ec000eea61c8f2d0369aadf5-json.log",
        "Name": "/blueColorApp",
        "RestartCount": 0,
        "Driver": "overlayfs",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "bridge",
            "PortBindings": {
                "3000/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "8081"
                    }
                ]
            },
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                25,
                80
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "private",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": null,
            "PidsLimit": null,
            "Ulimits": [],
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware",
                "/sys/devices/virtual/powercap"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": null,
            "Name": "overlayfs"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "a7ccc6e93534",
            "Domainname": "",
            "User": "node",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "3000/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "COLOR=blue",
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NODE_VERSION=14.21.3",
                "YARN_VERSION=1.22.19"
            ],
            "Cmd": [
                "npm",
                "start"
            ],
            "Image": "devwikirepo/envnodecolorapp",
            "Volumes": null,
            "WorkingDir": "/app",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "561f1214315f8c227be328b40d967df9d95bfac9c93de5eebef56c339b036b28",
            "SandboxKey": "/var/run/docker/netns/561f1214315f",
            "Ports": {
                "3000/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "8081"
                    }
                ]
            },
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "fd12b04fa69e085056c38e3b6e20556f14cf9a23a66ae7463ceb38b1eeeace71",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.4",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:04",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "MacAddress": "02:42:ac:11:00:04",
                    "DriverOpts": null,
                    "NetworkID": "0be5482a767f8d499ec894818bb10d24841880dec8d088ee30580a070f704cfe",
                    "EndpointID": "fd12b04fa69e085056c38e3b6e20556f14cf9a23a66ae7463ceb38b1eeeace71",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.4",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "DNSNames": null
                }
            }
        }
    }
]
➜  ~
```

