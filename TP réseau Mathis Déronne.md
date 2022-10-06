# TP réseau
## Partie 1
### 1.Commande ipconfig
le résultat affiché est  10.33.16.96
### 2. Commande ipconfig /all
#### 2.1. Adresse réseau
le résultat est 10.33.16.0
#### 2.2. Adresse de diffusion (broadcast)
le résultat est 10.33.19.255
#### 2.3. Masque de sous réseau
le résultat est 255.255.252.0
#### 2.4. Adresse loopback
le résultat est 127.0.0.1
#### 2.5. Passerelle réseau Ynov
le résultat est  10.33.19.254
### 3. Commande nslookup
#### 3.1. adresse obtenue
le résultat est  8.8.8.8
#### 3.2. Interrogation 8.8.8.8
le résultat obtenu est dns.google
#### 3.3. Interrogation 1.1.1.1
le résultat obtenu est one.one.one.one
## Partie 2
### 1. Commande ip a
#### 1.1. Adresse ip
les résultats sont 192.168.5.128 et 192.168.71.129
#### 1.2. Adresse loopback
le résultat est 127.0.0.1
### 2. Commande cat /etc/hostname
la réponse est tpcr-client1
### 3. Ping
#### 3.2 Ping adresse client 2 depuis client 1
la réponse est PING 192.168.5.130
#### 3. Ping adresse client 1 et 2 depuis serveur
##### 3.1.1. Ping Client 1
la réponse est PING 192.168.71.130
##### 3.1.2. Ping Client 2
la réponse est PING 192.168.5.130
### 4. Utilisation d'un des deux comme gateway
#### Commande nslookup www.ynov.com
les réponses sont 
172.67.74.226
104.26.10.233
104.26.11.233
### 5.Petit chat privé
🌞 **sur le PC *serveur*** avec par exemple l'IP 192.168.1.1

C:\Users\Bayle\netcat-1.11> .\nc.exe -l -p 8888

🌞 **sur le PC *client*** avec par exemple l'IP 192.168.1.2

"C:\Users\mathi\TP-réseau-03-10-2022\netcat-win32-1.11\netcat-1.11>.\nc.exe 192.168.1.1 8888
gh
coucou
ça fonctionne
wa c tro bien
uiiiiiiiiiiiii
coucou"

🌞 **Visualiser la connexion en cours**

"netstat -a -n -b | Select-String 8888

  TCP    192.168.1.1:8888       0.0.0.0:0              LISTENING
TCP    192.168.1.1:8888       192.168.1.2:55861      ESTABLISHED
 [nc.exe]"

🌞 **Pour aller un peu plus loin**

"netstat -a -n -b | Select-String 8888

  TCP    192.168.1.1:8888       0.0.0.0:0              LISTENING"

## 6. Firewall

🌞 **Activez et configurez votre firewall**

touche windows -> pare-feu windows defender -> "regles de trafic entrant" -> "nouvelle regle" -> "suivant" -> "suivant" -> "autoriser la connection"; "suivant" -> cocher "domaine","privé" et "public"; "suivant" -> nom "ping" description "autoriser les pings de 192.168.1.1"; "terminer"

### ping

ping 192.168.1.1

Envoi d’une requête 'Ping'  192.168.1.1 avec 32 octets de données :
Réponse de 192.168.1.1 : octets=32 temps<1ms TTL=128
Réponse de 192.168.1.1 : octets=32 temps<1ms TTL=128
Réponse de 192.168.1.1 : octets=32 temps<1ms TTL=128
Réponse de 192.168.1.1 : octets=32 temps<1ms TTL=128

Statistiques Ping pour 192.168.1.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
    
## III. Manipulations d'autres outils/protocoles côté client

### 1. DHCP
🌞Exploration du DHCP, depuis votre PC
 Serveur DHCP : 10.33.19.254

 La date d'expiration: vendredi 7 octobre 2022 8:50:33


### 2. DNS
🌞 Trouver l'adresse IP du serveur DNS que connaît votre ordinateur
Serveurs DNS : 8.8.8.8

🌞 Utiliser, en ligne de commande l'outil nslookup (Windows, MacOS) ou dig (GNU/Linux, MacOS) pour faire des requêtes DNS à la main

google.com:

 Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    google.com
Addresses:  2a00:1450:4007:80e::200e
          216.58.214.174

Ynov.com:

Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    ynov.com
Addresses:  2606:4700:20::681a:ae9
          2606:4700:20::ac43:4ae2
          2606:4700:20::681a:be9
          104.26.11.233
          104.26.10.233
          172.67.74.226

Pour l'adresse 231.34.113.12:

          Address:  8.8.8.8

          Serveur :   dns.google

Pour l'adresse 78.34.2.17:

Address:  8.8.8.8

Serveur :   dns.google
Address:  8.8.8.8

Nom :    cable-78-34-2-17.nc.de
Address:  78.34.2.17

# IV. Wireshark
🌞 Utilisez le pour observer les trames qui circulent entre vos deux carte Ethernet. Mettez en évidence :

![](https://i.imgur.com/fZqIhEf.png)
