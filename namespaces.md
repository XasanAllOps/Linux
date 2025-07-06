## 𝗪𝗵𝗮𝘁 𝗮𝗿𝗲 𝗡𝗮𝗺𝗲𝘀𝗽𝗮𝗰𝗲𝘀?

Namespaces in Linux isolate what a process can see — think of them as blinders for processes, preventing them from seeing or affecting the rest of the system.

A simple example to help drive the point home: Imagine a hotel with many rooms. Each room (namespace) has:

 • Its own bathroom  
 • Its own Wi-Fi  
 • Its own keycard  

Even though all the rooms are under the same roof (the host machine), the guests (processes) in Room 101 have no visibility or access to what’s happening in Room 102.

So how does Docker (🐳) come into this equation?

Docker uses a technology called `namespaces` to provide the isoalted workspace called the container. When you run a container, Docker creates a set of namespaces for that container - `Docker Docs`

Docker Engine uses `namespaces` extensively on Linux to isolate containers. Key types include:

 • `pid`: Process IDs – Each container sees only its own process list (ps aux, ps -ef)  
 • `net`: Network – Isolated interfaces, ports, and IPs  
 • `mnt`: Mount – Separate file systems & mountpoints  
 • `uts`: UTS – Containers can set their own hostname and domain name  
 • `ipc`: Interprocess Communication – Message queues, semaphores, and signals are isolated  

This is how Docker ensures containers are lightweight, secure, and independent.