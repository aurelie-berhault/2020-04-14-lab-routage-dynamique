# 2020-04-14-lab-routage-dynamique

Phase 1 : Configuration du Core

Étape 1 : Configuration globale des routeurs Core

  1.Fixer un nom d'hôte et un nom de domaine (FQDN) lan. 

         Router(config)#hostname R1
         R1(config)#ip domain-name lan
         R1(config)#crypto key generate rsa
         The name for the keys will be: R1.lan
         Choose the size of the key modulus in the range of 360 to 4096 for your
           General Purpose Keys. Choosing a key modulus greater than 512 may take
           a few minutes.

        How many bits in the modulus [512]: 2048
        % Generating 2048 bit RSA keys, keys will be non-exportable...
        [OK] (elapsed time was 1 seconds)


        Router(config)#hostname R2
        R2(config)#ip domain-name lan
        R2(config)#crypto key generate rsa
        The name for the keys will be: R2.lan
        Choose the size of the key modulus in the range of 360 to 4096 for your
          General Purpose Keys. Choosing a key modulus greater than 512 may take
          a few minutes.

        How many bits in the modulus [512]: 2048
        % Generating 2048 bit RSA keys, keys will be non-exportable...
        [OK] (elapsed time was 1 seconds)

        Router(config)#hostname R3
        R3(config)#ip domain-name lan
        R3(config)#crypto key generate rsa
        The name for the keys will be: R3.lan
        Choose the size of the key modulus in the range of 360 to 4096 for your
          General Purpose Keys. Choosing a key modulus greater than 512 may take
          a few minutes.

        How many bits in the modulus [512]: 2048
        % Generating 2048 bit RSA keys, keys will be non-exportable...
        [OK] (elapsed time was 1 seconds)

  2.Créer un enable secret : testtest. 

        R1(config)#enable secret testtest

        R2(config)#enable secret testtest

        R3(config)#enable secret testtest

  3.Créer un utilisateur privilégié root avec un mot de passe testtest. 

        R1(config)#username root secret testtest

        R2(config)#username root secret testtest

        R3(config)#username root secret testtest


  4.Activer uniquement SSHv2 sur les lignes VTY. 

        R1(config)#ip ssh version 2
        R1(config)#line vty 0 4
        R1(config-line)#transport input ssh
        R1(config-line)#login local

        R2(config)#ip ssh version 2
        R2(config)#line vty 0 4
        R2(config-line)#transport input ssh
        R2(config-line)#login local

        R3(config)#ip ssh version 2
        R3(config)#line vty 0 4
        R3(config-line)#transport input ssh
        R3(config-line)#login local


