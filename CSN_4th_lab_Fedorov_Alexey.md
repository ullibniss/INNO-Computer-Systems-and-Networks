# CSN. Lab 4. Networking Reconnaissance and Analysis

# Done by Fedorov Alexey

---

# Task 1. Task 1: Wireshark

## 1.1. Download this pcap file and try to investigate the traffic flow and extract any artifact that is inside the pcap file. 

Let's do it. I downloaded Task.pcapng file and opened it in wireshark.

![image](https://github.com/user-attachments/assets/e5f3f1ad-085c-4c9d-a187-f8d2ae3dfca8)

I see ftp protocol and I suppose image is transfered this way. I filtered ftp data.

![image](https://github.com/user-attachments/assets/7c26503b-9c13-492c-907e-fef20422d137)

File transfer starts at RETR command. Filename is `key.zip`. I will you Follow Stream function of Wireshark to extract file.

![image](https://github.com/user-attachments/assets/25095e69-095d-4d99-abb2-c708d2d2ee03)

I found that regardely it is not an image. It is server private key. Task is harder than I thought.

![image](https://github.com/user-attachments/assets/e449de02-020a-499e-972d-00fa38704ffb)

Now I have to decrypt TLS connection. The fact I know private key let me decrypt pre-master and make a `session key`. I will use Wireshark's fuctionality that allow to insert server's private key and decrypt TLS traffic.

![image](https://github.com/user-attachments/assets/70b9be74-2fde-48a6-a7b6-3d18deea7794)

Great. TLS decrypted!

![image](https://github.com/user-attachments/assets/910e2793-4b67-41f7-b373-bc3793f82808)

Lets extract image using http stream

![image](https://github.com/user-attachments/assets/ae2a5ae1-c330-47c0-a20e-98e100f06405)

I extract files as hex an convert them to binary files via [tomeko hex to bytes tool](https://tomeko.net/online_tools/hex_to_file.php?lang=en).

The result is:

![image](https://github.com/user-attachments/assets/86b6cb79-c2c9-4081-93de-3af8b4daff78)

## 1.2 Try to do a networking activity (for example, pinging Google DNS) and then use Wireshark filters to show only that activity.

Let's ping ya,ru and try to filter this activity

For this purpose I need to listen to network interface

![image](https://github.com/user-attachments/assets/e5510da8-9241-4963-9c3f-5c64780bd87d)

Let's start ping

![image](https://github.com/user-attachments/assets/11c9e655-0bda-401b-b681-0739744485f3)

Ping uses ICMP protocol. For the beggining lets filter ICMP packets

![image](https://github.com/user-attachments/assets/6f730413-ab84-4ac0-8d32-320bf706d3bf)

But it only filters by protocol. We need to filter by ip. Firstly I have to know ya.ru ip address

![image](https://github.com/user-attachments/assets/9eb3dbfc-28da-4440-889f-6aa0ee3050da)

And filter is:

![image](https://github.com/user-attachments/assets/404190c6-0093-454d-ba1c-c60b51898a33)

# 2. Nmap
```
Do an Nmap scan of your localhost or any virtual machine that you are allowed to
scan, what can you see?
```
I have virtual machine in cloud

![image](https://github.com/user-attachments/assets/dea59666-288b-43b6-b13d-c83c63a1dc2c)

Let's scan it.

## 2.1 Do an all-port scan

To scan all ports I will use `-p` flag with range `1-65535`

![image](https://github.com/user-attachments/assets/908e5d46-639c-4ee2-888d-2324d9fc4899)

As we can see 3 ports are open and 3 ports are filtered. Other ports are closed.

## 2.2  Do a version enumeration scan

Nmap can not only scan operationg system scan, but popular ports services (22-SSH for example).To enumerate versions I use flag `-sV`

![image](https://github.com/user-attachments/assets/6c0aabb4-c58a-4b89-b578-2680bbfd9e8b)

Nmap detected ssh version and operation system distro.

Actual version is:

![image](https://github.com/user-attachments/assets/968bc6b8-91f4-4d93-a330-295fd9d099c0)

Nmap is right!

## 2.3  When scanning a Windows system Nmap will stop the scan and report that the host is down, what can you do to solve this issue?

For windows hosts scan I can use `-Pn` flag. It forces Nmap to treat every host as online. 

![image](https://github.com/user-attachments/assets/8d1eca43-b83a-451e-a280-bea7c539fb6f)

## 2.4 . IF YOU ARE IN INNOPOLIS AND YOU HAVE A 10.1.1.X IP ADDRESS, then do a scan for the whole network (10.1.1.0/24)

I am in innopolis. Let's do a scan of whole network. 

![image](https://github.com/user-attachments/assets/197ce1c2-213e-42e5-befd-e1d37c90d8ba)

I saved scan to file. Let's analyse.

I want to know how much hosts is up for now:

![image](https://github.com/user-attachments/assets/064bc4a8-760a-4247-b8e8-efed0711f5f1)

How many 80 ports are open.

![image](https://github.com/user-attachments/assets/6465d6b5-2bf5-4563-82a2-6d7b946931a9)

6 ports are open.

# Task 3. Reconnaissance

```
There are two types of reconnaissance, active and passive. What are the differences
between them? which one would you use? Can you do a passive scan of the local
subnet that is connected to your PC? (Make sure you are connected to the 10.1.1.X
subnet or your own local subnet, for example home router)
```

Active reconnaissance involves interacting with the target system or network to gather information. This includes techniques such as running a port scan on the server to identify open ports and services, attempting to access restricted pages or resources within the application, or using tools to try and identify vulnerabilities within the application or underlying system.  
 
On the other hand, passive reconnaissance is gathering information from publicly available sources without actively interacting with the target system or network. This includes techniques such as analyzing the target application's website and social media presence, looking up information about the application's developers and users, and reviewing publicly available documents such as user manuals and support documentation.  

Passive scan of local area network means that I need to capture all packets (not only addressed for my device). To do this, I need to set my interface to `promisc` mode. Let's do it

![image](https://github.com/user-attachments/assets/feaddaa9-c523-4757-82a1-47691dced546)

Now I can capture traffic with wireshark:

![image](https://github.com/user-attachments/assets/43ecb2c6-8461-4d21-8a71-95aba6e8be64)

![image](https://github.com/user-attachments/assets/74a59e81-9004-4b73-ba36-66f81344f078)

As you can see, there are packets not only for my computer.

## References

1. https://habr.com/ru/articles/253521/
2. https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/
3. https://nmap.org/book/man-port-specification.html
4. https://nmap.org/book/man-version-detection.html
