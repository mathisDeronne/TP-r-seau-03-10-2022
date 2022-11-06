# TP3 : On va router des trucs
## I. ARP
### 1. Echange ARP

🌞**Générer des requêtes ARP**
```
-[user@localhost ~]$ ping 10.3.1.12
PING 10.3.1.12 (10.3.1.12) 56(84) bytes of data.
64 bytes from 10.3.1.12: icmp_seq=1 ttl=64 time=0.373 ms
64 bytes from 10.3.1.12: icmp_seq=2 ttl=64 time=0.368 ms
64 bytes from 10.3.1.12: icmp_seq=3 ttl=64 time=0.298 ms
64 bytes from 10.3.1.12: icmp_seq=4 ttl=64 time=0.488 ms
64 bytes from 10.3.1.12: icmp_seq=5 ttl=64 time=1.24 ms
64 bytes from 10.3.1.12: icmp_seq=6 ttl=64 time=0.368 ms
64 bytes from 10.3.1.12: icmp_seq=7 ttl=64 time=0.394 ms
64 bytes from 10.3.1.12: icmp_seq=8 ttl=64 time=0.506 ms
64 bytes from 10.3.1.12: icmp_seq=9 ttl=64 time=0.353 ms
64 bytes from 10.3.1.12: icmp_seq=10 ttl=64 time=0.358 ms

--- 10.3.1.12 ping statistics ---
10 packets transmitted, 10 received, 0% packet loss, time 9239ms
rtt min/avg/max/mdev = 0.298/0.474/1.239/0.261 ms
- pc John:
```
```
[user@localhost ~]$ ip neigh show
10.3.1.12 dev enp0s3 lladdr 08:00:27:22:76:0c STALE
10.3.1.1 dev enp0s3 lladdr 0a:00:27:00:00:34 REACHABLE
```
- **pc Marcel**
```
[user@localhost ~]$ ip neigh show
10.3.1.1 dev enp0s3 lladdr 0a:00:27:00:00:34 REACHABLE
10.3.1.11 dev enp0s3 lladdr 08:00:27:41:37:67 STALE
```
- **mac de Marcel**
sur la machine de Jhon : 08:00:27:22:76:0c
sur la machine de Marcel : 08:00:27:22:76:0c
- **mac de John** 
sur la machine de Marcel : 08:00:27:41:37:67
sur la machine de Jhon : 08:00:27:41:37:67

  - **pour voir la mac de Marcel dans le pc de Jhon :** 
  "ip neigh show"
  - **pour voir la mac de Marcel depuis le pc de Marcel :** 
  "ip a" 

### 2. Analyse de trames

🌞**Analyse de trames**

[Analyse de trames](./toto.pcap)

## II. Routage

| Machine  | `10.3.1.0/24` | `10.3.2.0/24` |
|----------|---------------|---------------|
| `router` | `10.3.1.254`  | `10.3.2.254`  |
| `john`   | `10.3.1.11`   | no            |
| `marcel` | no            | `10.3.2.12`   |
### 1. Mise en place du routage

🌞**Activer le routage sur le noeud `router`**
```
[user@localhost ~]$ sudo firewall-cmd --add-masquerade --zone=public --permanent
[sudo] password for user:
success
```

🌞**Ajouter les routes statiques nécessaires pour que `john` et `marcel` puissent se `ping`**

