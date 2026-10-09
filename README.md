# Part 2 Docker Containers with Commands
## Step 1 Using the docker image from Part 1

Download the same image uploaded to *Docker Hub* in part1 of this assignment

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker images
                                                                                                                                        i Info →   U  In Use
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
first-app:1.0        b6a47e1a7409        328MB         80.5MB
hello-world:latest   5e2309035332       25.9kB         9.49kB
mysql:latest         9d48c42f8341        1.3GB          290MB
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker pull highspeedali/dicebootcamp26:assign1
assign1: Pulling from highspeedali/dicebootcamp26
8dae1be95700: Pull complete
3f9b7c49588c: Pull complete
4f33f1505b05: Pull complete
2afde8c6dbc3: Pull complete
fc222269c5cf: Pull complete
44136fa355b3: Already exists
72b3023ef6e3: Download complete
Digest: sha256:b466f1bc7614737aba3d0a6e4202d369df9755a48d9417a1521c9cdb0ab44985
Status: Downloaded newer image for highspeedali/dicebootcamp26:assign1
docker.io/highspeedali/dicebootcamp26:assign1
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker images
                                                                                                                                        i Info →   U  In Use
IMAGE                                 ID             DISK USAGE   CONTENT SIZE   EXTRA
first-app:1.0                         b6a47e1a7409        328MB         80.5MB
hello-world:latest                    5e2309035332       25.9kB         9.49kB
highspeedali/dicebootcamp26:assign1   b466f1bc7614        585MB          142MB
mysql:latest                          9d48c42f8341        1.3GB          290MB
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

## Step 2 Run the Docker Container

Use `docker run -p 80:8080 highspeedali/dicebootcamp26:assign1` to run container
- `docker run` → starts a new container from the specified image.
- `-d` → runs the container in detached mode (background).
- `-p 80:8080` → maps host port 80 to container port 8080, so external traffic on port 80 reaches the app inside.
- `highspeedali/dicebootcamp26:assign1` → the image name and tag being used to launch the container.

This way, FastAPI app inside the container is accessible via `http://localhost:80` while it listens internally on port 8080.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker run -d -p 80:8080 highspeedali/dicebootcamp26:assign1
947aebbc2a608d58fdf984bfcddc40eb54a2eb13749296821f90271180406134
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

`docker ps` command displays the running container.
We can use `curl localhost` command to confirm if our webserver is responding to client requests

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker ps
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS          PORTS                                     NAMES
947aebbc2a60   highspeedali/dicebootcamp26:assign1   "uv run main.py --po…"   34 seconds ago   Up 33 seconds   0.0.0.0:80->8080/tcp, [::]:80->8080/tcp   blissful_mcclintock
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ curl localhost
["Hello from assignment1-part1!"]
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

## Step 3 Use Different Docker Container Commands

### `docker ps` command to list all running containers

- **Lists running containers** → shows all containers currently active on your system.
- **Displays details** → includes container ID, image name, command, status, ports, and names.
- **Quick monitoring** → helps you check which containers are up, what ports they’re using, and their runtime state. 

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker ps
CONTAINER ID   IMAGE                                 COMMAND                  CREATED         STATUS         PORTS                                     NAMES
947aebbc2a60   highspeedali/dicebootcamp26:assign1   "uv run main.py --po…"   5 minutes ago   Up 5 minutes   0.0.0.0:80->8080/tcp, [::]:80->8080/tcp   blissful_mcclintock
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

### `docker stop` command to stop a running container

- **Stops a running container** → gracefully shuts down the container process.
- **Usage** → `docker stop <container_id_or_name>` where you specify the container’s ID or name from `docker ps`.
- **Effect** → the container halts but isn’t removed, so you can restart it later with `docker start`.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker stop blissful_mcclintock
blissful_mcclintock
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

### `docker rm` command to remove a stopped container

- **Removes stopped containers** → deletes containers that are no longer running.
- **Usage** → `docker rm <container_id_or_name>` to specify which container to remove.
- **Effect** → frees up space and cleans up old containers, but doesn’t touch the image itself.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker rm blissful_mcclintock
blissful_mcclintock
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

### `docker logs` command to view the logs of a container

