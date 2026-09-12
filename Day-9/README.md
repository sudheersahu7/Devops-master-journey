Bilkul. Main is **Day 9 — Subnetting, CIDR & IP Calculation** ko simple Hinglish mein, **GitHub README ke liye ready format** mein parts mein explain karta hoon. Har concept ke saath example bhi rakhenge, taaki sirf ratna nahi pade.

 # Part 1 — IPv4 kya hota hai?

 Sabse pehle IP address samjho.

 Example:

```
192.168.1.10
```

 IPv4 address total **32 bits** ka hota hai.

 Isko 4 parts mein divide kiya gaya hai:

```
192 . 168 . 1 . 10
 │     │    │    │
8bit  8bit 8bit 8bit
```

 So:

```
8 + 8 + 8 + 8 = 32 bits
```

 Har part ko **octet** bolte hain.

 Har octet ki value:

```
0 - 255
```

 ke beech ho sakti hai.

 ### Binary example

```
192.168.1.10
```

 Binary mein:

```
11000000.10101000.00000001.00001010
```

 Isliye IPv4 ke paas total 32 bits hote hain.

---

 # Part 2 — Network aur Host kya hote hain?

 IP address ko broadly 2 parts mein samjho:

```
Network part + Host part
```

 Example:

```
192.168.1.10/24
```

 `/24` ka matlab:

```
First 24 bits = Network
Remaining 8 bits = Host
```

 Simple diagram:

```
192.168.1       .10
<------------>  <-->
   Network       Host
```

 Matlab:

 > Network batata hai ki machine kis network mein hai.

 > Host batata hai ki us network ke andar kaunsi particular machine hai.

 Example:

```
Network:
192.168.1.0

Host:
192.168.1.10
```

---

 # Part 3 — CIDR kya hai?

 CIDR ka full form hai:

 **Classless Inter-Domain Routing**

 Example:

```
192.168.1.0/24
```

 Yahan `/24` important hai.

 Iska matlab:

```
32 total bits
24 network bits
8 host bits
```

 Formula:

```
Host bits = 32 - CIDR
```

 For `/24`:

```
32 - 24 = 8
```

---

 # Part 4 — `/24` ko simple way mein samjho

 Network:

```
192.168.1.0/24
```

 Host bits:

```
32 - 24 = 8
```

 8 host bits hain, isliye:

```
2^8 = 256
```

 Total addresses:

```
256
```

 Range:

```
192.168.1.0
       ↓
192.168.1.255
```

 Normally:

```
Network address = 192.168.1.0
Broadcast address = 192.168.1.255
```

 Hosts:

```
192.168.1.1
192.168.1.2
...
192.168.1.254
```

 Traditional networking mein:

```
256 total
- 1 network
- 1 broadcast
----------------
254 usable
```

 ### Important AWS note

 AWS VPC subnet mein calculation thodi different hoti hai because AWS **5 IP addresses reserve** karta hai. Isko AWS section mein separately dekhenge.

---

 # Part 5 — CIDR ka golden formula

 Ye formula yaad rakho:

```
Host bits = 32 - CIDR
```

 Then:

```
Total IP addresses = 2^(host bits)
```

 Direct formula:

```
Total IPs = 2^(32 - CIDR)
```

 Examples:

 ### `/24`

```
32 - 24 = 8

2^8 = 256
```

 ### `/26`

```
32 - 26 = 6

2^6 = 64
```

 ### `/28`

```
32 - 28 = 4

2^4 = 16
```

 ### `/30`

```
32 - 30 = 2

2^2 = 4
```

 Simple rule:

 > **CIDR number jitna bada, network utna chhota.**

 For example:

```
/16 → bada network
/24 → smaller
/26 → aur smaller
/28 → aur bhi smaller
```

---

 # Part 6 — Subnetting kya hota hai?

 Ab maan lo tumhare paas:

```
192.168.1.0/24
```

 Isme:

```
256 IP addresses
```

 hain.

 Tumhe ye network 4 departments ko dena hai.

 Instead of ek hi bada network use karne ke, tum ise 4 smaller networks mein divide kar sakte ho.

 Ye process:

 > **Subnetting**

 kehlata hai.

 Example:

