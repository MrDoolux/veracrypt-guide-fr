# Guide Complet : Chiffrer un Disque Externe avec VeraCrypt sur Kali Linux

**Version détaillée pour débutants** — Mai 2026

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![Kali Linux](https://img.shields.io/badge/Distro-Kali%20Linux-blue?logo=kali)

Ce guide t’explique **pas à pas**, de façon claire et humaine, comment chiffrer un disque dur externe (SSD ou HDD) avec **VeraCrypt** sur Kali Linux.  
Le résultat est **entièrement cross-platform** (Windows, Linux, Mac).

---

## Sommaire : 
- [Pourquoi VeraCrypt ?](#pourquoi-veracrypt)
- [1. Installation](#1-installation-de-veracrypt)
- [2. Identifier ton disque](#2-identifier-ton-disque)
- [3. Chiffrement du disque](#3-chiffrement-du-disque)
- [4. Mode Hidden Volume](#4-mode-hidden-volume)
- [5. Keyfiles & KeePass](#5-keyfiles--keepass)
- [6. Backup du Header](#6-backup-du-header)
- [7. Monter & Démonter](#7-monter--démonter)
- [8. Guide Windows](#8-guide-windows)
- [Warnings Importants](#warnings-importants)

---

## Pourquoi VeraCrypt ?

- Chiffrement AES très robuste
- Totalement gratuit et open source
- Fonctionne sur Windows, Linux et Mac
- **Pas de backdoor** : si tu perds ta passphrase, les données sont perdues pour toujours

---

## 1. Installation de VeraCrypt.

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

---

## 2. Identifier ton disque (⚠️ CRITIQUE).

```bash
lsblk -f
```

**Ne jamais chiffrer `/dev/sda`** (c’est ton disque système Kali).

---

## 3. Chiffrement du disque.

```bash
sudo veracrypt -t --create /dev/sdb1
```

**Réponses recommandées :**
- Volume type → `1` (Standard)
- Encryption algorithm → `AES`
- Hash algorithm → `SHA-512`
- Filesystem → `exFAT` (recommandé pour Windows + Linux)
- Passphrase → phrase ultra-forte (30+ caractères avec espaces)

Bouge ta souris jusqu’à **100 %** d’entropie.

---

## 4. Mode Hidden Volume (Plausible Deniability).

Pendant la création, choisis **Hidden VeraCrypt volume** à la place de Standard.  
Tu auras deux passphrases :
- Une "faible" (pour des données visibles)
- Une forte (pour le volume caché invisible)

Très utile en cas de contrainte ou perquisition.

---

## 5. Keyfiles & KeePass.

**Recommandation forte :** Utilise **1 ou 2 keyfiles** + KeePass pour gérer tes passphrases.

- Crée une base KeePass chiffrée
- Stocke toutes tes passphrases VeraCrypt dedans
- Ajoute un keyfile (une photo ou un PDF que tu ne modifieras jamais)

---

## 6. Backup du Header (très important !).

```bash
mkdir -p ~/VERACRYPT_BACKUP
sudo veracrypt --save-volume-header-backup /dev/sdb1 ~/VERACRYPT_BACKUP/header_sdb1.vc
```

Fais ça **après chaque chiffrement**.

---

## 7. Monter & Démonter.

**Script automatique recommandé :**

```bash
cat > ~/mount-veracrypt << 'EOF'
#!/bin/bash
echo "🔐 Montage du disque VeraCrypt..."
sudo mkdir -p /mnt/veracrypt
sudo veracrypt -t /dev/sdb1 /mnt/veracrypt
echo "✅ Disque monté sur /mnt/veracrypt"
EOF

chmod +x ~/mount-veracrypt
sudo cp ~/mount-veracrypt /usr/local/bin/
```

Utilisation :
```bash
mount-veracrypt          # pour monter
sudo veracrypt -t -d /dev/sdb1   # pour démonter
```

---

## 8. Guide Windows.

- Installe VeraCrypt depuis le site officiel : 
-  https://www.veracrypt.fr
- Désactive **Fast Startup** (recommandé)
- Utilise la même passphrase + keyfiles
- Procédure quasi identique via l’interface graphique

---

## Warnings Importants.

> **⚠️ Attention**  
> - Perte de la passphrase ou du keyfile = **perte définitive** des données  
> - Toujours vérifier avec `lsblk -f` avant de chiffrer  
> - Ne jamais utiliser l’option `--quick` sur des données importantes

---

**Tu as des questions ?** Ouvre une Issue.

Merci d’avoir lu ce guide ❤️

---
