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

### docker --help

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

### docker container --help

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

### docker container run --help

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

