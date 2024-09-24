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

# 2


