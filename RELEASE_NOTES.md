# Release Notes

## 1.1.2
### Bugfixes
- Fixed a bug where the `podman image list`, `podman image rm`, and `podman container list` had broken global storage options
- Fixed a bug where the `--label` option to `podman create` and `podman run` was missing the `-l` alias
- Fixed a bug where running Podman with the `--config` flag would not set an appropriate default value for `tmp_dir` ([#2408](https://github.com/containers/libpod/issues/2408))
- Fixed a bug where the `podman logs` command with the `--timestamps` flag produced unreadable output ([#2500](https://github.com/containers/libpod/issues/2500))
- Fixed a bug where the `podman cp` command would automatically extract `.tar` files copied into the container ([#2509](https://github.com/containers/libpod/issues/2509))

### Misc
- The `podman container stop` command is now usable with the Podman remote client

## 1.1.1
### Bugfixes
- Fixed a bug where `podman container restore` was erroneously available as `podman restore` ([#2191](https://github.com/containers/libpod/issues/2191))
- Fixed a bug where the `volume_path` option in `libpod.conf` was not being respected
- Fixed a bug where Podman failed to build when the `varlink` tag was not present ([#2459](https://github.com/containers/libpod/issues/2459))
- Fixed a bug where the `podman image load` command was listed twice in help text
- Fixed a bug where the `podman image sign` command was also listed as `podman sign`
- Fixed a bug where the `podman image list` command incorrectly had an `image` alias
- Fixed a bug where the `podman images` command incorrectly had `ls` and `list` aliases
- Fixed a bug where the `podman image rm` command was being displayed as `podman image rmi`
- Fixed a bug where the `podman create` command would attempt to parse arguments meant for the container
- Fixed a bug where the combination of FIPS mode and user namespaces resulted in permissions errors
- Fixed a bug where the `--time` alias for `--timeout` for the `podman restart` and `podman stop` commands did not function
- Fixed a bug where the default stop timeout for newly-created containers was being set to 0 seconds (resulting in an immediate SIGKILL on running `podman stop`)
- Fixed a bug where the output format of `podman port` was incorrect, printing full container ID instead of truncated ID
- Fixed a bug where the `podman container list` command did not exist
- Fixed a bug where `podman build` could not build a container from images tagged locally that did not exist in a registry ([#2469](https://github.com/containers/libpod/issues/2469))
- Fixed a bug where some Podman commands that accept no arguments would not error when provided arguments
- Fixed a bug where `podman play kube` could not handle cases where a pod and a container shared a name

### Misc
- Usage text for many commands was greatly improved
- Major cleanups were made to Podman manpages, ensuring that command lists are accurate
- Greatly improved debugging output when the `newuidmap` and `newgidmap` binaries fail when using rootless Podman
- The `-s` alias for the global `--storage-driver` option has been removed
- The `podman container refresh` command has been deprecated, as its intended use case is no longer relevant. The command has been hidden and manpages deleted. It will be removed in a future release
- The `podman container runlabel` command will now pull images not available locally even without the `--pull` option. The `--pull` option has been deprecated
- The `podman container checkpoint` and `podman container restore` commands are now only available on OCI runtimes where they are supported (e.g. `runc`)

## 1.1.0
### Features
- Added `--latest` and `--all` flags to `podman mount` and `podman umount`
- Rootless Podman can now forward ports into containers (using the same `-p` and `-P` flags as root Podman)
- Rootless Podman will now pull some configuration options (for example, OCI runtime path) from the default root `libpod.conf` if they are not explicitly set in the user's own `libpod.conf` ([#2174](https://github.com/containers/libpod/issues/2174))
- Added an alias `-f` for the `--format` flag of the `podman info` and `podman version` commands
- Added an alias `-s` for the `--size` flag of the `podman inspect` command
- Added the `podman system info` and `podman system prune` commands
- Added the `podman cp` command to copy files between containers and the host ([#613](https://github.com/containers/libpod/issues/613))
- Added the `--password-stdin` flag to `podman login`
- Added the `--all-tags` flag to `podman pull`
- The `--rm` and `--detach` flags can now be used together with `podman run`
- The `podman start` and `podman run` commands for containers in pods will now start dependency containers if they are stopped
- Added the `podman system renumber` command to handle lock changes
- The `--net=host` and `--dns` flags for `podman run` and `podman create` no longer conflict
- Podman now handles mounting the shared /etc/resolv.conf from network namespaces created by `ip netns add` when they are passed in via `podman run --net=ns:`

### Bugfixes
- Fixed a bug with `podman inspect` where different information would be returned when the container was running versus when it was stopped
- Fixed a bug where errors in Go templates passed to `podman inspect` were silently ignored instead of reported to the user ([#2159](https://github.com/containers/libpod/issues/2159))
- Fixed a bug where rootless Podman with `--pid=host` containers was incorrectly masking paths in `/proc`
- Fixed a bug where full errors starting rootless `Podman` were not reported when a refresh was requested
- Fixed a bug where Podman would override the config file-specified storage driver with the driver the backing database was created with without warning users
- Fixed a bug where `podman prune` would prune all images not in use by a container, as opposed to only untagged images, by default ([#2192](https://github.com/containers/libpod/issues/2192))
- Fixed a bug where `podman create --quiet` and `podman run --quiet` were not properly suppressing output
- Fixed a bug where the `table` keyword in Go template output of `podman ps` was not working ([#2221](https://github.com/containers/libpod/issues/2221))
- Fixed a bug where `podman inspect` on images pulled by digest would double-print `@sha256` in output when printing digests ([#2086](https://github.com/containers/libpod/issues/2086))
- Fixed a bug where `podman container runlabel` will return a non-0 exit code if the label does not exist
- Fixed a bug where container state was always reset to Created after a reboot ([#1703](https://github.com/containers/libpod/issues/1703))
- Fixed a bug where `/dev/pts` was unconditionally overridden in rootless Podman, which was unnecessary except in very specific cases
- Fixed a bug where Podman run as root was ignoring some options in `/etc/containers/storage.conf` ([#2217](https://github.com/containers/libpod/issues/2217))
- Fixed a bug where Podman cleanup processes were not being given the proper OCI runtime path if a custom one was specified
- Fixed a bug where `podman images --filter dangling=true` would crash if no dangling images were present ([#2246](https://github.com/containers/libpod/issues/2246))
- Fixed a bug where `podman ps --format "{{.Mounts}}"` would not display a container's mounts ([#2238](https://github.com/containers/libpod/issues/2238))
- Fixed a bug where `podman pod stats` was ignoring Go templates specified by `--format` ([#2258](https://github.com/containers/libpod/issues/2258))
- Fixed a bug where `podman generate kube` would fail on containers with `--user` specified ([#2304](https://github.com/containers/libpod/issues/2304))
- Fixed a bug where `podman images` displayed incorrect output for images pulled by digest ([#2175](https://github.com/containers/libpod/issues/2175))
- Fixed a bug where `podman port` and `podman ps` did not properly display ports if the container joined a network namespace from a pod or another container ([#846](https://github.com/containers/libpod/issues/846))
- Fixed a bug where detaching from a container using the detach keys would cause Podman to hang until the container exited
- Fixed a bug where `podman create --rm` did not work with `podman start --attach`
- Fixed a bug where invalid named volumes specified in `podman create` and `podman run` could cause segfaults ([#2301](https://github.com/containers/libpod/issues/2301))
- Fixed a bug where the `runtime` field in `libpod.conf` was being ignored. `runtime` is legacy and deprecated, but will continue to be respected for the forseeable future
- Fixed a bug where `podman login` would sometimes report it logged in successfully when it did not
- Fixed a bug where `podman pod create` would not error on receiving unused CLI argument
- Fixed a bug where rootless `podman run` with the `--pod` argument would fail if the pod was stopped
- Fixed a bug where `podman images` did not print a trailing newline when not invoked on a TTY ([#2388](https://github.com/containers/libpod/issues/2388))
- Fixed a bug where the `--runtime` option was sometimes not overriding `libpod.conf`
- Fixed a bug where `podman pull` and `podman runlabel` would sometimes exit with 0 when they should have exited with an error ([#2405](https://github.com/containers/libpod/issues/2405))
- Fixed a bug where rootless `podman export -o` would fail ([#2381](https://github.com/containers/libpod/issues/2381))
- Fixed a bug where read-only volumes would fail in rootless Podman when the volume originated on a filesystem mounted `nosuid`, `nodev`, or `noexec` ([#2312](https://github.com/containers/libpod/issues/2312))
- Fixed a bug where some files used by checkpoint and restore received improper SELinux labels ([#2334](https://github.com/containers/libpod/issues/2334))
- Fixed a bug where Podman's volume path was not properly changed when containers/storage changed location ([#2395](https://github.com/containers/libpod/issues/2395))

### Misc
- Podman migrated to a new, shared memory locking model in this release. As part of this, if you are running Podman with pods or dependency containers (e.g. `--net=container:`), you should run the `podman system renumber` command to migrate your containers to the new model - please reference the `podman-system-renumber(1)` man page for further details
- Podman migrated to a new command-line parsing library, and the output format of help and usage text has somewhat changed as a result
- Updated Buildah to v1.7, picking up a number of bugfixes
- Updated containers/image library to v1.5, picking up a number of bugfixes and performance improvements to pushing images
- Updated containers/storage library to v1.10, picking up a number of bugfixes
- Work on the remote Podman client for interacting with Podman remotely over Varlink is progressing steadily, and many image and pod commands are supported - please see the [Readme](https://github.com/containers/libpod/blob/master/remote_client.md) for details
- Added path masking to mounts with the `:z` and `:Z` options, preventing users from accidentally performing an SELinux relabel of their entire home directory
- The `podman container runlabel` command will not pull an image if it does not contain the requested label
- Many commands' usage information now includes examples
- `podman rm` can now delete containers in containers/storage, which can be used to resolve some situations where Podman fails to remove a container
- The `podman search` command now searches multiple registries in parallel for improved performance
- The `podman build` command now defaults `--pull-always` to true
- Containers which share a network namespace (for example, when in a pod) will now share /etc/hosts and /etc/resolv.conf between all containers in the pod, causing changes in one container to propogate to all containers sharing their networks
- The `podman rm` and `podman rmi` commands now return 1 (instead of 127) when all specified container or images are missing

## 1.0.0
### Features
- The `podman exec` command now includes a `--workdir` option to set working directory for the executed command
- The `podman create` and `podman run` commands now support the `--init` flag to use a minimal init process in the container
- Added the `podman image sign` command to GPG sign images
- The `podman run --device` flag now accepts directories, and will added any device nodes in the directory to the container
- Added the `podman play kube` command to create pods and containers from Kubernetes pod YAML

### Bugfixes
- Fixed a bug where passing `podman create` or `podman run` volumes with an empty host or container path could cause a segfault
- Fixed a bug where `storage.conf` was sometimes ignored for rootless containers
- Fixed a bug where Podman run as root would error if CAP_SYS_RESOURCE was not available
- Fixed a bug where Podman would fail to start containers after a system restart due to an out-of-date default Apparmor profile
- Fixed a bug where Podman's bash completions were not working
- Fixed a bug where `podman login` would use existing login credentials even if new credentials were provided
- Fixed a bug where Podman could create some directories with the wrong permissions, breaking containers with user namespaces
- Fixed a bug where `podman runlabel` was not properly setting container names when the `--name` was specified
- Fixed a bug where `podman runlabel` sometimes included extra spaces in command output
- Fixed a bug where `podman commit` was including invalid port numbers in created images when committing containers with published ports
- Fixed a bug where `podman exec` was not honoring the container's environment variables
- Fixed a bug where `podman run --device` would fail when a symlink to a device was specified
- Fixed a bug where `podman build` was not properly picking up OCI runtime paths specified in `libpod.conf`
- Fixed a bug where Podman would mount `/dev/shm` into the container read-only for read-only containers (`/dev/shm` should always be read-write)
- Fixed a bug where Podman would ignore any mount whose container mountpoint was `/dev/shm`
- Fixed a bug where `podman export` did not work with the default `fuse-overlayfs` storage driver
- Fixed a bug where `podman inspect -f '{{ json .Config }}'` on images would not output anything (it now prints the image's config)
- Fixed a bug where `podman rmi -fa` displayed the wrong error message when trying to remove images used by pod infra containers

### Misc
- Rootless containers now unconditionally use postrun cleanup processes, ensuring resources are freed when the container stops
- A new version of Buildah is included for `podman build`, featuring improved build speed and numerous bugfixes
- Pulling images has been parallelized, allowing individual layers to be pulled in parallel
- The `podman start --attach` command now defaults the `sig-proxy` option to `true`, matching `podman create` and `podman run`
- The `podman info` command now prints the path of the configuration file controlling container storage
- Added `podman list` and `podman ls` as aliases for `podman ps`, and `podman container ps` and `podman container list` as aliases for `podman container ls`
- Changed `podman generate kube` to generate Kubernetes service YAML in the same file as pod YAML, generating a single file instead of two
- To improve compatability with the Docker command line, `podman inspect -f '{{ json .ContainerConfig }}'` on images is no longer valid; please use `podman inspect -f '{{ json .Config }}'` instead

## 0.12.1.2
### Bugfixes
- Fixed a bug where an empty path for named volumes could make it impossible to create containers
- Fixed a bug where containers using another container's network namespace would not also use the other container's /etc/hosts and /etc/resolv.conf
- Fixed a bug where containers with `--rm` which failed to start were not removed
- Fixed a potential race condition attempting to read `/etc/passwd` inside containers

## 0.12.1.1
### Features
- Added the `podman generate kube` command to generate Kubernetes Pod and Service YAML for Podman containers and pods
- The `podman pod stop` flag now accepts a `--timeout` flag to set the timeout for stopping containers in the pod

### Bugfixes
- Fixed a bug where rootless Podman would fail to start if the default OCI hooks directory is not present

## 0.12.1
### Features
- Rootless Podman now creates the storage.conf, libpod.conf, and mounts.conf configuration files automatically in `~/.config/containers/` for ease of reconfiguration
- The `podman pod create` command can expose ports in the pod's network namespace, allowing public services to be created in pods
- The `podman container checkpoint` command can now keep containers running after they are checkpointed with the `--leave-running` flag
- The `podman container checkpoint` and `podman container restore` commands now support the `--tcp-established` flag to checkpoint and restore containers with active TCP connections
- The `podman version` command now has a `--format` flag to produce machine-readable output
- Added the `podman container exists`, `podman pod exists`, and `podman image exists` commands to easily check for a container/pod/image, respectively, by name or ID
- The `podman ps --pod` flag now has a short alias, `-p`
- The `podman rmi` and `podman rm` commands now have a `--prune` flag to prune unused images and containers, respectively
- The `podman ps` command now has a `--sync` flag to force a sync of Podman's state against the OCI runtime, resolving some state desync errors
- Added the `podman volume` set of commands for creating and managing local-only named volumes

### Bugfixes
- Fixed a breaking change in rootless Podman where a change in default paths caused Podman to be unable to function on systems upgraded from 0.10.x or earlier
- Fixed a bug where `podman exec` without `-t` would still use a terminal if the container was created with `-t`
- Fixed a bug where container root propogation was not being properly adjusted if volumes with root propogation set were mounted into the container
- Fixed a bug where `podman exec` could hold the container lock longer than necessary waiting for an exited container
- Fixed a bug where rootless containers using `slirp4netns` for networking were reporting using `bridge` networking in `podman inspect`
- Fixed a bug where `podman container restore -a` was attempting to restore all containers, including created and running ones. It will now only attempt to restore stopped and exited containers
- Fixed a bug where rootless Podman detached containers were not being properly cleaned up
- Fixed a bug where privileged containers were being mounted with incorrect (too restrictive) mount options such as `nodev`
- Fixed a bug where `podman stop` would throw an error attempting to stop a container that had already stopped
- Fixed a bug where `NOTIFY_SOCKET` was not properly being passed into Podman containers
- Fixed a bug where `/dev/shm` was not properly mounted in rootless containers
- Fixed a bug where rootless Podman would set up the CNI plugins for networking (despite not using them in rootless mode), potentially causing `inotify` related errors
- Fixed a bug where Podman would error on numeric GIDs that do not exist in the container's `/etc/group`
- Fixed a bug where containers in pods or created with `--net=container` were not mounting `/etc/resolv.conf` and `/etc/hosts`

### Misc
- `podman build` now defaults the `--force-rm` flag to `true`
- Improved `podman runlabel` support for labels featuring arguments with whitespace
- Containers without a network namespace will now use the host's `resolv.conf`
- The `slirp4netns` network mode can now be used with containers running as root. It may be useful for container-in-container scenarios where the outer container does not have host networking set
- Podman now uses `inotify` to wait for container exit files to be created, instead of polling. If `inotify` cannot be used, Podman will fall back to polling to check if the file has been created
- The `podman logs` command now uses improved short-options handling, allowing its flags to be combined if desired (for example, `podman logs -lf` instead of `podman logs -l -f`)
- Hardcoded OCI hooks directories used by Podman are now deprecated; they should instead be coded into the `libpod.conf` configuration file. They can be specified as an array via `hooks_dir`

## 0.11.1.1
### Bugfixes
- Fixed a bug where Podman was not correctly adding firewall rules for containers, preventing them from accessing the network
- Fixed a bug where full error messages were being lost when creating containers with user namespaces
- Fixed a bug where container state was not properly updated if a failure occurred during network setup, which could cause mounts to be left behind when the container was removed
- Fixed a bug where `podman exec` could time out on slower systems by increasing the relevant timeout

### Misc
- `podman rm -f` now removes paused containers. As such, `podman rm -af` completing successfully guarantees all Podman containers have been removed
- Added a field to `podman info` to show if Podman is being run as rootless
- Made a small output format change to `podman images` - image sizes now feature a space between number and unit (e.g. `123 MB` now instead of `123MB`)
- Vendored an updated version of `containers/storage` to fix several bugs reported upstream

## 0.11.1
### Features
- Added `--all` and `--latest` flags to `podman checkpoint` and `podman restore`
- Added `--max-workers` flag to all Podman commands that support operating in parallel, allowing the maximum number of parallel workers used to be specified
- Added `--all` flag to `podman restart`

### Bugfixes
- Fixed a bug where `podman port -l` would segfault if no containers were present
- Fixed a bug where `podman stats -a` would error if containers were present but not running
- Fixed a bug where container status checks would sometimes leave zombie OCI runtime processes
- Fixed checkpoint and restore code to verify an appropriate version of `criu` is being used
- Fixed a bug where environment variables with no specified value (e.g. `-e FOO`) caused errors (they are now added as empty)
- Fixed a bug where rootless Podman would attempt to configure the system firewall, causing errors on some systems where iptables is not in the user's PATH
- Fixed a bug where rootless Podman was unable to successfully write the container ID to a file when `--cid-file` was specified to `podman run`
- Fixed a bug where `podman unmount` would refuse to unmount a container if it was running (the unmount will now be deferred until the container stops)
- Fixed a bug where rootless `podman attach` would fail to attach due to a too-long path name
- Fixed a bug where `podman info` was not properly reporting the Git commit Podman was built from
- Fixed a bug where `podman run --interactive` was not holding STDIN open when `-a` flag was specified
- Fixed a bug where Podman with the `cgroupfs` CGroup driver was sometimes not successfully removing pod CGroups
- Fixed a bug where rootless Podman was unable to run systemd containers (note that this also requires an update to systemd)
- Fixed a bug where `podman run` with the `--user` flag would fail if the container image did not contain `/etc/passwd` or `/etc/group`

### Misc
- `podman rm`, `podman restart`, `podman kill`, `podman pause`, and `podman unpause` now operate in parallel, greatly improving speed when multiple containers are specified
- `podman create`, `podman run`, and `podman ps` have a number of improvements which should greatly increase their speed
- Greatly improved performance and reduced memory utilization of container status checks, which should improve the speed of most Podman commands
- Improve ability of `podman runlabel` to run commands that are not Podman
- Podman containers with an IP address now add their hostnames to `/etc/hosts`
- Changed default location of temporary libpod files in rootless Podman
- Updated the default Podman seccomp profile

### Compatability
Several paths related to rootless Podman had their default values changed in this release.
If paths were not hardcoded in libpod.conf, your system may lose track of running containers and believe they are newly-created.

## 0.10.1.3
### Bugfixes
- Fixed a bug where `podman build` would not work while any containers were running

## 0.10.1.2
### Bugfixes
- Fixed cgroup mount for containers using systemd as init to work properly with the systemd cgroup manager

## 0.10.1.1
### Features
- Added handling for running containers as users with numeric UIDs not present in the container's /etc/passwd. This allows getpwuid() to work inside these containers.
- Added support for the REGISTRY_AUTH_FILE environment variable, which specifies the location of credentials for registry login. This is supported by the `push`, `pull`, `login`, `logout`, `runlabel`, and `search` commands

### Bugfixes
- Fixed handling for image volumes which are mounted on symlinks. The links are now resolved within the container, not on the host
- Fixed mounts for containers that use systemd as init to properly include all mounts required by systemd to function

### Misc
- Updated vendored version of Buildah used to power `podman build`

## 0.10.1
### Features
- Added the `podman container checkpoint` and `podman container restore` commands to checkpoint and restore containers
- Added the `podman container runlabel` command to run containers based on commands contained in their images
- Added the `podman create --ip` and `podman run --ip` flags to allow setting static IPs for containers
- Added the `podman kill --all` flag to send a signal to all running containers

### Bugfixes
- Fixed Podman cleanup processes for detached containers to properly print debug information when `--syslog` flag is specified
- Fixed manpages for `podman create` and `podman run` to document existing `--net` flag as an alias for `--network`
- Fixed issues with rootless Podman where specifying a single user mapping container was causing all Podman commands to hang
- Fixed an issue with rootless Podman not properly detecting when user namespaces were not enabled
- Fixed an issue where Podman user namespaces were not preserving file capabilities
- Fixed an issue where `resolv.conf` in container would unconditionally forward nameservers into the container, even localhost
- Fixed containers to release resources in the OCI runtime immediately after exiting, improving compatability with Kata containers
- Fixed OCI runtime handling to fix several issues when using gVisor as an OCI runtime
- Fixed SELinux relabel errors when starting containers after a system restart
- Fixed a crash when initializing hooks on containers running systemd as init
- Fixed an SELinux labelling issue with privileged containers
- Fixed rootless Podman to raise better errors when using CGroup resource limits, which are not currently compatible with rootless
- Fixed a crash when runc was used as the OCI runtime for containers running systemd as init
- Fixed SELinux labelling for containers run with `--security-opt label=disable` to assign the correct label

### Misc
- Changed flag ordering on all Podman commands to ensure flags are alphabetized
- Changed `podman stop` to work in parallel when multiple containers are specified, greatly speeding up stop for containers that do not stop after SIGINT
- Updated vendored version of Buildah used to power `podman build`
- Added version of vendored Buildah to `podman info` to better debug issues

## 0.9.3.1
### Bugfixes
- Fixed a critical issue where SELinux contexts set on tmpfs volumes were causing runc crashes

## 0.9.3
### Features
- Added a flag to `libpod.conf`, `label`, to globally enable/disable SELinux labelling for libpod
- Added `--mount` flag to `podman create` and `podman run` as a new, more explicit way of specifying volume mounts

### Bugfixes
- Fixed a crash during container creation when an image had no names
- Fixed default rootfs mount propagation to for containers to match Docker
- Fixed permissions of `/proc` in containers
- Fixed permissions of some default bind mounts (for example, `/etc/hosts`) in read-only containers
- Fixed `/dev/shm` in `--ipc=container` and `--ipc=host` containers to use the correct SHM
- Fixed rootless Podman to properly join the namespaces of other containers
- Fixed the output of `podman diff` to not display some default changes that will not be committed
- Fixed rootless to better handle cases where insufficient UIDs/GIDs are mapped into the container

## 0.9.2.1
### Bugfixes
- Updated Buildah dependency to fix several bugs in `podman build`

### Misc
- Small performance improvement in image handling code to not recalculate digests

## 0.9.2
### Features
- Added `--interval` flag to `podman wait` to determine the interval between checks for container status
- Added a switch in `libpod.conf` to disable reserving ports for running containers. This lowers the safety of port allocations, but can significantly reduce memory usage.
- Added ability to search all the contents of a registry if no image name is specified when using `podman search`

### Bugfixes
- Further fixes for sharing of UTS namespaces within pods
- Fixed a deadlock in containers/storage that could be caused by numerous parallel Podman processes.
- Fixed Podman running into open file limits when many ports are forwarded
- Fixed default mount propagation on volume mounts
- Fixed default mounts under /dev remaining if /dev is bind-mounted into the container
- Fixed rootless `podman create` with no command specified throwing an error

### Misc
- Added `podman rm --volumes` flag for compatability with Docker. As Podman does not presently support named volumes, this does nothing for now, but provides improved compatability with the Docker command line.
- Improved error messages from `podman pull`

### Compatability
- Podman is no longer being built by default with support for the Devicemapper storage driver. If you are using this storage driver, you should investigate switching to overlayfs.

## 0.9.1.1
### Bugfixes
- Added support for configuring iptables and firewalld firewalls to allow container traffic. This should resolve numerous issues with network access in containers.

### Note
It is recommended that you restart your system firewall after installing this release to clear any firewall rules created by older Podman versions. If port forwarding to containers does not work, it is recommended that you restart your system.

## 0.9.1
### Features
- Added initial support for the `podman pod` command as non-root

### Bugfixes
- Fixed regression where invalid Podman commands would still cause a clean exit
- Fixed `podman rmi --all` to not error if no images are present on the system
- Fixed parsing of container logs with `podman logs` to properly handle CRI logging, fixing some issues with blank lines in logs
- Fixed a bug creating pod cgroups using the systemd cgroup driver with systemd versions 239 and higher
- Fixed handling of volume mounts that overlapped with default container mounts (for example, `podman run -v /dev/:/dev`)
- Fixed sharing of UTS namespace in pods

### Misc
- Added additional debug information when pulling images if `--log-level=debug` is specified
- `podman build` now defaults to caching intermediate layers while building

## 0.8.5
### Features
- Added the ability to add a multipart entrypoint with `podman run --entrypoint`
- Improved help text when invalid commands are specified
- Greatly improved support for containers which use systemd as init

### Bugfixes
- Fixed several bugs with rootless `podman exec`
- Fixed rootless `podman` with a symlinked storage directory crashing
- Fixed bug with `podman ps` and multiple filters where the interface did not match Docker
- Fixed handling of `resolv.conf` on the host to handle symlinks
- Increased open file descriptor and process limits to match Docker and Buildah
- Fixed `podman run -h` to specify the container's hostname (as it does in Docker) instead of printing help text
- Fixed a bug with image shortname handling where repositories were incorrectly being treated as registries
- Fixed a bug where `podman wait` was busywaiting and consuming large amounts of CPU

## 0.8.4
### Features
- Added the `podman pod top` command
- Added the ability to easily share namespaces within a pod
- Added a pod statistics endpoint to the Varlink API
- Added information on container capabilities to the output of `podman inspect`

### Bugfixes
- Fixed a bug with the --device flag in `podman run` and `podman create`
- Fixed `podman pod stats` to accept partial pod IDs and pod names
- Fixed a bug with OCI hooks handling `ALWAYS` matches
- Fixed a bug with privileged rootless containers with `--net=host` set
- Fixed a bug where `podman exec --user` would not work with usernames, only numeric IDs
- Fixed a bug where Podman was forwarding both TCP and UDP ports to containers when protocol was not specified
- Fixed issues with Apparmor in rootless containers
- Fixed an issue with database encoding causing some containers created by Podman versions 0.8.1 and below to be unusable.

### Compatability:
We switched JSON encoding/decoding to a new library for this release to address a compatability issue introduced by v0.8.2.
However, this may cause issues with containers created in 0.8.2 and 0.8.3 with custom DNS servers.
