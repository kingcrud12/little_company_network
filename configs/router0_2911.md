# Router0 : Cisco 2911

Running-config du routeur de bordure. Gi0/0 porte le lien de transit 10.0.0.0/30 vers l'ASA, avec des routes statiques vers les deux sous-réseaux internes.

[← Retour au README](../README.md)

```cisco
!
version 15.1
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname Router
!
!
!
!
!
!
!
!
ip cef
no ipv6 cef
!
!
!
!
license udi pid CISCO2911/K9 sn FTX1524ZZ0V-
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
spanning-tree mode pvst
!
!
!
!
!
!
interface GigabitEthernet0/0
 description FIREWALL LINK
 ip address 10.0.0.1 255.255.255.252
 duplex auto
 speed auto
!
interface GigabitEthernet0/1
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface GigabitEthernet0/2
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface Vlan1
 no ip address
 shutdown
!
ip classless
ip route 192.168.1.0 255.255.255.192 10.0.0.2 
ip route 192.168.1.64 255.255.255.224 10.0.0.2 
!
ip flow-export version 9
!
!
!
!
!
!
!
line con 0
!
line aux 0
!
line vty 0 4
 login
!
!
!
end
```