Étape 2 : Vérifier, activer et configurer les interfaces IPv4 

  1.Quel est le statut des interfaces ?

          R1#show ip int b
          Interface                  IP-Address      OK? Method Status                Protocol
          GigabitEthernet0/0         unassigned      YES unset  administratively down down    
          GigabitEthernet0/1         unassigned      YES unset  administratively down down    
          GigabitEthernet0/2         unassigned      YES unset  administratively down down    
          GigabitEthernet0/3         unassigned      YES unset  administratively down down    
          GigabitEthernet0/4         unassigned      YES unset  administratively down down    
          GigabitEthernet0/5         unassigned      YES unset  administratively down down    
          GigabitEthernet0/6         unassigned      YES unset  administratively down down    
          GigabitEthernet0/7         unassigned      YES unset  administratively down down

          Les interfaces ne sont ni activées, ni assignées.

  2.Activer et configurer IPv4 sur R1 les interfaces G0/0, G0/1, G0/2 et G0/3.

          R1(config)#interface g0/0
          R1(config-if)#no shutdown
          R1(config-if)#ip address 192.168.1.1 255.255.255.0
          R1(config-if)#exit
          R1(config)#interface g0/2
          R1(config-if)#no shutdown
          R1(config-if)#ip address 192.168.225.1 255.255.255.0
          R1(config-if)#exit
          R1(config)#interface g0/3
          R1(config-if)#no shutdown
          R1(config-if)#ip address 192.168.226.1 255.255.255.0
          R1(config-if)#exit
          R1(config)#interface g0/1
          R1(config-if)#no shutdown
          R1(config-if)#exit

  3. Activer et configurer IPv4 sur R2 les interfaces G0/0, G0/1 et G0/3.

          R2(config)#interface g0/0
          R2(config-if)#no shutdown
          R2(config-if)#ip address 192.168.33.1 255.255.255.0
          R2(config-if)#ipv6 address fe80::2 link-local
          R2(config-if)#ipv6 address fd00:fd00:fd00:2::1/64
          R2(config-if)#exit
          R2(config)#interface g0/1
          R2(config-if)#no shutdown
          R2(config-if)#ip address 192.168.22.2 255.255.255.0
          R2(config-if)#ipv6 address fe80::2 link-local
          R2(config-if)#exit
          R2(config)#interface g0/3
          R2(config-if)#no shutdown
          R2(config-if)#ip address 192.168.227.1 255.255.255.0
          R2(config-if)#ipv6 address fe80::2 link-local
          R2(config-if)#exit
          R2(config)#ipv6 unicast-routing

  4. Activer et configurer IPv4 sur R3 les interfaces G0/0, G0/1 et G0/2.

          R3(config)#interface g0/0
          R3(config-if)#no shutdown
          R3(config-if)#ip address 192.168.65.1 255.255.255.0
          R3(config-if)#ipv6 address fe80::3 link-local
          R3(config-if)#ipv6 address fd00:fd00:fd00:3::1/64
          R3(config-if)#exit
          R3(config)#interface g0/2
          R3(config-if)#no shutdown
          R3(config-if)#ip address 192.168.227.2 255.255.255.0   
          R3(config-if)#ipv6 address fe80::3 link-local
          R3(config-if)#exit
          R3(config)#interface g0/1
          R3(config-if)#no shutdown
          R3(config-if)#ip address 192.168.226.2 255.255.255.0
          R3(config-if)#ipv6 address fe80::3 link-local
          R3(config-if)#exit
          R3(config)#ipv6 unicast-routing

  5. Quel est le statut des interfaces ?

          R1#show ip int b
          Interface                  IP-Address      OK? Method Status                Protocol
          GigabitEthernet0/0         192.168.1.1     YES manual up                    up      
          GigabitEthernet0/1         unassigned      YES unset  up                    up      
          GigabitEthernet0/2         192.168.225.1   YES manual up                    up      
          GigabitEthernet0/3         192.168.226.1   YES manual up                    up      
          GigabitEthernet0/4         unassigned      YES unset  administratively down down    
          GigabitEthernet0/5         unassigned      YES unset  administratively down down    
          GigabitEthernet0/6         unassigned      YES unset  administratively down down    
          GigabitEthernet0/7         unassigned      YES unset  administratively down down

          R1#show ipv6 int b
          GigabitEthernet0/0     [up/up]
              FE80::1
              FD00:FD00:FD00:1::
              FD00:FD00:FD00:1::1
          GigabitEthernet0/1     [up/up]
              unassigned
          GigabitEthernet0/2     [up/up]
              FE80::1
          GigabitEthernet0/3     [up/up]
              FE80::1
          GigabitEthernet0/4     [administratively down/down]
              unassigned
          GigabitEthernet0/5     [administratively down/down]
              unassigned
          GigabitEthernet0/6     [administratively down/down]
              unassigned
          GigabitEthernet0/7     [administratively down/down]
              unassigned


          R2#show ip int b
          Interface                  IP-Address      OK? Method Status                Protocol
          GigabitEthernet0/0         192.168.33.1    YES manual up                    up      
          GigabitEthernet0/1         192.168.22.2    YES manual up                    up      
          GigabitEthernet0/2         unassigned      YES unset  administratively down down    
          GigabitEthernet0/3         192.168.227.1   YES manual up                    up      
          GigabitEthernet0/4         unassigned      YES unset  administratively down down    
          GigabitEthernet0/5         unassigned      YES unset  administratively down down    
          GigabitEthernet0/6         unassigned      YES unset  administratively down down    
          GigabitEthernet0/7         unassigned      YES unset  administratively down down    

          R2#show ipv6 int b
          GigabitEthernet0/0     [up/up]
              FE80::2
              FD00:FD00:FD00:2::1
          GigabitEthernet0/1     [up/up]
              FE80::2
          GigabitEthernet0/2     [administratively down/down]
              unassigned
          GigabitEthernet0/3     [up/up]
              FE80::2
          GigabitEthernet0/4     [administratively down/down]
              unassigned
          GigabitEthernet0/5     [administratively down/down]
              unassigned
          GigabitEthernet0/6     [administratively down/down]
              unassigned
          GigabitEthernet0/7     [administratively down/down]
              unassigned

          R3#show ip int b
          Interface                  IP-Address      OK? Method Status                Protocol
          GigabitEthernet0/0         192.168.65.1    YES manual up                    up      
          GigabitEthernet0/1         192.168.226.2   YES manual up                    up      
          GigabitEthernet0/2         192.168.227.2   YES manual up                    up      
          GigabitEthernet0/3         unassigned      YES unset  administratively down down    
          GigabitEthernet0/4         unassigned      YES unset  administratively down down    
          GigabitEthernet0/5         unassigned      YES unset  administratively down down    
          GigabitEthernet0/6         unassigned      YES unset  administratively down down    
          GigabitEthernet0/7         unassigned      YES unset  administratively down down    


          R3#show ipv6 int b
          GigabitEthernet0/0     [up/up]
              FE80::3
              FD00:FD00:FD00:3::1
          GigabitEthernet0/1     [up/up]
              FE80::3
          GigabitEthernet0/2     [up/up]
              FE80::3
          GigabitEthernet0/3     [administratively down/down]
              unassigned
          GigabitEthernet0/4     [administratively down/down]
              unassigned
          GigabitEthernet0/5     [administratively down/down]
              unassigned
          GigabitEthernet0/6     [administratively down/down]
              unassigned
          GigabitEthernet0/7     [administratively down/down]
              unassigned


  6. Comment pouvez-vous vous assurer de la connectivité de couche 2 entre les routeurs voisins ? 

          Ping entre les PC.

  7. Veuillez activer CDP et commenter votre diagnostic.

          CDP : Cisco Discovery Protocol

          R1(config)#cdp run
          R1(config)#exit
          R1#sh cdp
          Global CDP information:
            Sending CDP packets every 60 seconds
            Sending a holdtime value of 180 seconds
            Sending CDPv2 advertisements is  enabled

          R2(config)#cdp run
          R2(config)#exit

          R3(config)#cdp run
          R3(config)#exit


  8. Comment pouvez-vous vous assurer de la connectivité de couche 3 entre les routeurs voisins ? 
  
           Ping entre chaque routeur.