- **depuis le PC de John :**
``[user@localhost ~]$ sudo ip route add 10.3.2.0/24 via 10.3.1.254 dev enp0s3 ``
- **depuis le PC de Marcel :**
``[user@localhost ~]$ sudo ip route add 10.3.1.0/24 via 10.3.2.254 dev enp0s3``
- **Ping depuis le PC de John :** 
```
[user@localhost ~]$ ping 10.3.2.12
PING 10.3.2.12 (10.3.2.12) 56(84) bytes of data.
64 bytes from 10.3.2.12: icmp_seq=1 ttl=63 time=0.540 ms
64 bytes from 10.3.2.12: icmp_seq=2 ttl=63 time=0.951 ms
64 bytes from 10.3.2.12: icmp_seq=3 ttl=63 time=0.655 ms
64 bytes from 10.3.2.12: icmp_seq=4 ttl=63 time=0.919 ms
64 bytes from 10.3.2.12: icmp_seq=5 ttl=63 time=0.693 ms
64 bytes from 10.3.2.12: icmp_seq=6 ttl=63 time=0.652 ms
64 bytes from 10.3.2.12: icmp_seq=7 ttl=63 time=0.810 ms
64 bytes from 10.3.2.12: icmp_seq=8 ttl=63 time=0.896 ms
64 bytes from 10.3.2.12: icmp_seq=9 ttl=63 time=0.706 ms
64 bytes from 10.3.2.12: icmp_seq=10 ttl=63 time=0.723 ms
64 bytes from 10.3.2.12: icmp_seq=11 ttl=63 time=2.20 ms
64 bytes from 10.3.2.12: icmp_seq=12 ttl=63 time=0.513 ms
64 bytes from 10.3.2.12: icmp_seq=13 ttl=63 time=0.697 ms
^C
--- 10.3.2.12 ping statistics ---
13 packets transmitted, 13 received, 0% packet loss, time 12303ms
rtt min/avg/max/mdev = 0.513/0.842/2.198/0.412 ms
```
### 2. Analyse de trames

🌞**Analyse des échanges ARP**

