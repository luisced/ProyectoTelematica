# Router 1 Config

Current configuration : 1602 bytes
!
! Last configuration change at 19:59:37 UTC Fri Mar 15 2024
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname router1
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
!
!
!
!
!
!
!
!
!
!
!
ip domain name cisco.com
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
cts logging verbose
!
!
license udi pid CISCO2901/K9 sn FJC1940A2CY
!
!
username cisco secret 5 $1$UiOu$cyuNd.tzRs75CPY18Rj.M.
!
redundancy
!
!
!
!
!
ip ssh version 2
!
!
!
!
!
!
!
!
!
!
interface Embedded-Service-Engine0/0
 no ip address
 shutdown
!
interface GigabitEthernet0/0
 ip address 172.16.10.1 255.255.255.252
 duplex auto
 speed auto
!
interface GigabitEthernet0/1
 ip address 172.16.10.6 255.255.255.252
 duplex auto
 speed auto
!
interface Serial0/0/0
 ip address 192.168.69.161 255.255.255.252
 clock rate 8000000
!
interface Serial0/0/1
 ip address 192.168.69.165 255.255.255.252
 clock rate 8000000
!
interface Serial0/1/0
 no ip address
 shutdown
 clock rate 2000000
!
interface Serial0/1/1
 no ip address
 shutdown
 clock rate 2000000
!
ip default-gateway 192.168.69.1
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
!
!
!
!
control-plane
!
!
!
line con 0
line aux 0
line 2
 no activation-character
 no exec
 transport preferred none
 transport output pad telnet rlogin lapb-ta mop udptn v120 ssh
 stopbits 1
line vty 0 4
 logging synchronous
 login local
 transport input ssh
line vty 5 15
 logging synchronous
 login local
 transport input ssh
!
scheduler allocate 20000 1000
!
end