- **Shows container output** → displays the standard output (stdout) and error (stderr) streams from a container.    
- **Usage** → `docker logs <container_id_or_name>` to view logs for a specific container.
- **Helpful for debugging** → lets you see application messages, errors, or print statements inside the container.
- **Options** → add `-f` to follow logs in real time, or `--tail N` to show the last _N_ lines.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker run -d -p 80:8080 highspeedali/dicebootcamp26:assign1
739bb30b0736ef772342e06a80fe8d904dd73e7bf2f01cda3b268d1b60e328aa
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker ps
CONTAINER ID   IMAGE                                 COMMAND                  CREATED         STATUS         PORTS                                     NAMES
739bb30b0736   highspeedali/dicebootcamp26:assign1   "uv run main.py --po…"   4 seconds ago   Up 4 seconds   0.0.0.0:80->8080/tcp, [::]:80->8080/tcp   great_bhaskara
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker logs great_bhaskara
warning: Ignoring existing virtual environment linked to non-existent Python interpreter: `.venv/bin/python` -> `/home/ubuntu/.local/share/uv/python/cpython-3.14-linux-x86_64-gnu/bin/python3.14`
Downloading cpython-3.14.8-linux-x86_64-gnu (download) (34.6MiB)
 Downloaded cpython-3.14.8-linux-x86_64-gnu (download)
Using CPython 3.14.8
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Downloading unicorn (15.7MiB)
Downloading pydantic-core (2.1MiB)
 Downloaded pydantic-core
 Downloaded unicorn
Installed 15 packages in 16ms
INFO:     Started server process [37]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:8080 (Press CTRL+C to quit)
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

### `docker inspect` command to view the details of a container

- **Displays detailed information** → shows low‑level metadata about a container or image.
- **Usage** → `docker inspect <container_id_or_name>` or `docker inspect <image_name>`.
- **Output format** → JSON with fields like configuration, networking, mounts, environment variables, and more.
- **Helpful for debugging** → lets you check exact settings, IP addresses, volumes, and runtime details.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker inspect great_bhaskara
[
    {
        "Id": "739bb30b0736ef772342e06a80fe8d904dd73e7bf2f01cda3b268d1b60e328aa",
        "Created": "2026-10-09T23:14:37.083092782Z",
        "Path": "uv",
        "Args": [
            "run",
            "main.py",
            "--port",
            "8080",
            "--host",
            "0.0.0.0"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 5604,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2026-10-09T23:14:37.133533715Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:b466f1bc7614737aba3d0a6e4202d369df9755a48d9417a1521c9cdb0ab44985",
        "ResolvConfPath": "/var/lib/docker/containers/739bb30b0736ef772342e06a80fe8d904dd73e7bf2f01cda3b268d1b60e328aa/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/739bb30b0736ef772342e06a80fe8d904dd73e7bf2f01cda3b268d1b60e328aa/hostname",
        "HostsPath": "/var/lib/docker/containers/739bb30b0736ef772342e06a80fe8d904dd73e7bf2f01cda3b268d1b60e328aa/hosts",
        "LogPath": "/var/lib/docker/containers/739bb30b0736ef772342e06a80fe8d904dd73e7bf2f01cda3b268d1b60e328aa/739bb30b0736ef772342e06a80fe8d904dd73e7bf2f01cda3b268d1b60e328aa-json.log",
        "Name": "/great_bhaskara",
        "RestartCount": 0,
        "Driver": "overlayfs",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "docker-default",
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
                "8080/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "80"
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
                42,
                156
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "private",
            "Dns": null,
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
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/acpi",
                "/proc/asound",
                "/proc/interrupts",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/sys/devices/virtual/powercap",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "Storage": {
            "RootFS": {
                "Snapshot": {
                    "Name": "overlayfs"
                }
            }
        },
        "Mounts": [],
        "Config": {
            "Hostname": "739bb30b0736",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "8080/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "LANG=C.UTF-8",
                "GPG_KEY=7169605F62C751356D054A26A821E680E5FA6305",
                "PYTHON_VERSION=3.12.15",
                "PYTHON_SHA256=c2c4321961fab0fb999d66e0cecf521c2ab3994c7992873ea99e306c1094fd5a"
            ],
            "Cmd": [
                "uv",
                "run",
                "main.py",
                "--port",
                "8080",
                "--host",
                "0.0.0.0"
            ],
            "Image": "highspeedali/dicebootcamp26:assign1",
            "Volumes": null,
            "WorkingDir": "/app",
            "Entrypoint": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "SandboxID": "c716e3939c68191122d557fd6b6a86dd6e7f84e1896c868c8ee02973cb20bb1b",
            "SandboxKey": "/var/run/docker/netns/c716e3939c68",
            "Ports": {
                "8080/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "80"
                    },
                    {
                        "HostIp": "::",
                        "HostPort": "80"
                    }
                ]
            },
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "DriverOpts": null,
                    "GwPriority": 0,
                    "NetworkID": "202f1cad0d22aefd621c2edf4c3b481be40eec9fedd361a31280e811067ede9a",
                    "EndpointID": "fd5f0fed02705c94a1edff8c8b12858ba1ce29c1f5542eb9d8b55d1c26b97c57",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "MacAddress": "c2:90:55:8f:10:34",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "DNSNames": null
                }
            }
        },
        "ImageManifestDescriptor": {
            "mediaType": "application/vnd.oci.image.manifest.v1+json",
            "digest": "sha256:1770bc68f7f0ca3478d6e66e545784eb56c62136e6d248804af670c4a73b4ccf",
            "size": 2011,
            "platform": {
                "architecture": "amd64",
                "os": "linux"
            }
        }
    }
]
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

