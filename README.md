# TCP-UDP-onwireshark

|Nama|NRP|
|----------------------------|----------|
|Timothy Hosi |5025211098|

1. [TCP-Problems](#TCP-Problems)
2. [UDP-Problems](#UDP-Problems)

### **TCP-Problems**
1. What is the IP address and TCP port number used by the client computer (source) that is transferring the alice.txt file to gaia.cs.umass.edu?
![image](https://github.com/thossb/TCP-UDP-onwireshark/assets/90438426/9f397264-b93d-4bb8-b92b-54e716babd3a)
``` 
Answer:
Filter dengan http.request.method == "POST"
IP address : 192.168.86.68
TCP port number : 55639
```
2. What is the IP address of gaia.cs.umass.edu? On what port number is it sending and receiving TCP segments for this connection?

``` 
Answer:
IP address : 128.119.245.12
Port : 80
```
3. What is the sequence number of the TCP SYN segment that is used to initiate the TCP connection between the client computer and gaia.cs.umass.edu? What is it in this TCP segment that identifies the segment as a SYN segment? Will the TCP receiver in this session be able to use Selective Acknowledgments?
![image](https://github.com/thossb/TCP-UDP-onwireshark/assets/90438426/702c5bd2-fb3f-4d25-a892-96aa65fca29b)
![image](https://github.com/thossb/TCP-UDP-onwireshark/assets/90438426/d46733f2-b753-44ab-a134-ac2c66afe194)
``` 
Answer:
Filter dengan tcp.flags.syn == 1 and ip.dst == 128.119.245.12
- Sequence number : 4236649187
- Lihatlah flag di TCP Control Bits untuk mengidentifikasi segmen sebagai segmen SYN
- Untuk memastikan apakah penerima TCP bermaksud menggunakan Pengakuan Selektif, periksa bagian opsi TCP dalam segmen SYN awal untuk mengidentifikasi keberadaan opsi SACK Diizinkan. Jika opsi ini ditemukan, ini menunjukkan bahwa SACK diaktifkan untuk sesi tersebut.```.
```

4. What is the sequence number of the SYNACK segment sent by gaia.cs.umass.edu to the client computer in reply to the SYN? What is it in the segment that identifies the segment as a SYNACK segment? What is the value of the Acknowledgement field in the SYNACK segment? How did gaia.cs.umass.edu determine that value?
![image](https://github.com/thossb/TCP-UDP-onwireshark/assets/90438426/d76f293d-66de-4cc3-8bf4-545532824e54)
``` 
Answer:
Filter dengan tcp.flags.syn == 1 and tcp.flags.ack == 1 and ip.src == 128.119.245.12
-  Sequence number (raw) : 1068969752
- Lihatlah flag di TCP Control Bits untuk mengidentifikasi segmen sebagai segmen SYNACK
-  Acknowledge number (raw) : 4236649188
-  Situs web gaia.cs.umass.edu menjelaskan bahwa mereka menentukan nilai dalam Acknowledgment field di dalam segmen SYN/ACK dengan menambahkan 1 pada nomor urutan yang mereka terima dari paket SYN. Dalam protokol TCP, ini adalah praktik yang diterapkan di mana nomor Acknowledgment dalam segmen SYN/ACK adalah hasil dari menambahkan 1 pada nomor urutan yang diterima sebelumnya dalam paket SYN. 
```

5. What is the sequence number of the TCP segment containing the header of the HTTP POST command?. How many bytes of data are contained in the payload (data) field of this TCP segment? Did all of the data in the transferred file alice.txt fit into this single segment?
![image](https://github.com/thossb/TCP-UDP-onwireshark/assets/90438426/038b7bf6-1d4f-4750-a252-5563bfe38976)
![image](https://github.com/thossb/TCP-UDP-onwireshark/assets/90438426/b9cf2cc9-845d-4697-9f8c-7ea670ea05d4)
``` 
Answer:
-  Sequence Number (raw) : 4236649188**
-  Payload (data) size : 1385  bytes**
- tidak cukup karena "alice.txt" lebih 152K bytes 
```

6.  Consider the TCP segment containing the HTTP “POST” as the first segment in the data transfer part of the TCP connection.
* At what time was the first segment (the one containing the HTTP POST) in the data-transfer part of the TCP connection sent?
![image](https://github.com/thossb/TCP-UDP-onwireshark/assets/90438426/4d59849a-2cd1-4857-b388-5fd39532df2c)
``` 
Answer: 0.147682s
```
* At what time was the ACK for this first data-containing segment received?
![image](https://github.com/thossb/TCP-UDP-onwireshark/assets/90438426/e2b5cc13-41b8-48d4-af6a-a40c9c996b3a)
``` 
Answer: 0.149626s
```
7. What is the length (header plus payload) of each of the first four data-carrying TCP segments?
![image](https://github.com/thossb/TCP-UDP-onwireshark/assets/90438426/18fc654d-655c-448b-b318-ad72696e6a4c)
```
Answer:
Packet 1: header = 44 bytes; payload = 0 byte 
Packet 2: header = 40 bytes; payload = 0 byte 
Packet 3: header = 32 bytes; payload = 0 byte 
Packet 4: header = 32 bytes; payload = 1448 bytes
```
8. What is the minimum amount of available buffer space advertised to the client by gaia.cs.umass.edu among these first four data-carrying TCP segments7? Does the lack of receiver buffer space ever throttle the sender for these first four data- carrying segments?

```
answer :
Packet 1: Window = 65535
Packet 2: Window = 28960
Packet 3: Window = 2058
Packet 4: Window = 2058
Jadi minimumnya adalah 2058
Dia tidak throttle karena 1448 kurang dari 2058
```
9. Are there any retransmitted segments in the trace file? What did you check for (in the trace) in order to answer this question?

```
Answer :
Ya, kurangnya ruang buffer penerima dapat menghambat pengirim
menyebabkannya memperlambat laju transmisi datanya untuk memastikan data tersebut
tidak hilang atau terjepit di ujung penerima.
```
### **UDP-Problems**
1. Select the first UDP segment in your trace. What is the packet number4 of this segment in the trace file? What type of application-layer payload or protocol message is being carried in this UDP segment? Look at the details of this packet in Wireshark. How many fields there are in the UDP header?Name these fields.![image](https://github.com/thossb/TCP-UDP-onwireshark/assets/90438426/0385e882-dbc1-49aa-a791-6e4c192a324d)
```
Packet number   : 5 
Protocol message : SSDP
Port number     : 1900
Ada 4 field : Source port, Destination port, Length, and Checksum
```
2. By consulting the displayed information in Wireshark’s packet content field for this packet, determine the length (in bytes) of each of the UDP header fields.
```
Tiap UDP Header memiliki panjang 2 bytes, sehingga total panjang UDP Header adalah 8 bytes
```
3. The value in the Length field is the length of what?.Verify your claim with your captured UDP packet.
![image](https://github.com/thossb/TCP-UDP-onwireshark/assets/90438426/e6677d47-d9f1-49bc-8939-12b3c3bb4ba4)
```
Length menentukan jumlah byte di segmen UDP
(header ditambah data). Nilai panjang eksplisit diperlukan karena ukurannya
bidang data mungkin berbeda dari satu segmen UDP ke segmen UDP berikutnya.

Contoh Panjang payload UDP untuk paket nomer 5 adalah 275 byte.
283 byte - 8 byte = 275 byte.
```

4. What is the maximum number of bytes that can be included in a UDP payload? (Hint: the answer to this question can be determined by your answer to 2. above)
```
Jumlah maksimum byte yang dapat dimasukkan dalam payload UDP adalah (2^16 – 1) byte ditambah byte header.
Ini menghasilkan 65535 byte – 8 byte = 65527 byte.
```

5. What is the largest possible source port number? (Hint: see the hint in 4.)
```
Nomor port sumber terbesar yang mungkin adalah (2^16 – 1) = 65535.
```

6. What is the protocol number for UDP? Give your answer in both hexadecimal and decimal notation. To answer this question, you’ll need to look into the Protocol field of the IP datagram containing this UDP segment (see Figure 4.13 in the text, and the discussion of IP header fields).
![image](https://github.com/thossb/TCP-UDP-onwireshark/assets/90438426/33757451-191e-4753-8271-7ef5ba16c4a9)
```
Nomor protokol IP untuk UDP adalah 0x11 hex, yaitu 17 dalam nilai desimal.
```

7. Examine a pair of UDP packets in which your host sends the first UDP packet and the second UDP packet is a reply to this first UDP packet. (Hint: for a second packet to be sent in response to a first packet, the sender of the first packet should be the destination of the second packet). Describe the relationship between the port numbers in the two packets.
![image](https://github.com/thossb/TCP-UDP-onwireshark/assets/90438426/498560bf-dbdb-426d-bf19-3d7b7c8eadf6)
![image](https://github.com/thossb/TCP-UDP-onwireshark/assets/90438426/8b250a39-b8c5-4c9a-b5d4-845798324c6f)
```
Contoh paket nomor 15 dan 17
The source port of the UDP packet sent by the host is the same as the 
destination port of the reply packet, and conversely the destination 
port of the UDP packet sent by the host is the same as the source port 
of the reply packet.
```




















