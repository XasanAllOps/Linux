## 𝗪𝗵𝗮𝘁 𝗮𝗿𝗲 𝗡𝗮𝗺𝗲𝘀𝗽𝗮𝗰𝗲𝘀?

Namespaces in Linux isolate what a process can see — think of them as blinders for processes, preventing them from seeing or affecting the rest of the system.

A simple example to help drive the point home: Imagine a hotel with many rooms. Each room (namespace) has:

 • Its own bathroom  
 • Its own Wi-Fi  
 • Its own keycard  

Even though all the rooms are under the same roof (the host machine), the guests (processes) in Room 101 have no visibility or access to what’s happening in Room 102.

So how does Docker (🐳) come into this equation?

Docker uses a technology called `namespaces` to provide the isoalted workspace called the container. When you run a container, Docker creates a set of namespaces for that container - Docker Docs.

Docker Engine uses **namespaces** extensively on Linux to isolate containers. Key types include:

 • `pid`: Process IDs – Each container sees only its own process list (ps aux, ps -ef)  
 • `net`: Network – Isolated interfaces, ports, and IPs  
 • `mnt`: Mount – Separate file systems & mountpoints  
 • `uts`: UTS – Containers can set their own hostname and domain name  
 • `ipc`: Interprocess Communication – Message queues, semaphores, and signals are isolated  

This is how Docker ensures containers are lightweight, secure, and independent.

## What are Control Groups?

cgroups control "What a process can use."
Think of them as resource managers that decide how much CPU, MEM, etc. a process can consume.
Let's use the example of the hotel analogy. In the same hotel, you can decide:

1. How much electricity each room can use ⚡️
2. How much water they get 💧
3. Whether they can access the gym 💪🏽

Therefore, a guest can't take over all the hotel's resources – same as how a container can't use all of the host's memory.

What cgroups can do:

1. `cpu` => CPU usage
2. `memory` => RAM usage
3. `blio` => Disk I/O bandwidth
4. `net_cls` => Network traffic shaping
5. `devices` => Access to specific hardware devices

So how does Docker (🐳) come into this equation? 🤔

Docker usses a technology called `namespaces` to provide the isoalted workspace called the container. When you run a container, Docker creates a set of namespaces for that container - Docker Docs.

These namespaces provide a layer of isolation. Each aspect of a container runs in a separate namespace and its access is limited to that namespace

To test this out I decided to run a container and execute into the shell
  - docker exec -it ubuntu-container /bin/bash
  - Docker will use a namespace so the container has it's own network, PID list, file system, etc.

- docker inspect --format='{{.Id}}' demo-test = Container ID
- docker inspect --format "{{.State.Pid}}" demo-test = Process ID