```
192.168.1.0/26
192.168.1.64/26
192.168.1.128/26
192.168.1.192/26
```

 Har subnet mein:

```
64 IP addresses
```

 hain.

 4 × 64:

```
256
```

 Original `/24` ke saare IPs cover ho gaye.

---

 # Part 7 — `/24` ko `/26` mein kaise divide karein?

 Original:

```
192.168.1.0/24
```

 New:

```
/26
```

 Host bits:

```
32 - 26 = 6
```

 Total IPs:

```
2^6 = 64
```

 Therefore har subnet ka block size:

```
64
```

 So ranges:

```
0 - 63
64 - 127
128 - 191
192 - 255
```

 Hence:

 ### Subnet 1

```
Network:   192.168.1.0
Hosts:     192.168.1.1 - 192.168.1.62
Broadcast: 192.168.1.63
```

 ### Subnet 2

```
Network:   192.168.1.64
Hosts:     192.168.1.65 - 192.168.1.126
Broadcast: 192.168.1.127
```

 ### Subnet 3

```
Network:   192.168.1.128
Hosts:     192.168.1.129 - 192.168.1.190
Broadcast: 192.168.1.191
```

 ### Subnet 4

```
Network:   192.168.1.192
Hosts:     192.168.1.193 - 192.168.1.254
Broadcast: 192.168.1.255
```

---

 # Part 8 — Block Size kya hota hai?

 Subnetting mein **block size** bahut useful shortcut hai.

 Example:

```
/26
```

 Subnet mask:

```
255.255.255.192
```

 Last interesting octet:

```
192
```

 Block size:

```
256 - 192 = 64
```

 Therefore boundaries:

```
0
64
128
192
```

 Isi wajah se `/26` ke networks hain:

```
.0
.64
.128
.192
```

 ### Common block sizes

```
/25 → 128
/26 → 64
/27 → 32
/28 → 16
/29 → 8
/30 → 4
```

 Isko yaad rakhna subnetting ke questions mein bahut helpful hai.

---

 # Part 9 — Subnet Mask kya hota hai?

 CIDR ka traditional subnet-mask representation bhi hota hai.

```
/24 → 255.255.255.0
```

```
/25 → 255.255.255.128
```

```
/26 → 255.255.255.192
```

```
/27 → 255.255.255.224
```

```
/28 → 255.255.255.240
```

 Simple table:

 | CIDR | Subnet Mask | Total IPs |
| --- | --- | --- |
| `/24` | `255.255.255.0` | 256 |
| `/25` | `255.255.255.128` | 128 |
| `/26` | `255.255.255.192` | 64 |
| `/27` | `255.255.255.224` | 32 |
| `/28` | `255.255.255.240` | 16 |
| `/29` | `255.255.255.248` | 8 |
| `/30` | `255.255.255.252` | 4 |

---

 # Part 10 — Kisi IP ka Network Address kaise find karein?

 Ye interview mein bahut common question hai.

 Example:

```
192.168.1.75/26
```

 ### Step 1 — `/26` ka block size find karo

```
/26 → 64
```

 ### Step 2 — Boundaries likho

```
0
64
128
192
```

 ### Step 3 — IP dekho

 IP ka last octet:

```
75
```

 75 kis range mein hai?

```
64 - 127
```

 Therefore:

```
Network = 192.168.1.64
```

 Broadcast:

```
192.168.1.127
```

 Hosts:

```
192.168.1.65 - 192.168.1.126
```

 ### Easy trick

 IP ko dekho aur pucho:

 > "Ye IP kis block ke andar aa raha hai?"

 Bas wahi block ka starting number **network address** hai.

---

 # Part 11 — Ek aur example

 Question:

```
10.10.20.150/27
```

 `/27` ka block size:

```
32
```

 Boundaries:

```
0
32
64
96
128
160
192
224
```

 IP:

```
150
```

 150 is range mein hai:

```
128 - 159
```

 Therefore:

```
Network:
10.10.20.128
```

 Broadcast:

```
10.10.20.159
```

 Host range:

