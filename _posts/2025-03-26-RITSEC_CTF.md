---
title: "RITSEC_CTF"
categories: [Forensics]
tags: [Forensics]
image: /assets/img/ritsec25/logo_transparent.png

show_excerpts: false

---

# RITSEC_CTF Forensics Writeup

---
## **Challenge : Intercepted Transmission**


### **Challenge Description**
- We have intercepted a transmission from the aliens. We believe they were pinging government installations in order to find the locations.

---

### **Solution**

1. **Analyze the PCAP file:**  
   - We are given `transmission.pcapng`. Opening it in **Wireshark**, we see multiple ICMP packets.  
   ![flag](/assets/img/ritsec25/ritsec0.png)  

2. **Filter ICMP packets:**  
   - Apply the following Wireshark filter to isolate ICMP traffic:
     ```
     icmp
     ```
   - Observing the **data field** in ICMP packets, we notice that some contain readable text.

3. **Identify Flag Containing Packets:**  
   - By sorting packets based on **length**, we discover that **ICMP packets with a data length of 43** contain pieces of the flag.
     ![data](/assets/img/ritsec25/ritsec1.png)  
     ![data](/assets/img/ritsec25/ritsec2.png)  
   - Extracting data from these specific packets, we concatenate them to reconstruct the flag.

4. **Extract Flag using TShark:**  
   - Using `tshark`, we can extract ICMP data and convert it:
     ```bash
     tshark -r transmission.pcapng -Y "icmp && frame.len == 43" -T fields -e data | xxd -r -p
     ```
   - This outputs the final flag.
     ![flag](/assets/img/ritsec25/ritsec3.png)  

### **Flag**
```
RS{Its_A_Coverup} 

```