Étape 3 : Activer DHCP pour IPv4 

  1.Sur R1 créer un pool nommé LANR1, LANR2 et LANR3 pour chaque réseau local en définissant : 
    -	la plage du réseau,
    -	la passerelle,
    -	le serveur DNS 1.1.1.1. 

              R1(config)#ip dhcp pool LANR1
              R1(dhcp-config)#network 192.168.1.0 255.255.255.0
              R1(dhcp-config)#default-router 192.168.1.1
              R1(dhcp-config)#dns-server 1.1.1.1
              R1(dhcp-config)#exit
              R1(config)#ip dhcp pool LANR2
              R1(dhcp-config)#network 192.168.33.0 255.255.255.0
              R1(dhcp-config)#default-router 192.168.33.1       
              R1(dhcp-config)#dns-server 1.1.1.1
              R1(dhcp-config)#exit
              R1(config)#ip dhcp pool LANR3
              R1(dhcp-config)#network 192.168.65.0 255.255.255.0
              R1(dhcp-config)#default-router 192.168.65.1
              R1(dhcp-config)#dns-server 1.1.1.1
              R1(dhcp-config)#exit

  2.Exclure les cent premières adresses. 

              R1(config)#ip dhcp pool excluded-address 192.168.1.1 192.168.1.99
              R1(dhcp-config)#ip dhcp pool excluded-address 192.168.33.1 192.168.33.99
              R1(dhcp-config)#ip dhcp pool excluded-address 192.168.65.1 192.168.65.99
              R1(dhcp-config)#exit

  3.Configurer R2 et R3 en DHCP-Relay. 

              R2(config)#int g0/0
              R2(config-if)#ip helper-address 192.168.1.1

              R3(config)#int g0/0
              R3(config-if)#ip helper-address 192.168.1.1