- **Depuis le PC de John :** 
```
[user@localhost ~]$ sudo ip neigh flush all
[sudo] password for user:
[user@localhost ~]$ ping 10.3.2.12
PING 10.3.2.12 (10.3.2.12) 56(84) bytes of data.
64 bytes from 10.3.2.12: icmp_seq=1 ttl=63 time=0.918 ms
64 bytes from 10.3.2.12: icmp_seq=2 ttl=63 time=0.639 ms
64 bytes from 10.3.2.12: icmp_seq=3 ttl=63 time=0.705 ms
64 bytes from 10.3.2.12: icmp_seq=4 ttl=63 time=0.756 ms
64 bytes from 10.3.2.12: icmp_seq=5 ttl=63 time=0.639 ms
^C
--- 10.3.2.12 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4125ms
rtt min/avg/max/mdev = 0.639/0.731/0.918/0.103 ms
```
- **Table ARP de John :** 
```
[user@localhost ~]$ ip neigh show
10.3.1.1 dev enp0s3 lladdr 0a:00:27:00:00:0d REACHABLE
10.3.1.254 dev enp0s3 lladdr 08:00:27:05:6d:73 STALE
```
- **Table ARP de Marcel :**
```
[user@localhost ~]$ ip neigh show
10.3.2.1 dev enp0s3 lladdr 0a:00:27:00:00:15 DELAY
10.3.2.254 dev enp0s3 lladdr 08:00:27:dc:22:d1 STALE
```
- **Table ARP du Router**
```
[user@localhost ~]$ ip neigh show
10.3.1.11 dev enp0s8 lladdr 08:00:27:7e:f2:d8 STALE
10.3.2.1 dev enp0s9 lladdr 0a:00:27:00:00:15 DELAY
10.3.2.12 dev enp0s9 lladdr 08:00:27:22:76:0c STALE
```
- **essayez de déduire un peu les échanges ARP qui ont eu lieu :** 
On peut voir dans les tables ARP de John et Marcel qu'ils n'ont pas vraiment communiqué entre eux; Ils ont chacun envoyé le ping et le pong a leur passerelle (10.3.1.254 pour John et 10.3.2.254 pour Marcel) et la passerelle a transmis les informations au PC destinataire
**sens du ping (idem pour le pong mais dans l'autre sens):**
```
John(10.3.1.11) ---> Carte 1 routeur(10.3.1.254) ---> Carte 2 routeur(10.3.2.254) ---> Marcel(10.3.2.12)
``` 
- répétez l'opération précédente (vider les tables, puis `ping`), en lançant `tcpdump` sur `marcel
```
[user@localhost ~]$ sudo ip neigh flush all
[user@localhost ~]$ ping 10.3.2.12
PING 10.3.2.12 (10.3.2.12) 56(84) bytes of data.
64 bytes from 10.3.2.12: icmp_seq=1 ttl=63 time=1.15 ms
64 bytes from 10.3.2.12: icmp_seq=2 ttl=63 time=0.631 ms
64 bytes from 10.3.2.12: icmp_seq=3 ttl=63 time=2.18 ms
64 bytes from 10.3.2.12: icmp_seq=4 ttl=63 time=0.998 ms
^C
--- 10.3.2.12 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3033ms
rtt min/avg/max/mdev = 0.631/1.240/2.177/0.573 ms
```
[capture de trame](./capture_de_trames_2.pcap)

| ordre | type trame  | IP source | MAC source              | IP destination | MAC destination            |
|-------|-------------|-----------|-------------------------|----------------|----------------------------|
| 1     | Requête ARP | 10.3.1.11        | `Router` `fe80::a00:27ff:fedc:22d1` | broadcast              | Broadcast `FF:FF:FF:FF:FF` |
| 2     | Réponse ARP | 10.3.2.12         | `marcel` `fe80::a00:27ff:fe22:760c`                       | 10.3.1.11              | `Router` `fe80::a00:27ff:fedc:22d1`    |
| 3     | Ping        | 10.3.2.254         | `Router` `fe80::a00:27ff:fedc:22d1`                       | 10.3.2.12              | `Marcel` `fe80::a00:27ff:fe22:760c`                          |
| 4     | Pong        | 10.3.2.12         | `marcel` `fe80::a00:27ff:fe22:760c`                       | 10.3.2.254              | `Router` `fe80::a00:27ff:fedc:22d1`                          |

### 3. Accès internet

🌞**Donnez un accès internet à vos machines**
```
[user@localhost ~]$ sudo ip route add default via 10.3.1.254 dev enp0s3
[sudo] password for user:
[user@localhost ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=112 time=15.0 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=112 time=15.2 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=112 time=15.5 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=112 time=15.4 ms
64 bytes from 8.8.8.8: icmp_seq=5 ttl=112 time=15.1 ms
^C
--- 8.8.8.8 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4007ms
rtt min/avg/max/mdev = 15.026/15.230/15.473/0.162 ms
```
```
[user@localhost ~]$ dig gitlab.com

; <<>> DiG 9.16.23-RH <<>> gitlab.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 53486
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;gitlab.com.                    IN      A

;; ANSWER SECTION:
gitlab.com.             300     IN      A       172.65.251.78

;; Query time: 28 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Sun Nov 06 13:43:55 CET 2022
;; MSG SIZE  rcvd: 55

[user@localhost ~]$ ping gitlab.com
PING gitlab.com (172.65.251.78) 56(84) bytes of data.
64 bytes from 172.65.251.78 (172.65.251.78): icmp_seq=1 ttl=52 time=15.2 ms
64 bytes from 172.65.251.78 (172.65.251.78): icmp_seq=2 ttl=52 time=15.2 ms
^C
--- gitlab.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 15.164/15.181/15.198/0.017 ms
```

🌞**Analyse de trames**

- effectuez un `ping 8.8.8.8` depuis `john`
- capturez le ping depuis `john` avec `tcpdump`

[Analyse de trames](./capture_de_trames_3.pcap)

- analysez un ping aller et le retour qui correspond et mettez dans un tableau :

| ordre | type trame | IP source          | MAC source              | IP destination | MAC destination |     |
|-------|------------|--------------------|-------------------------|----------------|-----------------|-----|
| 1     | ping       | `John` `10.3.1.11` | `John` `fe80::a00:27ff:fe7e:f2d8` | `8.8.8.8`      | ?               |     |
| 2     | pong       | `8.8.8.8`                | ?                   | `John` `10.3.2.11`            | `John` `fe80::a00:27ff:fe7e:f2d8`             |