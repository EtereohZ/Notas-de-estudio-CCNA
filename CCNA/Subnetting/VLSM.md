Variable-Length Subnet Masks
Until now, we have practiced [[CCNA/Subnetting/Subnetting|subnetting]] using FLSM (Fixed-Length Subnet Masks)
This means that all of the subnets use the same prefix length (Ex. subnetting a class C network into 4 subnets using /26)

 VLSM is the process of creating subnets of different sizes, to make sure your use of network addresses more efficient.


Pasos
1. Asignar la subnet mas grande al inicio del espacio de la dirección
2. Asignar la mas grande que venga
3. Repetir hasta asignar todas
Se tiene que mantener la continuidad
![[Captura de pantalla 2024-08-30 175216.png]]

#### To LAN A
192.168.1.0/24 requiere 110 hosts

11000000.10101000.00000001.**0**0000000
tomo 1 bit
Red: 192.168.1.0/25
Broad: 192.168.1.127/25
First usable: 192.168.1.1/25
Last usable: 192.168.1.126/25
Total: 126

#### Toronto *LAN* B
 
192.168.1.128/26 requiere 45 hosts
11000000.10101000.00000001.**10**000000
Tomo 2 bits

Red: 192.168.1.128/26
Broadcast   192.168.1.191/26
First usable 192.168.1.129/26
Last usable 192.168.1.190/26
Total: 62

#### Toronto LAN A

192.168.1.129/24  requiere 29 hosts
11000000.10101000.00000001.**110**00000
Tomo 3 bits

red 192.168.1.192/27
Broad: 192.168.1.223/27
First usable: 192.168.1.193/27
Last usable: 192.168.1.222/27
Total: 30

#### Tokyo LAN B requiere 8 hosts
192.168.1.224
11000000.10101000.00000001.**1110**0000
Tomo 4 bits

red 192.168.1.224/28
Broad: 192.168.1.239/28
First usable: 192.168.1. 225/28
Last usable: 192.168.1.238/28
Total: 14

#### Point-to-Point
192.168.1.240
11000000.10101000.00000001.**111100**00
Tomo 6 bits

Red: 192.168.1.240/30
Broadcast: 192.168.1.243/30
First usable:192.168.1.241/30
Last usable: 192.168.1.242/30
Total: 2


192.168.5.0/24

#### LAN2 requiere 64 hosts 
192.168.5.0/24
11000000.10101000.00000101.**0**0000000
Tomamos 1 bit

Broad: 192.168.5.127/25
First usable: 192.168.5.1/25
Last usable: 192.168.5.126/25
Total: 126
Subnet mask: 255.255.255.128
#### LAN1 requiere 45 hosts
192.168.5.128/25
11000000.10101000.00000101.**10**000000
Tomamos 2 bits

Broad: 192.168.5.191/26
First usable: 192.168.5.129/26
Last usable: 192.168.5.190/26
Total: 62
Subnet mask: 255.255.255.192

#### LAN3 requiere 14 hosts
192.168.5.192
11000000.10101000.00000101.**1100**0000
tomamos 4 bits

Broad: 192.168.5.207/28
First usable: 192.168.5.193/28
Last usable: 192.168.5.206/28
Total: 14
Subnet mask: 255.255.255.240

#### LAN 4 requiere 9 hosts
192.168.5.208
11000000.10101000.00000101.**1101**0000
tomamos 4 bits

Broad: 192.168.5.223/28
First usable: 192.168.5.209/28
Last usable: 192.168.5.222/28
total: 14
Subnet mask: 255.255.255.240

#### Point-to-Point
192.168.5.224
11000000.10101000.00000101.**111000**00
Tomamos 6 bits

Broad: 192.168.5.227/30
First usable 192.168.5.225/30
Last usable: 192.168.5.226/30
Total: 2
Subnet mask: 255.255.255.252



192.168.5.0/24

LAN2


red: 192.168.5.0/25
Broad: 192.168.5.127/25
First usable: 192.168.5.1/25
Last usable: 192.168.5.126/25
Total: 126

LAN1
**1**0000000

red: 192.168.5.128/26
Broad: 192.168.191
First usable: 192.168.5.129/26
Last usable: 192.168.5.190/26
Total: 62

LAN3
**1**0000000

red: 192.168.5.192/27
Broad: 192.168.5.223/27
First usable: 192.168.5.193/27
Last usable: 192.168.5.222/27
Total: 30

LAN4


red: 192.168.5.224
Broad: 192.168.5.239
First usable: 192.168.5.225
Last usable: 192.168.5.238/28
Total: 14

Point to Point


red: 192.168.5.240/30
Broad: 192.158.5.243
First usable: 192.168.5.241
Last usable: 192.168.5.242/30
Total: 2


B management
10.00000000.00000000.**0001**00000