```
10.10.20.129
-
10.10.20.158
```

---

 # Part 12 — Network Address, Host Address aur Broadcast

 Ye teen concepts clearly samjho.

 Suppose:

```
192.168.10.0/24
```

 ### Network address

```
192.168.10.0
```

 Ye poore network ko represent karta hai.

 ### Host addresses

```
192.168.10.1
192.168.10.2
...
192.168.10.254
```

 Ye machines/devices ko assign kiye ja sakte hain in traditional IPv4 subnetting.

 ### Broadcast address

```
192.168.10.255
```

 Ye subnet ke sabhi hosts ko broadcast traffic bhejne ke liye traditional IPv4 networking mein use hota hai.

 So:

```
192.168.10.0     → Network
192.168.10.1     → Host
192.168.10.2     → Host
...
192.168.10.254   → Host
192.168.10.255   → Broadcast
```

---

 # Part 13 — `/24` ko `/25` mein divide karna

 Original:

```
192.168.1.0/24
```

 Total:

```
256 IPs
```

 Now `/25`:

```
32 - 25 = 7 host bits
```

 Total:

```
2^7 = 128
```

 So 2 subnets banenge:

```
192.168.1.0/25
192.168.1.128/25
```

 ### First subnet

```
Network:   192.168.1.0
Hosts:     .1 - .126
Broadcast: .127
```

 ### Second subnet

```
Network:   192.168.1.128
Hosts:     .129 - .254
Broadcast: .255
```

 Notice:

```
128 + 128 = 256
```

---

 # Part 14 — `/16` ko `/20` mein divide karna

 Example:

```
10.0.0.0/16
```

 Original host bits:

```
32 - 16 = 16
```

 Total:

```
2^16 = 65,536
```

 Now `/20`.

 Host bits:

```
32 - 20 = 12
```

 IPs per subnet:

```
2^12 = 4096
```

 Kitne `/20` subnets banenge?

```
20 - 16 = 4 borrowed bits

2^4 = 16 subnets
```

 So:

```
10.0.0.0/20
10.0.16.0/20
10.0.32.0/20
10.0.48.0/20
...
10.0.240.0/20
```

 Yahan interesting octet second-last hai.

 `/20` mask:

```
255.255.240.0
```

 Block size:

```
256 - 240 = 16
```

 Isliye:

```
0
16
32
48
64
...
240
```

---

 # Part 15 — VLSM kya hai?

 VLSM ka full form:

 > **Variable Length Subnet Mask**

 Simple language mein:

 > Har team/application ko same-size subnet dena zaroori nahi hai.

 Example:

```
Team A → 500 hosts
Team B → 100 hosts
Team C → 50 hosts
Team D → 10 hosts
```

 Agar sabko `/24` de diya:

```
Team A → /24
Team B → /24
Team C → /24
Team D → /24
```

 bahut saare IPs waste honge.

 Better:

```
Team A → /23
Team B → /25
Team C → /26
Team D → /28
```

 Yahi VLSM hai.

 ### DevOps mein importance

 Real infrastructure mein different workloads ko different IP requirements hoti hain.

 For example:

```
Load Balancers → small subnet
Application servers → bigger subnet
Database → controlled subnet
Kubernetes nodes → potentially larger subnet
```

 Toh VLSM IP space efficiently use karne mein help karta hai.

---

 # Part 16 — Private IP ranges

 Internet par directly routable private IP ranges:

```
10.0.0.0/8
```

 Range:

```
10.0.0.0 - 10.255.255.255
```

 Second:

```
172.16.0.0/12
```

 Range:

```
172.16.0.0 - 172.31.255.255
```

 Third:

```
192.168.0.0/16
```

 Range:

```
192.168.0.0 - 192.168.255.255
```

 Common AWS VPC example:

```
10.0.0.0/16
```

---

 # Part 17 — AWS VPC mein subnetting

 Ab actual DevOps connection samjho.

 Suppose AWS mein VPC hai:

```
10.0.0.0/16
```

 Ye tumhara bada network hai.

 Iske andar tum multiple subnets bana sakte ho:

```
VPC
10.0.0.0/16
│
├── Public Subnet
│   10.0.32.0/24
│
├── Public Subnet
│   10.0.33.0/24
│
├── Private Subnet
│   10.0.0.0/20
│
└── Private Subnet
    10.0.16.0/20
```

 Conceptually:

```
                 VPC
             10.0.0.0/16
                   │
        ┌──────────┴──────────┐
        │                     │
       AZ-A                  AZ-B
        │                     │
   ┌────┴────┐           ┌────┴────┐
   │         │           │         │
 Public   Private      Public   Private
```

 Is tarah networking ko logically organize karte hain.

---

 # Part 18 — Public vs Private subnet

 Subnetting aur routing ko confuse mat karna.

 CIDR decide karta hai:

 > IP addresses ka range kya hoga?

 Routing decide karta hai:

 > Traffic kahan jayega?

 For example:

```
10.0.32.0/24
```

 sirf ek subnet CIDR hai.

 Us subnet ko public/private behavior dene mein **route table aur other AWS configuration** important role play karte hain.

 Simple mental model:

```
CIDR
 ↓
IP range

Route Table
 ↓
Traffic direction

Internet Gateway / NAT etc.
 ↓
Connectivity
```

---

 # Part 19 — Kubernetes mein subnetting

 Kubernetes/EKS mein networking thodi complex ho sakti hai.

 Conceptually tum ye layers dekh sakte ho:

```
VPC
 ↓
Subnet
 ↓
Node networking
 ↓
Pod networking
 ↓
Service networking
```

 Example:

```
AWS VPC
10.0.0.0/16
      │
      └── Subnet
          10.0.1.0/24
              │
              └── Nodes
                  │
                  └── Pods
```

 Important:

 > EKS mein pod IP allocation exactly kaise hota hai, ye CNI/configuration par depend karta hai.

 Isliye abhi sirf architecture samjho. Kubernetes networking ke detailed CIDRs hum Kubernetes networking chapter mein deeply cover kar sakte hain.

---

 # Part 20 — Linux commands

 Networking practically dekhne ke liye Linux mein ye commands important hain.

 ### IP address check

```
ip addr
```

 Short and clean:

```
ip -br addr
```

 Example:

```
eth0    UP    10.32.124.127/20
```

 Iska matlab:

```
Interface = eth0
IP = 10.32.124.127
CIDR = /20
```

---

 ### Routing check

```
ip route
```

 Example:

```
default via 10.32.112.1 dev eth0
```

 Iska simple meaning:

```
default traffic
       ↓
gateway 10.32.112.1
       ↓
eth0
```

---

 ### Interface information

```
ip addr show
```

---

 ### Neighbor table

```
ip neigh
```

 Ye nearby Layer-2/ARP/neighbor information dekhne mein useful hai.

---

 ### IP calculation tool

 Agar installed ho:

```
ipcalc 192.168.1.75/26
```

 Ye automatically calculate kar sakta hai:

```
Network
Broadcast
Netmask
Host range
```

---

 # Part 21 — Subnetting solve karne ka fixed method

 Har question ke liye ye **5-step method** follow karo.

 Suppose:

```
192.168.1.75/26
```

 ### Step 1 — Host bits

```
32 - 26 = 6
```

 ### Step 2 — Total IPs

```
2^6 = 64
```

 ### Step 3 — Block size

 `/26`:

```
64
```

 ### Step 4 — IP kis block mein hai?

```
0-63
64-127  ← 75 yahan hai
128-191
192-255
```

 So:

```
Network = .64
```

 ### Step 5 — Broadcast

 Next block start:

```
128
```

 Usse ek kam:

```
127
```

 So:

```
Broadcast = .127
```

 Hosts:

```
.65 - .126
```

 Bas. Isi method se majority basic subnetting questions solve ho jayenge.

---

 # Part 22 — 5 practical calculations

 Ab khud solve karne se pehle examples dekho.

 ### Example 1

```
192.168.1.50/24
```

 `/24`:

```
256 IPs
```

 Therefore:

```
Network   = 192.168.1.0
Broadcast = 192.168.1.255
Hosts     = 192.168.1.1 - 192.168.1.254
```