Étape 4 : Configurer et activer le routage OSPFv2 

  1.Activer le processus OSPFv2 avec l'ID 1. 
  2. Fixer le router-id à sa valeur convenue.
  3.Activer l'interface LAN de chaque routeur en passive-interface. Que signifie cette directive ? N’envoie pas de message OSPF à travers cette interface
  4.Déclarer chaque réseau interne directement connecté avec la commande network dans la zone de Backbone. 
  Commandes qui répondent aux questions 1 à 4

              R1(config)#router ospf 1
              R1(config-router)#router-id 1.1.1.1
              R1(config-router)#passive-interface g0/0
              R1(config-router)#network 192.168.1.1 0.0.0.0 area 0
              R1(config-router)#network 192.168.225.1 0.0.0.0 area 0
              R1(config-router)#network 192.168.226.1 0.0.0.0 area 0

              R2(config)#router ospf 1
              R2(config-router)#router-id 2.2.2.2
              R2(config-router)#passive-interface g0/0
              R2(config-router)#network 192.168.33.1 0.0.0.0 area 0
              R2(config-router)#network 192.168.225.2 0.0.0.0 area 0
              R2(config-router)#network 192.168.227.1 0.0.0.0 area 0 

              R3(config)#router ospf 1
              R3(config-router)#router-id 3.3.3.3
              R3(config-router)#passive-interface g0/0
              R3(config-router)#network 192.168.65.1 0.0.0.0 area 0
              R3(config-router)#network 192.168.227.2 0.0.0.0 area 0
              R3(config-router)#network 192.168.226.2 0.0.0.0 area 0

  5.Veuillez adapter la valeur de référence de la métrique à la norme 1Gbps. 

              R1(config-router)#auto-cost reference-bandwidth 1000
              % OSPF: Reference bandwidth is changed.  

              R2(config-router)#auto-cost reference-bandwidth 1000
              % OSPF: Reference bandwidth is changed. 

              R3(config-router)#auto-cost reference-bandwidth 1000
              % OSPF: Reference bandwidth is changed.
              
              Par défaut, la valeur de référence est fixée à 100 Mbps.
              
  6.Quelle est la commande utilisée pour vérifier la configuration OSPFv2 ? 
  
              Show ip route
              Show ip protocols

  7.Qui est DR et qui est BDR sur chaque liaison entre les routeurs ? 

                R1#sh ip ospf neigh

                Neighbor ID     Pri   State           Dead Time   Address         Interface
                3.3.3.3           1   FULL/BDR        00:00:38    192.168.226.2   GigabitEthernet0/3

                R2#show ip ospf neigh

                Neighbor ID     Pri   State           Dead Time   Address         Interface
                3.3.3.3           1   FULL/BDR        00:00:31    192.168.227.2   GigabitEthernet0/3
                R3#sh ip ospf neigh     

                Neighbor ID     Pri   State           Dead Time   Address         Interface
                1.1.1.1           1   FULL/DR         00:00:39    192.168.226.1   GigabitEthernet0/1
                2.2.2.2           1   FULL/DR         00:00:38    192.168.227.1   GigabitEthernet0/2
                
                D’après les informations obtenues par la commande ci-dessus :
                -	R1 voit uniquement R3 comme BDR
                -	R2 voit uniquement R3 comme BDR
                -	R3 voit R1 et R2 comme DR.
                
  8.Combien de destinations sont-elles à joindre ? Comment trouver ces destinations sur chaque routeur ? 
  
                Utiliser la commande show ip route
                
  9.Vérifier la connectivité IPv4 de bout-en-bout. 
  
              Les adresses devraient démarrer à .100 or si je demande une adresse (ip dhcp) sur pc1, j’obtiens une adresse .2
              PC1> ip dhcp
              DDORA IP 192.168.1.2/24 GW 192.168.1.1

              Toutefois, le Ping entre chaque PC fonctionne bien.


Étape 5 : Activer les interfaces et le routage IPv6 

  1.Configurer l'adresse link-local sur chaque interface interne des routeurs. 
  2.Configurer les adresses ULA sur chaque interface LAN des routeurs. 
  3.Activer le routage IPv6 
                La commande “no shutdown” est inutile ici car elle a déjà été encodée lors de la config. IPv4.

                R1(config)#interface g0/0
                R1(config-if)#ipv6 address fe80::1 link-local
                R1(config-if)#ipv6 address fd00:fd00:fd00:1::1/64
                R1(config-if)#exit
                R1(config)#interface g0/2
                R1(config-if)#ipv6 address fe80::1 link-local
                R1(config-if)#exit
                R1(config)#interface g0/3
                R1(config-if)#ipv6 address fe80::1 link-local
                R1(config-if)#exit
                R1(config)#ipv6 unicast-routing

                R2(config)#interface g0/0
                R2(config-if)#ipv6 address fe80::2 link-local
                R2(config-if)#ipv6 address fd00:fd00:fd00:2::1/64
                R2(config-if)#exit
                R2(config)#interface g0/1
                R2(config-if)#ipv6 address fe80::2 link-local
                R2(config-if)#exit
                R2(config)#interface g0/3
                R2(config-if)#ipv6 address fe80::2 link-local
                R2(config-if)#exit
                R2(config)#ipv6 unicast-routing

                R3(config)#interface g0/0
                R3(config-if)#ipv6 address fe80::3 link-local
                R3(config-if)#ipv6 address fd00:fd00:fd00:3::1/64
                R3(config-if)#exit
                R3(config)#interface g0/2
                R3(config-if)#ipv6 address fe80::3 link-local
                R3(config-if)#exit
                R3(config)#interface g0/1
                R3(config-if)#ipv6 address fe80::3 link-local
                R3(config-if)#exit
                R3(config)#ipv6 unicast-routing


