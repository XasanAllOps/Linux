## 𝗪𝗵𝗮𝘁 𝗔𝗿𝗲 𝗖𝗼𝗻𝘁𝗿𝗼𝗹 𝗚𝗿𝗼𝘂𝗽𝘀 (𝗰𝗴𝗿𝗼𝘂𝗽𝘀)? 

While namespaces 𝗶𝘀𝗼𝗹𝗮𝘁𝗲 what a process can see, control groups (cgroups) 𝗺𝗮𝗻𝗮𝗴𝗲 what a process can use.

Think of cgroups as resource managers that control how much CPU, memory, and other resources a process or container can consume.

Continuing the hotel analogy:
In the same hotel (the host machine), you can decide:

 • How much electricity each room can use 

 • How much water they get 

 • Whether they can access the gym 

This ensures no single guest (process) can hog all the hotel’s amenities. Similarly, cgroups prevent a single container from monopolising system resources.

What can cgroups manage? 

 • cpu – Limit CPU usage

 • memory – Cap RAM usage

 • blkio – Control disk I/O bandwidth

 • net_cls – Shape network traffic

 • devices – Restrict access to specific h/w

Together with namespaces, cgroups form the backbone of Linux containerization ensuring security, fairness, and isolation at scale.