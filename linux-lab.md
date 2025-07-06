### Step 1: Start the privileged "inspector" container simulating the Docker Linux VM

```bash
docker run --rm -it --privileged --pid=host --cgroupns=host -v /:/mnt ubuntu chroot /mnt bash
```

- The inspector container is a specially started Docker container designed to help you inspect and interact with the host system and other containers at a deeper level.

- This container effectively acts as a bridge allowing you to peek into and manage the host’s process namespaces, cgroups, and filesystems as if you were on the host itself.

- `--pid=host` = This container will see and interact with the processes of the host machine, not just its own containerized ones. With this flag, `ps` or `/proc` shows the host system processes.

- `--privileged` = Gives the container all Linux capabilities (normally restricted) giving it extended Linux capabilities to access system resources.

- `--cgroupns=host` = Allows you to inspect and control other containers' resource usage and see how host processes (including other containers) are grouped.

- `-v /:/mnt` = Mounts the entire host filesystem (`/`) into the container under `/mnt`. Maps the host's entire filesystem into the container.

- `chroot /mnt bash` = Switch the container's root filesystem to the host filesystem and launch a bash shell inside this new root.

### Step 2: Run test containers 

```bash
docker run -dit --name demo-test ubuntu sleep 800
```

- This container simulates an isolated workload.

- `-dit` = Runs a container in the background, while still allowing interaction via the terminal if required.

### Step 3: Find container process ID (PID)

- Run `docker inspect --format "Process ID (PID): {{.State.Pid}}, Container ID: {{.Id}}" demo-test`

```bash
Process ID (PID): 12345, Container ID: <example123456789>
```

- This command uses docker inspect to fetch metadata about the container.

- `{{.State.Pid}}` returns the PID of the container's init process as seen by the host — this is the actual Linux process running on the host that represents the container.

- `{{.Id}}` returns the container’s full ID for reference.

- Knowing this PID allows you to explore the container’s namespaces, cgroups, and interact with it directly via tools like nsenter.

### Step 4: Inspect the container process namespaces and control groups

- Use `cat /proc/<PID>/cgroup` to see the control groups (cgroups) that the process belongs to.  
  ```bash
  0::/docker/<CONTAINER ID>
  ```
- This shows the container’s unique cgroup path, which helps isolate and manage resource limits for that container.

- The directory `/proc/<PID>` represents the process with that PID on the host system.

- Inside `/proc/<PID>`, you can find detailed information about the process, including:

  - Its namespaces in `/proc/<PID>/ns/`, which are symbolic links representing the kernel namespaces the process belongs to (like `pid`, `net`, `mnt`, etc).

- Use `ls -l /proc/<PID>/ns/` to list these namespaces. Processes inside the same container share the same namespace IDs, isolating them from other containers and the host.

This step helps you understand how Linux namespaces and cgroups work together to isolate container processes from the rest of the system.


### Step 5: Observe the Container Process from Host and Inside the Container

This step helps compare how a container process appears from the host system versus inside the container itself.

- From the **inspector container**, you can see all processes on the host, including those started by containers.

- Run the following to locate the container process (e.g. running `sleep`):

- Run `ps -ef | grep sleep`
  ```bash
  root      12345  1382  0 20:02 ?        00:00:00 sleep 1000
  ```
  
- 12345 is the process ID of the container’s sleep command from the host’s perspective.

- Now, use `nsenter` to enter that process’s namespace context:
- Run `nsenter --target 1404 --pid --mount --uts --ipc --net bash`
- Then run `ps -ef` to list all running processes on the current system:
  ```bash
  UID        PID  PPID  C STIME TTY          TIME CMD
  root         1     0  0 20:02 pts/0    00:00:00 sleep 1000
  root         7     0  0 20:06 pts/0    00:00:00 bash
  root        10     7  0 20:06 pts/0    00:00:00 ps -ef

- This illustrates namespace isolation: while the process ID on the host is 12345, inside the container it appears as PID 1.