### `docker exec` command to execute a command inside a running container

- **Runs a command inside a running container** → lets you interact with the container without stopping it.
- **Usage** → `docker exec <container_id_or_name> <command>` (e.g., `docker exec my_container ls /app`).
- **Interactive mode** → add `-it` to open a shell inside the container, like:

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker exec great_bhaskara ls /app
Dockerfile
README.md
main.py
pyproject.toml
uv.lock
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

### `docker attach` command to attach to a running container

- **Connects to a running container’s main process** → lets you see its live output and interact with it directly.
- **Usage** → `docker attach <container_id_or_name>`.
- **Effect** → your terminal becomes linked to the container’s console, showing logs and accepting input.
- **Difference from** `exec` → `attach` connects to the container’s existing process, while `exec` starts a new command inside it.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker attach great_bhaskara


```

### `docker commit` command to create a new image from a container

- **Creates a new image from a container** → saves the current state of a running or stopped container as a reusable image.
- **Usage** → `docker commit <container_id_or_name> <new_image_name:tag>`.
- **Effect** → captures any changes made inside the container (installed packages, modified files, configurations) into a new image.
- **Helpful for snapshots** → lets you preserve a customized container setup without rebuilding from scratch.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker ps
CONTAINER ID   IMAGE                                 COMMAND                  CREATED         STATUS              PORTS                                     NAMES
739bb30b0736   highspeedali/dicebootcamp26:assign1   "uv run main.py --po…"   8 minutes ago   Up About a minute   0.0.0.0:80->8080/tcp, [::]:80->8080/tcp   great_bhaskara
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker commit  great_bhaskara  great_bhaskara:snapshot
sha256:b1617cb4f154214c30bf5c47e66bab5d13737db8c86a4a93f92a57b09bc0b544
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker images
                                                                                                                                        i Info →   U  In Use
IMAGE                                 ID             DISK USAGE   CONTENT SIZE   EXTRA
first-app:1.0                         b6a47e1a7409        328MB         80.5MB
great_bhaskara:snapshot               b1617cb4f154        838MB          203MB
hello-world:latest                    5e2309035332       25.9kB         9.49kB
highspeedali/dicebootcamp26:assign1   b466f1bc7614        585MB          142MB    U
mysql:latest                          9d48c42f8341        1.3GB          290MB
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

### `docker cp` command to copy files/folders between the container and the host

- **Purpose** → copies files or directories between your host machine and a container.
- **Usage** 
    - From host → container: `docker cp ./local_file.txt my_container:/app/`
    - From container → host: `docker cp my_container:/app/logs.txt ./logs.txt`      
- **Effect** → makes it easy to transfer configs, code, or logs without rebuilding the image or restarting the container.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ touch testfile
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ cat testfile
Test file
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker cp testfile  great_bhaskara:/app/
Successfully copied 10B (transferred 2.05kB) to great_bhaskara:/app/
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker exec great_bhaskara cat /app/testfile
Test file
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

### `docker stats` command to view the resource usage of containers

- **Live resource monitoring** → streams CPU, memory, network I/O, and block I/O usage for all running containers.
- **Interactive dashboard** → updates continuously until you stop it with `Ctrl+C`.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker stats  great_bhaskara
                                                                            
CONTAINER ID   NAME             CPU %     MEM USAGE / LIMIT     MEM %     NET I/O         BLOCK I/O     PIDS
739bb30b0736   great_bhaskara   0.34%     38.91MiB / 3.785GiB   1.00%     1.16kB / 126B   0B / 8.19kB   3

```

### `docker top` command to view the running processes inside a container

- **Shows running processes inside a container** → similar to the `top` command on Linux, but scoped to one container.
- **Output** → lists process IDs (PIDs), user, CPU/memory usage, and the command each process is running.
- **Helpful for debugging** → lets you see what’s actually executing inside the container at that moment.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker top great_bhaskara
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                5907                5882                0                   23:21               ?                   00:00:00            uv run main.py --port 8080 --host 0.0.0.0
root                5970                5907                0                   23:21               ?                   00:00:02            /app/.venv/bin/python main.py --port 8080 --host 0.0.0.0
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

