enable
configure terminal

hostname R2
ip domain-name cisco.com
crypto key generate rsa
1024

ip ssh version 2

username cisco secret cisco
enable secret cisco

line vty 0 4
transport input ssh
login local
exit

interface Serial 0/0/0
ip address 192.168.69.162 255.255.255.252
no shutdown
exit

! etherchannel

interface port-channel 1.617
encapsulation dot1q 617
ip address 192.168.69.65 255.255.255.224

interface po1.904
encapsulation dot1q 904
ip address 192.168.69.1 255.255.255.192

interface range Gi0/0-1
channel-group 1 mode on
no shut
exit

Router 3
enable
configure terminal

hostname R3
ip domain-name cisco.com
crypto key generate rsa
1024

ip ssh version 2

username cisco secret cisco
enable secret cisco

line vty 0 4
transport input ssh
login local
exit

interface Serial 0/0/1
ip address 192.168.69.166 255.255.255.252
no shutdown
exit

! etherchannel

interface port-channel 1.617
encapsulation dot1q 617
ip address 192.168.69.129 255.255.255.224

interface po1.904
encapsulation dot1q 904
ip address 192.168.69.97 255.255.255.224

interface range Gi0/0-1
channel-group 1
no shut
exit