---

 ### Example 2

```
192.168.1.100/26
```

 Block:

```
64
```

 Ranges:

```
0-63
64-127  ← 100
128-191
192-255
```

 Therefore:

```
Network   = 192.168.1.64
Broadcast = 192.168.1.127
Hosts     = 192.168.1.65 - 192.168.1.126
```

---

 ### Example 3

```
192.168.1.200/27
```

 Block size:

```
32
```

 Ranges:

```
0-31
32-63
64-95
96-127
128-159
160-191
192-223 ← 200
224-255
```

 Therefore:

```
Network   = 192.168.1.192
Broadcast = 192.168.1.223
Hosts     = 192.168.1.193 - 192.168.1.222
```

---

 ### Example 4

```
10.10.10.75/28
```

 Block size:

```
16
```

 Ranges:

```
0-15
16-31
32-47
48-63
64-79 ← 75
80-95
...
```

 Therefore:

```
Network   = 10.10.10.64
Broadcast = 10.10.10.79
Hosts     = 10.10.10.65 - 10.10.10.78
```

---

 ### Example 5

```
172.16.50.130/25
```

 `/25` block:

```
128
```

 Ranges:

```
0-127
128-255 ← 130
```

 Therefore:

```
Network   = 172.16.50.128
Broadcast = 172.16.50.255
Hosts     = 172.16.50.129 - 172.16.50.254
```

---

 # Part 23 — AWS Senior-Level Challenge

 Ab important real-world design.

 Requirement:

```
VPC = 10.0.0.0/16
```

 Need:

```
2 Public subnets
4 Application subnets
2 Database subnets
```

 Host requirement:

```
Public     → ~250 hosts each
Application → ~500 hosts each
Database   → ~100 hosts each
```

 ## Step 1 — CIDR choose karo

 Approx 250 hosts ke liye:

```
/24 = 256 total
```

 Traditional networking mein 254 usable.

 So:

```
Public → /24
```

 Approx 500 hosts ke liye:

```
/23 = 512 total
```

 Traditional networking mein 510 usable.

 So:

```
Application → /23
```

 Approx 100 hosts ke liye:

```
/25 = 128 total
```

 Traditional networking mein 126 usable.

 So:

```
Database → /25
```

 **AWS mein actual usable IPs 5 reserved addresses ki wajah se lower honge**, isliye production design mein AWS-specific capacity check zaroor karna.

---

 # Part 24 — Ek possible non-overlapping design

 VPC:

```
10.0.0.0/16
```

 Example allocation:

```
AZ-A
├── Public
│   └── 10.0.0.0/24
│
├── Application
│   └── 10.0.2.0/23
│
└── Database
    └── 10.0.4.0/25
```

 AZ-B:

```
├── Public
│   └── 10.0.1.0/24
│
├── Application
│   └── 10.0.6.0/23
│
└── Database
    └── 10.0.8.0/25
```

 Notice:

```
10.0.0.0/24
10.0.1.0/24
10.0.2.0/23
10.0.4.0/25
10.0.6.0/23
10.0.8.0/25
```

 Ye ranges overlap nahi kar rahe.

 Real production design mein subnet sizing ke saath future growth, AWS reserved addresses, AZ strategy, EKS IP consumption, load balancers, NAT architecture, etc. bhi consider karna padega.

---

 # Part 25 — CIDR overlap kya hai?

 Suppose:

```
Network A:
10.0.0.0/24
```

 and:

```
Network B:
10.0.0.0/25
```

 Problem:

```
10.0.0.0 - 10.0.0.127
```

 Network A aur B dono mein aa raha hai.

 Matlab:

 > **CIDRs overlap kar rahe hain.**

 Connected networks mein overlapping CIDRs routing problems create kar sakte hain because router ke paas clear answer nahi hota ki same destination IP range kis network ko bhejni hai.

 Isi wajah se AWS VPC peering, Transit Gateway, VPN, hybrid networking, etc. design karte waqt CIDR planning important hoti hai.

---

 # Part 26 — Default Gateway

 Suppose:

```
ip route
```

 Output:

```
default via 192.168.1.1 dev eth0
```

 Simple meaning:

 > Jab system ko kisi destination ke liye specific route nahi milta, to traffic default gateway `192.168.1.1` ki taraf bheja jayega.

 Think:

```
Your machine
     ↓
Default Gateway
     ↓
Other network
     ↓
Internet
```

---

 # Part 27 — `ip addr` vs `ip route`

 Ye interview mein yaad rakho:

 ### `ip addr`

 Batata hai:

```
Machine ke interfaces
IP addresses
CIDR
Interface status
```

 Command:

```
ip addr
```

 ### `ip route`

 Batata hai:

```
Traffic kaha jaana chahiye
Default gateway
Network routes
```

 Command:

```
ip route
```

 Simple:

```
ip addr
   ↓
"Mera IP kya hai?"

ip route
   ↓
"Traffic kaha bhejna hai?"
```

---

 # Part 28 — Day 9 ka mental map

 Poore topic ko ek chain mein yaad rakho:

```
IPv4
 ↓
32 bits
 ↓
Network + Host
 ↓
CIDR
 ↓
/24, /26, /28 etc.
 ↓
Host bits
 ↓
Total IPs
 ↓
Subnet mask
 ↓
Block size
 ↓
Network address
 ↓
Broadcast
 ↓
Host range
 ↓
Subnetting
 ↓
VLSM
 ↓
AWS VPC
 ↓
Kubernetes/EKS networking
```

 Agar ye chain samajh aa gayi, to subnetting ka foundation strong ho gaya.

---

 # Part 29 — README mein kya likhna hai?

 Tumhare `day-2-subnetting-cidr.md` ka structure aisa rakh sakte ho:

```
# Subnetting & CIDR

## 1. What is IPv4?

## 2. Network and Host

## 3. What is CIDR?

## 4. CIDR Calculation

## 5. Total IP Calculation

## 6. Subnet Mask

## 7. Block Size

## 8. Network Address

## 9. Broadcast Address

## 10. Host Range

## 11. Subnetting

## 12. VLSM

## 13. Private IP Ranges

## 14. AWS VPC Subnetting

## 15. Kubernetes Networking Connection

## 16. Linux Networking Commands

## 17. Practical Subnet Calculations

## 18. CIDR Overlap

## 19. Default Gateway

## 20. Interview Questions
```

 **Best approach:** README mein definitions + examples + apne practical outputs rakho. Sirf theory copy mat karo.

---

 # Part 30 — Sabse important cheat sheet

```
IPv4 = 32 bits

Host bits:
32 - CIDR

Total IPs:
2^(32 - CIDR)

Traditional usable hosts:
Total IPs - 2

Block size:
256 - interesting subnet-mask octet
```

 Common values:

```
/24 → 256 IPs → block 256
/25 → 128 IPs → block 128
/26 → 64 IPs  → block 64
/27 → 32 IPs  → block 32
/28 → 16 IPs  → block 16
/29 → 8 IPs   → block 8
/30 → 4 IPs   → block 4
```

 And:

```
/24 → 255.255.255.0
/25 → 255.255.255.128
/26 → 255.255.255.192
/27 → 255.255.255.224
/28 → 255.255.255.240
```

 ### One-line trick

 > **CIDR dekho → host bits nikalo → total IP nikalo → block size dekho → IP kis block mein hai → network + broadcast + host range nikalo.**

---

 ## Ab tumhara Day 9 practice

 Abhi **Day 10 par mat jaana**. Pehle ye 5 manually solve karo:

```
1. 192.168.1.50/24

2. 192.168.1.100/26

3. 192.168.1.200/27

4. 10.10.10.75/28

5. 172.16.50.130/25
```

 Har question ke liye sirf ye format use karo:

```
CIDR:
Host bits:
Total IPs:
Subnet mask:
Block size:
Network:
Broadcast:
Host range:
```

 Phir **AWS wala 2 Public + 4 App + 2 Database subnet design** bhi solve karo.

 Tum apne answers yahin bhejna. Main **ek-ek calculation verify karke**, jahan galti hogi wahan _why_ samjhaunga — especially interview perspective se.