### `docker start` command to start a stopped container

- **Starts an existing container** → brings a stopped container back to life.
- **Effect** → resumes the container with its original configuration, without creating a new one.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker stop great_bhaskara
great_bhaskara
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker start great_bhaskara
great_bhaskara
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker ps
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS         PORTS                                     NAMES
739bb30b0736   highspeedali/dicebootcamp26:assign1   "uv run main.py --po…"   16 minutes ago   Up 2 seconds   0.0.0.0:80->8080/tcp, [::]:80->8080/tcp   great_bhaskara
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

### `docker pause` command to pause a running container

- **Suspends all processes in a container** → it freezes the container by sending a `SIGSTOP` signal to every process inside.
- **Effect** → the container remains running but inactive — CPU and execution stop, though memory and state are preserved.
- **Helpful for resource management** → lets you temporarily halt a container without stopping or killing it.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker pause great_bhaskara
great_bhaskara
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker ps
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS                   PORTS                                     NAMES
739bb30b0736   highspeedali/dicebootcamp26:assign1   "uv run main.py --po…"   17 minutes ago   Up 41 seconds (Paused)   0.0.0.0:80->8080/tcp, [::]:80->8080/tcp   great_bhaskara
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ curl localhost
^C
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

### `docker unpause` command to unpause a paused container

- **Resumes all processes in a paused container** → it reverses the effect of `docker pause`, allowing the container to continue running normally.
- **Effect** → the container’s processes pick up right where they left off, without losing state or data.
- **Helpful for resource control** → lets you temporarily freeze a container (`pause`) and then bring it back (`unpause`) when you’re ready.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker unpause great_bhaskara
great_bhaskara
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ curl localhost
["Hello from assignment1-part1!"]
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

### `docker rename` command to rename a container

- **Renames an existing container** → changes the container’s name to something new.
- **Usage** → `docker rename <old_name> <new_name>`.
- **Effect** → makes it easier to identify containers with meaningful names instead of random IDs.
- **Note** → this only changes the container’s name, not the image or its contents.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker ps
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS         PORTS                                     NAMES
739bb30b0736   highspeedali/dicebootcamp26:assign1   "uv run main.py --po…"   19 minutes ago   Up 3 minutes   0.0.0.0:80->8080/tcp, [::]:80->8080/tcp   great_bhaskara
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker rename great_bhaskara dicebootcamp26
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker ps
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS         PORTS                                     NAMES
739bb30b0736   highspeedali/dicebootcamp26:assign1   "uv run main.py --po…"   20 minutes ago   Up 3 minutes   0.0.0.0:80->8080/tcp, [::]:80->8080/tcp   dicebootcamp26
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

### `docker wait` command to wait for a container to exit and then display its exit code

- **Blocks until a container stops** → it waits for the specified container to exit.    
- **Usage** → `docker wait <container_id_or_name>`.
- **Returns exit code** → once the container finishes, it outputs the container’s exit status (useful for scripts or automation).
- **Helpful for orchestration** → lets you synchronize actions, ensuring one container finishes before starting another task.
- In below example, stopping a running container after executing `docker wait` command gave 143 exit code which indicates **a successful, graceful termination of the container**

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker wait dicebootcamp26

143
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

### `docker port` command to display the public-facing port that a container is listening on

- **Purpose** → shows how container ports are mapped to host machine ports.
- **Output** → displays mappings in the format `<container_port>/<protocol> -> <host_ip>:<host_port>`.
- **Helpful for access** → lets you know which host port to use when connecting to services running inside the container.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker port dicebootcamp26
8080/tcp -> 0.0.0.0:80
8080/tcp -> [::]:80
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

### `docker update` command to update a container's resource limits

- **Purpose** → modifies resource limits and restart policies of an already running container, without recreating it.
- **Common options**:
    - `--cpus` → set CPU quota.
    - `--memory` → set memory limit.
    - `--memory-swap` → set combined memory + swap limit.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker update --memory=512m --memory-swap=512m  --cpus=1 dicebootcamp26
dicebootcamp26
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```

### `docker restart` command to restart a running container

- **Purpose** → stops and then starts a container in one step.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker restart dicebootcamp26
dicebootcamp26
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$ docker ps
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS         PORTS                                     NAMES
739bb30b0736   highspeedali/dicebootcamp26:assign1   "uv run main.py --po…"   28 minutes ago   Up 3 seconds   0.0.0.0:80->8080/tcp, [::]:80->8080/tcp   dicebootcamp26
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part2$
```
