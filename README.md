# Guide Complet : Chiffrer un Disque Externe avec VeraCrypt sur Linux.

**Version détaillée pour débutants** — Mai 2026.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Kali Linux](https://img.shields.io/badge/Distro-Kali%20Linux-blue?logo=kali)](https://www.kali.org)

Ce guide t’explique **pas à pas** comment chiffrer complètement un disque dur externe (SSD ou HDD) avec **VeraCrypt** sur Kali Linux.  
Le résultat est **cross-platform** : tu pourras l’utiliser sur Windows, Linux et Mac.

---

## Sommaire :
- [Pourquoi VeraCrypt ?](#pourquoi-veracrypt)
- [1. Installation](#1-installation-de-veracrypt)
- [2. Identifier ton disque](#2-identifier-ton-disque)
- [3. Chiffrement du disque](#3-chiffrement-du-disque)
- [4. Monter & Démonter](#4-monter--démonter)
- [5. Script de montage automatique](#5-script-de-montage-automatique)
- [Warnings importants](#warnings-importants)

---

## Pourquoi VeraCrypt ?

- Chiffrement AES ultra-sécurisé  
- Compatible Windows / Linux / Mac  
- Gratuit, open source et sans backdoor  
- **Si tu perds ta passphrase → les données sont perdues pour toujours**

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

## 2. Identifier ton disque (⚠️ TRÈS IMPORTANT).

```bash
lsblk -f
```

**Ne jamais toucher `/dev/sda`** (c’est ton disque système).

---

## 3. Chiffrement du disque.

### Méthode recommandée (ligne de commande) :

```bash
sudo veracrypt -t --create /dev/sdb1
```

**Réponses à donner :**
- Volume type → `1` (Standard)
- Encryption algorithm → `AES`
- Hash algorithm → `SHA-512`
- Filesystem → `exFAT` (recommandé)
- Passphrase → **phrase longue et forte** (30+ caractères avec espaces)

Bouge ta souris jusqu’à **100 %** d’entropie.

---

## 4. Monter & Démonter.

**Monter :**
```bash
sudo mkdir -p /mnt/veracrypt
sudo veracrypt -t /dev/sdb1 /mnt/veracrypt
```

**Démonter :**
```bash
sudo veracrypt -t -d /dev/sdb1
```

---

## 5. Script de montage automatique (très pratique).

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

Après ça, il te suffit de taper :
```bash
mount-veracrypt
```

---

## Warnings Importants.

<div style="background:#330000; padding:15px; border-left:5px solid #ff4444; border-radius:6px;">
<strong>⚠️ Attention :</strong><br>
• Perte de la passphrase = perte définitive des données (aucun backdoor)<br>
• Toujours vérifier avec <code>lsblk -f</code> avant de chiffrer<br>
• Faire un backup du header après chiffrement
</div>

---
