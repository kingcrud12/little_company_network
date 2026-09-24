# Consignes : réseau d'une entreprise (2 VLANs)

[← Retour au README](README.md)

> Cahier des charges reconstitué à partir de la réalisation finale du lab.

## Contexte

Une petite entreprise veut un réseau qui sépare ses **postes internes** du **Wi‑Fi mis à disposition des invités**. Les invités doivent pouvoir sortir vers l'extérieur, mais sans jamais accéder aux ressources internes (serveur, postes).

Le réseau est à réaliser sous **Cisco Packet Tracer**.

## Matériel imposé

| Quantité | Équipement                | Rôle                                           |
| -------- | ------------------------- | ---------------------------------------------- |
| 1        | Routeur Cisco 2911        | Routeur de bordure (sortie vers l'extérieur)   |
| 1        | Pare-feu Cisco ASA 5506-X | Filtrage entre les zones, passerelle des VLANs |
| 1        | Switch Cisco 2960-24TT    | Switch d'accès, porte les VLANs                |
| 1        | Serveur                   | Services internes (DHCP, web, mail, fichiers…) |
| 2        | Points d'accès Wi‑Fi      | Un par VLAN                                    |
| 5        | Ordinateurs portables     | 3 internes, 2 invités                          |

## Travail demandé

### 1. Plan d'adressage

À partir du réseau **`192.168.1.0/24`**, découper les sous-réseaux en **VLSM** :

- **VLAN 10 : Interne**, jusqu'à 60 hôtes, soit un **/26**
- **VLAN 20 : Wi‑Fi invités**, jusqu'à 30 hôtes, soit un **/27**
- **Lien de transit** entre l'ASA et le routeur, en **/30** (`10.0.0.0/30`)

Documenter pour chaque sous-réseau : l'adresse réseau, le masque, la passerelle et la plage utilisable.

### 2. Switch

- Créer les **VLAN 10** et **VLAN 20**
- Mettre en mode **access**, dans le bon VLAN, les ports des points d'accès, du serveur et des deux interfaces de l'ASA
- Mettre une **description** sur chaque port utilisé

### 3. Pare-feu ASA

- Créer trois interfaces nommées (`nameif`) avec leur niveau de sécurité :
  - `INTERNE` : security-level **100**
  - `WIFI-GUEST` : security-level **50**
  - `OUTSIDE` : security-level **0**
- Configurer une **route par défaut** vers le routeur
- Activer un **serveur DHCP** pour le VLAN invités, avec un DNS public
- Vérifier que les invités **ne peuvent pas** joindre le réseau interne

### 4. Routeur

- Adresser l'interface côté ASA dans le /30
- Ajouter des **routes statiques** vers les deux sous-réseaux internes, via l'ASA

### 5. Serveur

- Adresse IP **fixe** dans le VLAN 10
- Activer le **DHCP** pour le VLAN interne (passerelle = ASA)

### 6. Wi‑Fi

- Un SSID par VLAN : **`INTERNE`** et **`WIFI-GUEST`**
- Sécurité **WPA2-PSK**
- Connecter les 3 portables internes au SSID `INTERNE` et les 2 portables invités au SSID `WIFI-GUEST`

## Validation attendue

- [ ] Chaque portable obtient une adresse IP dans le bon sous-réseau
- [ ] Les postes internes joignent leur passerelle et le serveur
- [ ] Les invités joignent leur passerelle
- [ ] Les invités **ne joignent pas** le serveur interne (blocage par l'ASA)
- [ ] `show vlan brief` sur le switch montre les ports dans les bons VLANs

## Livrables

- Le fichier Packet Tracer `.pkt`
- Une capture de la topologie
- Un README qui documente l'adressage, les configurations et les tests
