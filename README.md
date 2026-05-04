# 🧪 CyberLab — Analyse réseau (VMware, Kali Linux, Metasploitable2, Nmap, Wireshark)

> Laboratoire **isolé** sous **VMware** pour analyser un réseau, identifier les services exposés sur une machine vulnérable et **observer le trafic** généré par des scans avec **Wireshark**.

---

## 🎯 Objectifs

- Créer un environnement de test **sécurisé et isolé**
- Réaliser des scans **Nmap** (découverte, services/versions, scripts NSE)
- Observer et comprendre le trafic réseau généré par un scan (ARP/TCP) avec **Wireshark**
- Identifier les services exposés et **évaluer les risques** associés
- Produire un mini **rapport d’analyse** (résultats + recommandations)

---

## 🧰 Technologies & outils

- **VMware Workstation / Player**
- **Kali Linux** (machine d’audit)
- **Metasploitable2** (machine cible vulnérable)
- **Nmap**
- **Wireshark**

---

## 🗺️ Architecture / Environnement

### 🔧 Machines
| Rôle | Nom (exemple) | OS | Description |
|---|---|---|---|
| Attaquant / Audit | `KALI01` | Kali Linux | scans, collecte, analyse |
| Cible vulnérable | `META01` | Metasploitable2 | services exposés pour apprentissage |

### 🌐 Réseau (isolé)
- Type : **Host‑Only**
- Nom : `VMnet1`
- Objectif : isolation totale (aucun scan accidentel vers le réseau réel)

---

## 📌 Plan d’adressage IP (exemple)

> Adressage volontairement simple sur un /24, typique d’un lab.  

| Équipement | Interface | IP | Masque | Passerelle | DNS |
|---|---|---|---|---|---|
| `KALI01` (Kali) | Host‑Only | `192.168.56.10` | `255.255.255.0` | *(aucune)* | *(aucun / optionnel)* |
| `META01` (Metasploitable2) | Host‑Only | `192.168.56.20` | `255.255.255.0` | *(aucune)* | *(aucun / optionnel)* |
| VMware Host‑Only (VMnet1) | Host | `192.168.56.1` | `255.255.255.0` | — | — |


---

## ✅ Mise en place (résumé des étapes)

### 1) Installation et configuration des VM
- Import/installation de **Kali** et **Metasploitable2**
- Connexion des deux VMs sur **Host‑Only (VMnet1)**
- Configuration IP (statique ou DHCP Host‑Only)
- Vérification connectivité :
  - `ping 192.168.56.20` depuis Kali
  - `arp -a` pour vérifier la résolution MAC

### 2) Reconnaissance & scans Nmap
Commandes (IP cible: `192.168.56.20`) :
```bash
# Découverte des ports (SYN scan)
nmap -sS -T3 192.168.56.20

# Détection des services + versions
nmap -sS -sV -T3 192.168.56.20

# Scripts NSE par défaut (souvent "safe" mais à utiliser en lab)
nmap -sC -sV 192.168.56.20

# Scan "agressif" (plus verbeux) - lab uniquement
nmap -A 192.168.56.20
```

### 3) Capture et analyse Wireshark
- Lancer Wireshark sur l’interface Host‑Only de Kali
- Filtre utiles :
  - `arp`
  - `tcp`
  - `ip.addr == 192.168.56.20`
- Observer :
  - ARP (qui a 192.168.56.20 ?)
  - TCP handshake (SYN / SYN-ACK / ACK)
  - RST selon les ports fermés

### 4) Identification des services exposés
- Listing : ports ouverts + services + versions
- Analyse de risques (surface d’attaque, services obsolètes, protocoles en clair)

---

## 🔐 Sécurité / bonnes pratiques appliquées

- ✅ Environnement **totalement isolé** (Host‑Only)
- ✅ Scans orientés **découverte/analyse** dans un cadre d’apprentissage
- ✅ Observation réseau pour comprendre la visibilité d’un scan
- ✅ Documentation (résultats + recommandations)

---

## 🧪 Tests & vérifications

- ✅ Détection des ports ouverts et services (Nmap)
- ✅ Observation des paquets ARP/TCP pendant le scan (Wireshark)
- ✅ Compréhension des signatures/flows générés par les probes Nmap
- ✅ Identification des services “sensibles” et évaluation du risque

---

## 📌 Ce que j’ai appris

- Utilisation de Nmap :
  - découverte de ports
  - détection services/versions
  - scripts NSE (lab)
- Lecture de captures Wireshark :
  - ARP, handshake TCP, reset, patterns de scan
- Lien entre **ports ouverts** → **surface d’attaque** → **risques**
- Rédaction d’une restitution simple et claire (rapport)

---

## 📬 Contact
Pour toute question : **ymansouri.pro@gmail.com**