Étape 6 : Configurer et activer le routage OSPFv3 

  1.Activer le processus OSPFv3 avec l'ID 1. 
  
                  R1(config)#ipv6 router ospf 1

                  R2(config)#ipv6 router ospf 1

                  R3(config)#ipv6 router ospf 1
                
  2.Fixer le router-id à sa valeur convenue. 
  
                  R1(config-rtr)#router-id 1.1.1.1

                  R2(config-rtr)#router-id 2.2.2.2

                  R3(config-rtr)#router-id 3.3.3.3

3.Activer l'interface LAN de chaque routeur en passive-interface. Que signifie cette directive ? 

                  R1(config-rtr)#passive-interface g0/0
                  R1(config-rtr)#exit

                  R2(config-rtr)#passive-interface g0/0
                  R2(config-rtr)#exit

                  R3(config-rtr)#passive-interface g0/0
                  R3(config-rtr)#exit
                  
  4. Déclarer chaque réseau interne dans la zone de Backbone.
  
                  R1(config)#int g0/0
                  R1(config-if)#ipv6 ospf 1 area 0
                  R1(config-if)#exit
                  R1(config)#int g0/2          
                  R1(config-if)#ipv6 ospf 1 area 0
                  R1(config-if)#ipv6 ospf priority 255
                  R1(config-if)#exit
                  R1(config)#int g0/3
                  R1(config-if)#ipv6 ospf 1 area 0    
                  R1(config-if)#ipv6 ospf priority 255

                  R2(config)#int g0/0          
                  R2(config-if)#ipv6 ospf 1 area 0
                  R2(config-if)#exit
                  R2(config)#int g0/1        
                  R2(config-if)#ipv6 ospf 1 area 0
                  R2(config-if)#exit
                  R2(config)#int g0/3          
                  R2(config-if)#ipv6 ospf 1 area 0
                  R2(config-if)#exit
                  R2(config)#exit

                  R3(config)#int g0/0
                  R3(config-if)#ipv6 ospf 1 area 0
                  R3(config-if)#exit
                  R3(config)#int g0/1          
                  R3(config-if)#ipv6 ospf 1 area 0
                  R3(config-if)#exit              
                  R3(config)#int g0/2          
                  R3(config-if)#ipv6 ospf 1 area 0
                  R3(config-if)#exit  
                  
  5.Veuillez adapter la valeur de référence de la métrique à la norme 1Gbps. 
  
                  R1(config)#ipv6 router ospf 1
                  R1(config-rtr)#auto-cost reference-bandwidth 1000
                  % OSPFv3-1-IPv6:  Reference bandwidth is changed.

                  R2(config)#ipv6 router ospf 1
                  R2(config-rtr)#auto-cost reference-bandwidth 1000
                  % OSPFv3-1-IPv6:  Reference bandwidth is changed.

                  R3(config)#ipv6 router ospf 1
                  R3(config-rtr)#auto-cost reference-bandwidth 1000
                  % OSPFv3-1-IPv6:  Reference bandwidth is changed.       

  6.Quelle est la commande utilisée pour vérifier la configuration OSPFv3 ? 
  
                  R1#show ipv6 protocols | begin ospf 1
                  IPv6 Routing Protocol is "ospf 1"
                    Router ID 1.1.1.1
                    Number of areas: 1 normal, 0 stub, 0 nssa
                    Interfaces (Area 0):
                      GigabitEthernet0/3
                      GigabitEthernet0/2
                      GigabitEthernet0/0
                    Redistribution:
                    None

                  R2#show ipv6 protocols | begin ospf 1
                  IPv6 Routing Protocol is "ospf 1"
                    Router ID 2.2.2.2
                    Number of areas: 1 normal, 0 stub, 0 nssa
                    Interfaces (Area 0):
                      GigabitEthernet0/3
                      GigabitEthernet0/0
                      GigabitEthernet0/1
                    Redistribution:
                      None

                  R3#show ipv6 protocols | begin ospf 1
                  IPv6 Routing Protocol is "ospf 1"
                    Router ID 3.3.3.3
                    Number of areas: 1 normal, 0 stub, 0 nssa
                    Interfaces (Area 0):
                      GigabitEthernet0/2
                      GigabitEthernet0/1
                      GigabitEthernet0/0
                    Redistribution:
                     None

  7.Qui est DR et qui est BDR sur chaque liaison entre les routeurs ? Faites en sorte que R1 deviennent DR dans tous les cas. 
                    Commande : show ipv6 ospf neigh

                    R1#show ipv6 ospf neigh

                                OSPFv3 Router with ID (1.1.1.1) (Process ID 1)

                    Neighbor ID     Pri   State           Dead Time   Interface ID    Interface
                    3.3.3.3           1   FULL/BDR        00:00:38    3               GigabitEthernet0/3
                    2.2.2.2           1   FULL/BDR        00:00:37    3               GigabitEthernet0/2


                    R2#show ipv6 ospf neigh

                                OSPFv3 Router with ID (2.2.2.2) (Process ID 1)

                    Neighbor ID     Pri   State           Dead Time   Interface ID    Interface
                    3.3.3.3           1   FULL/BDR        00:00:34    4               GigabitEthernet0/3
                    1.1.1.1         255   FULL/DR         00:00:38    4               GigabitEthernet0/1
                    R3#show ipv6 ospf neigh

                                OSPFv3 Router with ID (3.3.3.3) (Process ID 1)

                    Neighbor ID     Pri   State           Dead Time   Interface ID    Interface
                    2.2.2.2           1   FULL/DR         00:00:37    5               GigabitEthernet0/2
                    1.1.1.1         255   FULL/DR         00:00:32    5               GigabitEthernet0/1

                    Comment est-ce possible que R3 voit ses deux voisins (R1 et R2) comme un DR ?
                    R2 voit R1 comme le DR et R3 comme le BDR.
                    R1 voit R2 et R3 comme un BDR.
                    
  8.Combien de destinations sont-elles à joindre ? Comment trouver ces destinations sur chaque routeur ? 
  
                    Commande : show ipv6 route

                    R1#show ipv6 route
                    C   FD00:FD00:FD00:1::/64 [0/0]
                         via GigabitEthernet0/0, directly connected
                    L   FD00:FD00:FD00:1::/128 [0/0]
                         via GigabitEthernet0/0, receive
                    L   FD00:FD00:FD00:1::1/128 [0/0]
                         via GigabitEthernet0/0, receive
                    O   FD00:FD00:FD00:2::/64 [110/2]
                         via FE80::2, GigabitEthernet0/2
                    O   FD00:FD00:FD00:3::/64 [110/2]
                         via FE80::3, GigabitEthernet0/3
                    L   FF00::/8 [0/0]
                      via Null0, receive

                    R2#show ipv6 route
                    O   FD00:FD00:FD00:1::/64 [110/2]
                         via FE80::1, GigabitEthernet0/1
                    C   FD00:FD00:FD00:2::/64 [0/0]
                         via GigabitEthernet0/0, directly connected
                    L   FD00:FD00:FD00:2::1/128 [0/0]
                         via GigabitEthernet0/0, receive
                    O   FD00:FD00:FD00:3::/64 [110/2]
                         via FE80::3, GigabitEthernet0/3
                    L   FF00::/8 [0/0]
                      via Null0, receive


                    R3#show ipv6 route
                    O   FD00:FD00:FD00:1::/64 [110/2]
                         via FE80::1, GigabitEthernet0/1
                    O   FD00:FD00:FD00:2::/64 [110/2]
                         via FE80::2, GigabitEthernet0/2
                    C   FD00:FD00:FD00:3::/64 [0/0]
                         via GigabitEthernet0/0, directly connected
                    L   FD00:FD00:FD00:3::1/128 [0/0]
                         via GigabitEthernet0/0, receive
                    L   FF00::/8 [0/0]
                         via Null0, receive
                         
  9.Vérifier la connectivité IPv6 de bout-en-bout.
  
                    Pour tester la connectivité ipv6 de bout en bout, on récupère l’adresse de PC3 puis on ping cette adresse       sur PC1. On obtient bien une réponse : PC1 et PC3 communiquent. On fait de même entre tous les PC (on obtient une réponse dans tous les cas testés : tous les PC communiquent entre eux).
                  PC3> show ipv6

                  NAME              : PC3[1]
                  LINK-LOCAL SCOPE  : fe80::250:79ff:fe66:6802/64
                  GLOBAL SCOPE      : fd00:fd00:fd00:3:2050:79ff:fe66:6802/64
                  ROUTER LINK-LAYER : 0c:d0:f2:a4:b2:00
                  MAC               : 00:50:79:66:68:02
                  LPORT             : 20210
                  RHOST:PORT        : 127.0.0.1:20211
                  MTU:              : 1500


                  PC1> ping fd00:fd00:fd00:3:2050:79ff:fe66:6802

                  fd00:fd00:fd00:3:2050:79ff:fe66:6802 icmp6_seq=1 ttl=60 time=12.204 ms
                  fd00:fd00:fd00:3:2050:79ff:fe66:6802 icmp6_seq=2 ttl=60 time=1.681 ms
                  fd00:fd00:fd00:3:2050:79ff:fe66:6802 icmp6_seq=3 ttl=60 time=1.964 ms
                  fd00:fd00:fd00:3:2050:79ff:fe66:6802 icmp6_seq=4 ttl=60 time=2.144 ms
                  fd00:fd00:fd00:3:2050:79ff:fe66:6802 icmp6_seq=5 ttl=60 time=1.540 ms


