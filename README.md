# Guide Complet VeraCrypt 2026

**Chiffrer ses disques durs (Linux & Windows)** — Version détaillée pour débutants

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![Kali](https://img.shields.io/badge/Distro-Kali%20Linux-blue)
![Windows](https://img.shields.io/badge/Platform-Windows-blue)

Ce repository contient un guide complet pour chiffrer des disques externes avec **VeraCrypt** de façon sécurisée et portable.

---

## Sommaire
- [Pourquoi VeraCrypt ?](#pourquoi-veracrypt)
- [1. Installation](#1-installation)
- [2. Identifier le disque](#2-identifier-le-disque)
- [3. Chiffrement du disque](#3-chiffrement-du-disque)
- [4. Mode Hidden Volume](#4-mode-hidden-volume)
- [5. Keyfiles & KeePass](#5-keyfiles--keepass)
- [6. Backup du Header](#6-backup-du-header)
- [7. Monter / Démonter](#7-monter--démonter)
- [8. Guide Windows](#8-guide-windows)
- [Warnings Importants](#warnings-importants)

---

## Pourquoi VeraCrypt ?

- Chiffrement AES très robuste
- Cross-platform (Windows, Linux, Mac)
- Gratuit, open source, sans backdoor
- **Perte de passphrase = perte définitive**

---

## 1. Installation

**Sur Kali Linux :**
```bash
cd ~/Downloads
wget https://launchpad.net/veracrypt/trunk/1.26.24/+download/veracrypt-1.26.24-setup.tar.bz2
tar -xvf veracrypt-1.26.24-setup.tar.bz2
sudo ./veracrypt-1.26.24-setup-gui-x64
```
Lance ensuite :
```bash
veracrypt
```


**Sur Windows :** Télécharge depuis https://www.veracrypt.fr

---

## 2. Identifier le disque

```bash
lsblk -f
```

**⚠️ Ne jamais chiffrer `/dev/sda` !**

---

## 3. Chiffrement du disque

```bash
sudo veracrypt -t --create /dev/sdb1
```

Réponses recommandées :

Volume type → 1 (Standard)
Encryption algorithm → AES
Hash algorithm → SHA-512
Filesystem → exFAT (meilleur choix cross-platform)
Passphrase → phrase longue (30+ caractères avec espaces)

Bouge ta souris et tape sur le clavier jusqu’à 100 % d’entropie.

---

## 4. Mode Hidden Volume (Plausible Deniability)

Choisis l’option **Hidden VeraCrypt volume** pendant la création.  
Tu auras deux passphrases :
- Une faible (pour les données visibles)
- Une forte (pour le volume caché)


---

## 5. Keyfiles & KeePass

**Recommandation forte :** Utilise **1 ou 2 keyfiles** + KeePass pour gérer tes passphrases.

- Crée une base KeePass chiffrée
- Stocke-y toutes tes passphrases VeraCrypt
- Utilise un keyfile (photo, PDF, etc.) que tu ne modifieras jamais

---

## 6. Backup du Header (très important !)

```bash
mkdir -p ~/VERACRYPT_BACKUP
sudo veracrypt --save-volume-header-backup /dev/sdb1 ~/VERACRYPT_BACKUP/header_sdb1.vc
```

Fais ça **après chaque chiffrement**.

---

## 7. Monter / Démonter

Monter :
```bash
sudo mkdir -p /mnt/veracrypt
sudo veracrypt -t /dev/sdb1 /mnt/veracrypt
```

Démonter :
```bash
sudo veracrypt -t -d /dev/sdb1
```


**Script automatique :**
```bash
cat > ~/mount-veracrypt << 'EOF'
#!/bin/bash
sudo mkdir -p /mnt/veracrypt
sudo veracrypt -t /dev/sdb1 /mnt/veracrypt
EOF
```

Puis `mount-veracrypt` et `sudo veracrypt -t -d /dev/sdb1`

---

## 8. Guide Windows

- Installe VeraCrypt
- Désactive **Fast Startup** (recommandé)
- Même procédure que sur Linux via l’interface graphique
- Utilise le même keyfile et passphrase

---

## Warnings Importants

> **⚠️ Attention**  
> - Perte de la passphrase ou du keyfile = **perte définitive**  
> - Toujours vérifier le disque avec `lsblk -f`  
> - Ne jamais utiliser `--quick` sur des données importantes

---

