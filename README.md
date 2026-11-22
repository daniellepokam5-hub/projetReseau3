# projetReseau3
Projet 3 : DHCP Relay et pools multiples

Pitch : un seul serveur distribue les adresses à plusieurs réseaux distants.

1. Problème : les requêtes DHCP sont des broadcasts, et un routeur ne transmet pas les broadcasts. Un serveur placé dans un autre réseau ne les reçoit donc jamais.

2. Solution : ip helper-address. Le routeur capte la requête sur l’interface du LAN et la retransmet en unicast au serveur. Il y ajoute l’adresse de son interface (champ giaddr), ce qui permet au serveur de choisir le bon pool.

3. Topologie

Interface	Réseau
g0/0	Segment du serveur
g0/1	LAN 1
g0/2	LAN 2

4. Étapes

Interfaces du routeur : adresse sur g0/0, g0/1, g0/2, avec no shutdown.
Serveur : IP statique, passerelle = IP de g0/0. Elle est indispensable pour que le serveur puisse répondre aux réseaux distants.
Pools : un pool par LAN, chacun avec la passerelle du LAN concerné (IP de g0/1 ou g0/2).
Relais :
interface g0/1
 ip helper-address <IP_SERVEUR>
interface g0/2
 ip helper-address <IP_SERVEUR>
Clients : PC des deux LAN en DHCP.

5. Vérification et résultat
   <img width="1092" height="702" alt="image" src="https://github.com/user-attachments/assets/aac75c8d-500f-4c09-901a-6b7f3bb37765" />


Chaque LAN reçoit une adresse de son propre pool.
ping entre un PC du LAN 1 et un PC du LAN 2 (routage inter-réseaux).
show running-config interface g0/1 pour contrôler le helper-address.

6. Erreurs fréquentes : helper-address oublié sur un LAN ; passerelle du serveur absente ; passerelle de pool erronée ; mauvaise adresse de serveur dans le helper-address.

7. Compétences : DHCP Relay, routage inter-réseaux, architecture multi-LAN, dépannage.

8. Dépôt parapluie : synthèse
<img width="717" height="177" alt="image" src="https://github.com/user-attachments/assets/9b280f8a-4b01-4ce0-8eea-231687d0ed07" />

Contexte : automatiser l’adressage IP d’un réseau d’entreprise, de la plus simple à la plus réaliste des architectures.
Ce que j’ai appris : concepts DORA, broadcast vs unicast, rôle des passerelles, dépannage par ping et show.
Pour aller plus loin (idées, à ne mentionner comme réalisées que si vous les faites) : VLANs, DHCP snooping, serveur DNS, ACL.
