# Come montare un disco NTFS su Debian

## 1. Assicurati che il pacchetto necessario sia installato
Debian usa il pacchetto `ntfs-3g` per gestire i file system NTFS. Installalo con questo comando:

```bash
sudo apt update
sudo apt install ntfs-3g
```

## 2. Identifica il disco da montare
Usa `lsblk` o `fdisk` per identificare il disco e la partizione NTFS:

```bash
lsblk
```

Oppure:

```bash
sudo fdisk -l
```

Prendi nota del nome del dispositivo, ad esempio `/dev/sdXn` (dove `X` è la lettera del disco e `n` è il numero della partizione).

## 3. Crea una directory per il punto di montaggio
Scegli dove vuoi montare il disco e crea la directory:

```bash
sudo mkdir -p /mnt/ntfs
```

## 4. Monta il disco manualmente
Usa il comando `mount` per montare la partizione NTFS:

```bash
sudo mount -t ntfs-3g /dev/sdXn /mnt/ntfs
```

Sostituisci `/dev/sdXn` con il nome della tua partizione e `/mnt/ntfs` con il tuo punto di montaggio.

## 5. Verifica il montaggio
Controlla se il disco è montato correttamente:

```bash
df -h
```

Dovresti vedere la partizione NTFS montata nel punto scelto.

## 6. Montaggio automatico (opzionale)
Se vuoi che la partizione NTFS venga montata automaticamente all'avvio:

### 6.1 Trova l'UUID del disco:

```bash
sudo blkid /dev/sdXn
```

Copia l'UUID della partizione.

### 6.2 Modifica il file `/etc/fstab`:

Apri `/etc/fstab` con un editor di testo:

```bash
sudo nano /etc/fstab
```

Aggiungi una riga come questa:

```plaintext
UUID=IL_TUO_UUID /mnt/ntfs ntfs-3g rw,uid=1000,gid=1000  0  0
```

Sostituisci `IL_TUO_UUID` con quello copiato e `uid` e `gid` con id del tuo Utente.

### 6.3 Esegui il reload del deamon
```bash
sudo systemctl daemon-reload
```

### 6.4 Prova il montaggio:

Prova a montare tutto con:

```bash
sudo mount -a
```

### 6.5 Elenca i dischi montati:
```bash
dh -h
```