Étape 7 : Activer et configurer de la connectivité IPv4 publique 

  1.Activer l'interface externe de R1. 
  
                    R1(config)#interface g0/1
                    R1(config-if)#ip address dhcp
                    R1(config-if)#no shutdown
                    R1(config-if)#exit

  2.Activer le NAT pour tous les LANs seulement. 
  
                    R1(config)#access-list 1 permit 192.168.1.0 0.0.0.255
                    R1(config)#access-list 1 permit 192.168.33.0 0.0.0.255
                    R1(config)#access-list 1 permit 192.168.65.0 0.0.0.255
                    R1(config)#ip nat inside source list 1 interface g0/1 overload
                    R1(config)#interface g0/0
                    R1(config-if)#ip nat inside
                    R1(config-if)#exit
                    R1(config)#interface g0/2
                    R1(config-if)#ip nat inside
                    R1(config-if)#exit
                    R1(config)#interface g0/3
                    R1(config-if)#ip nat inside 
                    R1(config-if)#exit
                    R1(config)#interface g0/1
                    R1(config-if)#ip nat outside
                    R1(config-if)#exit
                    R1(config)#router ospf 1
                    R1(config-router)#default-information originate
                    R1(config-router)#end
                    
  3.Vérifier la propagation de la route par défaut auprès de R2 et de R3 

  4.Vérifier la connectivité des LAN vers l'Internet public (L3/L7). 
  
