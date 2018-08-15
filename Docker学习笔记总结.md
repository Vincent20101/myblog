Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从Apache2.0协议开源。

Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。


docker info 
```bash

$ sudo docker info 
Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 0
Server Version: 1.13.1
Storage Driver: aufs
 Root Dir: /var/lib/docker/aufs
 Backing Filesystem: extfs
 Dirs: 0
 Dirperm1 Supported: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins: 
 Volume: local
 Network: bridge host macvlan null overlay
Swarm: inactive
Runtimes: runc
Default Runtime: runc
Init Binary: docker-init
containerd version:  (expected: aa8187dbd3b7ad67d8e5e3a15115d3eef43a7ed1)
runc version: N/A (expected: 9df8b306d01f59d3a8029be411de015b7304dd8f)
init version: N/A (expected: 949e6facb77383876aeff8a6944dde66b3089574)
Security Options:
 apparmor
 seccomp
  Profile: default
Kernel Version: 4.4.0-130-generic
Operating System: Ubuntu 16.04.4 LTS
OSType: linux
Architecture: x86_64
CPUs: 8
Total Memory: 15.59 GiB
Name: linux-pc
ID: VWSC:LJG5:54QS:36UP:7CIP:CKML:NHHE:KUQ7:QBH7:LDRN:5DKB:7XMM
Docker Root Dir: /var/lib/docker
Debug Mode (client): false
Debug Mode (server): false
Registry: https://index.docker.io/v1/
WARNING: No swap limit support
Experimental: false
Insecure Registries:
 127.0.0.0/8
Live Restore Enabled: false
$                          

```

docker help
```bash
$ docker help 

Usage:	docker COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default "/home/mamh/.docker")
  -D, --debug              Enable debug mode
      --help               Print usage
  -H, --host list          Daemon socket(s) to connect to (default [])
  -l, --log-level string   Set the logging level ("debug", "info", "warn", "error", "fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default "/home/mamh/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/home/mamh/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/home/mamh/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  container   Manage containers
  image       Manage images
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  volume      Manage volumes

Commands:
  attach      Attach to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker COMMAND --help' for more information on a command.



```

docker version
```bash
$ sudo docker version  
Client:
 Version:      1.13.1
 API version:  1.26
 Go version:   go1.6.2
 Git commit:   092cba3
 Built:        Thu Nov  2 20:40:23 2017
 OS/Arch:      linux/amd64

Server:
 Version:      1.13.1
 API version:  1.26 (minimum version 1.12)
 Go version:   go1.6.2
 Git commit:   092cba3
 Built:        Thu Nov  2 20:40:23 2017
 OS/Arch:      linux/amd64
 Experimental: false

```

docker images
```bash

$ sudo docker images 
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
httpd               latest              11426a19f1a2        13 days ago         178 MB
ubuntu              latest              735f80812f90        2 weeks ago         83.5 MB
ubuntu              14.04               971bb384a50a        4 weeks ago         188 MB
ubuntu              15.10               9b9cb95443b5        2 years ago         137 MB
training/webapp     latest              6fae60ef3446        3 years ago         349 MB


$ docker images --help  

Usage:  docker images [OPTIONS] [REPOSITORY[:TAG]]

List images

Options:
  -a, --all             Show all images (default hides intermediate images)
      --digests         Show digests
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print images using a Go template
      --help            Print usage
      --no-trunc        Don't truncate output
  -q, --quiet           Only show numeric IDs

```


docker commit
```bash

$ docker commit --help 

Usage:      docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

Create a new image from a container's changes

Options:
  -a, --author string    Author (e.g., "John Hannibal Smith <hannibal@a-team.com>")
  -c, --change list      Apply Dockerfile instruction to the created image (default [])
      --help             Print usage
  -m, --message string   Commit message
  -p, --pause            Pause container during commit (default true)


docker commit -a 'mamh' -m 'remove .cpan dir' 9b9229abdd7a bs-ubuntu-14.04:1.2
```

docker cp
```bash
# 复制文件，可以从host复制到容器内，也可以反过来复制的
$ docker cp --help 

Usage:  docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
        docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH

Copy files/folders between a container and the local filesystem

Options:
  -L, --follow-link   Always follow symbol link in SRC_PATH
      --help          Print usage


```

docker pull
```bash
# 从服务器拉取指定的镜像
$ docker pull --help 

Usage:    docker pull [OPTIONS] NAME[:TAG|@DIGEST]

Pull an image or a repository from a registry

Options:
  -a, --all-tags                Download all tagged images in the repository
      --disable-content-trust   Skip image verification (default true)
      --help                    Print usage


```


docker search
```bash
# 搜索docker image 镜像
$ docker search --help 

Usage:      docker search [OPTIONS] TERM

Search the Docker Hub for images

Options:
  -f, --filter filter   Filter output based on conditions provided
      --help            Print usage
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output


```

docker run
````bash
$ docker run --help 

Usage:	docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Run a command in a new container

