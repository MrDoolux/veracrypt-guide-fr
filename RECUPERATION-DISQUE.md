# Récupération d'un disque après échec de chiffrement VeraCrypt

Ce guide explique comment remettre à zéro un disque qui ne monte plus après un problème de chiffrement VeraCrypt.

---

## 1. Version Windows (PowerShell)

Ouvrez **PowerShell en tant qu'Administrateur**.

### Identifier le disque
```powershell
Get-Disk | Select Number, FriendlyName, Size, OperationalStatus
```

### Remettre le disque à zéro (remplacer `2` par votre numéro de disque)
```powershell
Clear-Disk -Number 2 -RemoveData -RemoveOEM -Confirm:$false

Initialize-Disk -Number 2 -PartitionStyle GPT

New-Partition -DiskNumber 2 -UseMaximumSize -AssignDriveLetter | Format-Volume -FileSystem exFAT -NewFileSystemLabel "DISQUE_VIDE" -Confirm:$false
```

---

## 2. Version Linux (Kali / Debian)

### Identifier le disque
```bash
lsblk -d -o NAME,SIZE,MODEL
```

### Remettre le disque à zéro
```bash
# Attention : remplacer sdb par votre disque !!!
sudo wipefs -af /dev/sdb
sudo dd if=/dev/zero of=/dev/sdb bs=1M count=200 status=progress

sudo parted -s /dev/sdb mklabel gpt
sudo parted -s /dev/sdb mkpart primary 1MiB 100%

sudo mkfs.exfat -n "DISQUE_VIDE" /dev/sdb1
```

---

## ⚠️ Avertissements importants

- **Vérifiez deux fois** le disque avant de lancer les commandes
- Ces commandes **effacent toutes les données**
- Après avoir rechiffré, faites toujours un **backup du header**

---