Étape 8 : Migrer vers EIGRP pour IPv4 et IPv6 

  1.Activer le routage EIGRP dans l'AD "1". 
  2.Activer EIGRP pour IPv4 et pour IPv6 sur chaque interface avec le router-id et la passive-interface appropriés. 
  
                    R1(config)#router eigrp 1
                    R1(config-router)#passive-interface g0/0
                    R1(config-router)#eigrp router-id 1.1.1.1
                    R1(config-router)#network 192.168.1.0
                    R1(config-router)#network 192.168.225.0
                    R1(config-router)#network 192.168.226.0
                    R1(config-router)#redistribute static
                    R1(config-router)#exit
                    R1(config)#ipv6 router eigrp 1
                    R1(config-rtr)#passive-interface g0/0
                    R1(config-rtr)#eigrp router-id 1.1.1.1
                    R1(config-rtr)#exit
                    R1(config)#interface g0/0
                    R1(config-if)#ipv6 eigrp 1
                    R1(config-if)#exit
                    R1(config)#interface g0/2
                    R1(config-if)#ipv6 eigrp 1  
                    R1(config-if)#exit          
                    R1(config)#interface g0/3
                    R1(config-if)#ipv6 eigrp 1


                    R2(config)#router eigrp 1
                    R2(config-router)#passive-interface g0/0
                    R2(config-router)#eigrp router-id 2.2.2.2
                    R2(config-router)#network 192.168.33.0
                    R2(config-router)#network 192.168.225.0
                    R2(config-router)#network 192.168.227.0
                    R2(config-router)#exit
                    R2(config)#ipv6 router eigrp 1
                    R2(config-rtr)#passive-interface g0/0
                    R2(config-rtr)#eigrp router-id 2.2.2.2
                    R2(config-rtr)#exit
                    R2(config)#int g0/0
                    R2(config-if)#ipv6 eigrp 1
                    R2(config-if)#exit
                    R2(config)#int g0/1
                    R2(config-if)#ipv6 eigrp 1
                    R2(config-if)#exit
                    R2(config-if)#exit
                    R2(config)#int g0/3
                    R2(config-if)#ipv6 eigrp 1
                    R2(config-if)#end


                    R3(config)#router eigrp 1
                    R3(config-router)#passive-interface g0/0
                    R3(config-router)#eigrp router-id 3.3.3.3
                    R3(config-router)#network 192.168.65.0
                    R3(config-router)#network 192.168.226.0
                    R3(config-router)#network 192.168.226.0
                    R3(config-router)#network 192.168.227.0
                    R3(config-router)#exit
                    R3(config)#ipv6 router eigrp 1
                    R3(config-rtr)#passive-interface g0/0
                    R3(config-rtr)#eigrp router-id 3.3.3.3
                    R3(config-rtr)#exit
                    R3(config)#int g0/0
                    R3(config-if)#ipv6 eigrp 1
                    R3(config-if)#exit
                    R3(config)#int g0/1
                    R3(config-if)#ipv6 eigrp 1
                    R3(config-if)#exit
                    R3(config)#int g0/2
                    R3(config-if)#ipv6 eigrp 1
                    R3(config-if)#exit

  3.Vérifier les tables de routage. 
  
                      Commandes : show ip route et show ipv6 route

                      R1#show ip route

                      S*    0.0.0.0/0 [254/0] via 192.168.122.1
                            192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
                      C        192.168.1.0/24 is directly connected, GigabitEthernet0/0
                      L        192.168.1.1/32 is directly connected, GigabitEthernet0/0
                      D     192.168.33.0/24 
                                 [90/3328] via 192.168.226.2, 00:06:48, GigabitEthernet0/3
                      D     192.168.65.0/24 
                                 [90/3072] via 192.168.226.2, 00:06:51, GigabitEthernet0/3
                            192.168.122.0/24 is variably subnetted, 2 subnets, 2 masks
                      C        192.168.122.0/24 is directly connected, GigabitEthernet0/1
                      L        192.168.122.188/32 is directly connected, GigabitEthernet0/1
                            192.168.225.0/24 is variably subnetted, 2 subnets, 2 masks
                      C        192.168.225.0/24 is directly connected, GigabitEthernet0/2
                      L        192.168.225.1/32 is directly connected, GigabitEthernet0/2
                            192.168.226.0/24 is variably subnetted, 2 subnets, 2 masks
                      C        192.168.226.0/24 is directly connected, GigabitEthernet0/3
                      L        192.168.226.1/32 is directly connected, GigabitEthernet0/3
                      D     192.168.227.0/24 
                        [90/3072] via 192.168.226.2, 00:06:48, GigabitEthernet0/3

                      R1#show ipv6 route
                      C   FD00:FD00:FD00:1::/64 [0/0]
                           via GigabitEthernet0/0, directly connected
                      L   FD00:FD00:FD00:1::/128 [0/0]
                           via GigabitEthernet0/0, receive
                      L   FD00:FD00:FD00:1::1/128 [0/0]
                           via GigabitEthernet0/0, receive
                      D   FD00:FD00:FD00:2::/64 [90/3072]
                           via FE80::2, GigabitEthernet0/2
                      D   FD00:FD00:FD00:3::/64 [90/3072]
                           via FE80::3, GigabitEthernet0/3
                      L   FF00::/8 [0/0]
                        via Null0, receive
                      Les routes EIGRP (« D ») sont bien inscrites à la table de routage.

  4.Vérifier la connectivité IPv4 et IPv6 de bout-en-bout. 
  
                      Ping entre les PC.
                      
  5.Désactiver OSPFv2 et OSPFv3 sur chaque routeur. 
  
            R1(config)#no router ospf 1
            R1(config)#no ipv6 router ospf 1

            R2(config)#no router ospf 1
            R2(config)#no ipv6 router ospf 1

            R3(config)#no router ospf 1
            R3(config)#no ipv6 router ospf 1

            En encodant la commande show ip route (et show ipv6 route), on vérifie que les routes OSPF ont bien disparu.

            De la même façon, en encodant la commande show ip protocols (et show ipv6 protocols), on s’assure que le protocole maintenant en vigueur est EIGRP.
            Exemple sur R2 :
            
            R2#show ip protocols
            *** IP Routing is NSF aware ***

            Routing Protocol is "application"
              Sending updates every 0 seconds
              Invalid after 0 seconds, hold down 0, flushed after 0
              Outgoing update filter list for all interfaces is not set
              Incoming update filter list for all interfaces is not set
              Maximum path: 32
              Routing for Networks:
              Routing Information Sources:
                Gateway         Distance      Last Update
              Distance: (default is 4)

            Routing Protocol is "eigrp 1"
              Outgoing update filter list for all interfaces is not set
              Incoming update filter list for all interfaces is not set
              Default networks flagged in outgoing updates
              Default networks accepted from incoming updates
              EIGRP-IPv4 Protocol for AS(1)
                Metric weight K1=1, K2=0, K3=1, K4=0, K5=0
                Soft SIA disabled
                NSF-aware route hold timer is 240
                Router-ID: 2.2.2.2
                Topology : 0 (base) 
                  Active Timer: 3 min
                  Distance: internal 90 external 170
                  Maximum path: 4
                  Maximum hopcount 100
                  Maximum metric variance 1