Options:
      --add-host list                         Add a custom host-to-IP mapping (host:ip) (default [])
  -a, --attach list                           Attach to STDIN, STDOUT or STDERR (default [])
      --blkio-weight uint16                   Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0)
      --blkio-weight-device weighted-device   Block IO weight (relative device weight) (default [])
      --cap-add list                          Add Linux capabilities (default [])
      --cap-drop list                         Drop Linux capabilities (default [])
      --cgroup-parent string                  Optional parent cgroup for the container
      --cidfile string                        Write the container ID to the file
      --cpu-count int                         CPU count (Windows only)
      --cpu-percent int                       CPU percent (Windows only)
      --cpu-period int                        Limit CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int                         Limit CPU CFS (Completely Fair Scheduler) quota
      --cpu-rt-period int                     Limit CPU real-time period in microseconds
      --cpu-rt-runtime int                    Limit CPU real-time runtime in microseconds
  -c, --cpu-shares int                        CPU shares (relative weight)
      --cpus decimal                          Number of CPUs (default 0.000)
      --cpuset-cpus string                    CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string                    MEMs in which to allow execution (0-3, 0,1)
      --credentialspec string                 Credential spec for managed service account (Windows only)
  -d, --detach                                Run container in background and print container ID
      --detach-keys string                    Override the key sequence for detaching a container
      --device list                           Add a host device to the container (default [])
      --device-read-bps throttled-device      Limit read rate (bytes per second) from a device (default [])
      --device-read-iops throttled-device     Limit read rate (IO per second) from a device (default [])
      --device-write-bps throttled-device     Limit write rate (bytes per second) to a device (default [])
      --device-write-iops throttled-device    Limit write rate (IO per second) to a device (default [])
      --disable-content-trust                 Skip image verification (default true)
      --dns list                              Set custom DNS servers (default [])
      --dns-option list                       Set DNS options (default [])
      --dns-search list                       Set custom DNS search domains (default [])
      --entrypoint string                     Overwrite the default ENTRYPOINT of the image
  -e, --env list                              Set environment variables (default [])
      --env-file list                         Read in a file of environment variables (default [])
      --expose list                           Expose a port or a range of ports (default [])
      --group-add list                        Add additional groups to join (default [])
      --health-cmd string                     Command to run to check health
      --health-interval duration              Time between running the check (ns|us|ms|s|m|h) (default 0s)
      --health-retries int                    Consecutive failures needed to report unhealthy
      --health-timeout duration               Maximum time to allow one check to run (ns|us|ms|s|m|h) (default 0s)
      --help                                  Print usage
  -h, --hostname string                       Container host name
      --init                                  Run an init inside the container that forwards signals and reaps processes
      --init-path string                      Path to the docker-init binary
  -i, --interactive                           Keep STDIN open even if not attached
      --io-maxbandwidth string                Maximum IO bandwidth limit for the system drive (Windows only)
      --io-maxiops uint                       Maximum IOps limit for the system drive (Windows only)
      --ip string                             Container IPv4 address (e.g. 172.30.100.104)
      --ip6 string                            Container IPv6 address (e.g. 2001:db8::33)
      --ipc string                            IPC namespace to use
      --isolation string                      Container isolation technology
      --kernel-memory string                  Kernel memory limit
  -l, --label list                            Set meta data on a container (default [])
      --label-file list                       Read in a line delimited file of labels (default [])
      --link list                             Add link to another container (default [])
      --link-local-ip list                    Container IPv4/IPv6 link-local addresses (default [])
      --log-driver string                     Logging driver for the container
      --log-opt list                          Log driver options (default [])
      --mac-address string                    Container MAC address (e.g. 92:d0:c6:0a:29:33)
  -m, --memory string                         Memory limit
      --memory-reservation string             Memory soft limit
      --memory-swap string                    Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --memory-swappiness int                 Tune container memory swappiness (0 to 100) (default -1)
      --name string                           Assign a name to the container
      --network string                        Connect a container to a network (default "default")
      --network-alias list                    Add network-scoped alias for the container (default [])
      --no-healthcheck                        Disable any container-specified HEALTHCHECK
      --oom-kill-disable                      Disable OOM Killer
      --oom-score-adj int                     Tune host's OOM preferences (-1000 to 1000)
      --pid string                            PID namespace to use
      --pids-limit int                        Tune container pids limit (set -1 for unlimited)
      --privileged                            Give extended privileges to this container
  -p, --publish list                          Publish a container's port(s) to the host (default [])
  -P, --publish-all                           Publish all exposed ports to random ports
      --read-only                             Mount the container's root filesystem as read only
      --restart string                        Restart policy to apply when a container exits (default "no")
      --rm                                    Automatically remove the container when it exits
      --runtime string                        Runtime to use for this container
      --security-opt list                     Security Options (default [])
      --shm-size string                       Size of /dev/shm, default value is 64MB
      --sig-proxy                             Proxy received signals to the process (default true)
      --stop-signal string                    Signal to stop a container, SIGTERM by default (default "SIGTERM")
      --stop-timeout int                      Timeout (in seconds) to stop a container
      --storage-opt list                      Storage driver options for the container (default [])
      --sysctl map                            Sysctl options (default map[])
      --tmpfs list                            Mount a tmpfs directory (default [])
  -t, --tty                                   Allocate a pseudo-TTY
      --ulimit ulimit                         Ulimit options (default [])
  -u, --user string                           Username or UID (format: <name|uid>[:<group|gid>])
      --userns string                         User namespace to use
      --uts string                            UTS namespace to use
  -v, --volume list                           Bind mount a volume (default [])
      --volume-driver string                  Optional volume driver for the container
      --volumes-from list                     Mount volumes from the specified container(s) (default [])
  -w, --workdir string                        Working directory inside the container
  
例子：  
docker run -it \
--network host  \
-u buildfarm \
-w /home/buildfarm \
-h bf-01 \
-v /home/mamh/code/sdm845:/home/buildfarm/jenkins/workspace/test \
-v /tmp:/tmp \
-v /dev:/dev \
-v /home/mirror:/home/mirror \
-v /home/qcom:/home/qcom \
bs-ubuntu-14.04 /bin/bash

````






















