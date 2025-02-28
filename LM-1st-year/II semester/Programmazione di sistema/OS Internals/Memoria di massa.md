# Overview sulle memorie di massa

La memoria secondaria di un computer sono divisi in **Hard disk drives** e **Nonvolatile memory**: i supporti al giorno d'oggi sono di natura elettronica (SSD).
Nei classici HDD ci sono dischi che girano tra le 60 e 250 volte per secondo.
Sono caratterizzate da un **transfer rate**, **positioning time**, **seek time** and **head crash**: sono tutti parametri che ci indicano la **lentezza** di un supporto del genere.

### HDD

![[Pasted image 20240513140015.png]]

Definiamo il parametro **Access latency** su un HDD come:
$$
\text{access latency} = \text{average access time } = \text{average seek time} + \text{average latency}
$$
E **Average I/O time**:
$$
\text{Average I/O time = average access time + (amount to transfer /
transfer rate) + controller overhead}
$$

### Non-volatile memory

Fanno parte delle memorie non volatile, **solid state drive**, sono caratterizzate da soli componenti elettronici (no testine, no dischi in rotazione).
Assumono varie forme, tra cui SSD, USB drive, ...
Sono in genere più affidabili, più costose, più veloci (non c'è il positioning e seek time) ma hanno una durata di vita minore. Dato che i bus possono essere lenti, sono spesso connesse direttamente al computer con porte PCI.

Altre caratteristiche:
- La cancellazione di dati avviene in "blocchi"
- Possono essere cancellate solamente un numero finito di volte
- La durata dei componenti è espressa in **drive writes per day (DWPD)**

In particolare, nelle memorie **NAND flash**, in seguito a varie riscritture, si vengono a formare blocchi valid e invalid: la memoria tiene traccia di ciò in un **FTL** (flash translation layer) e tramite un **garbage collector** si vanno a liberare i blocchi invalid.

### Volatile memory

Le memorie non volatili possono essere **temporaneamente usate come memoria secondaria**: sono dette **RAM drives**, e possono essere usati a condizioni che vengano salvati i dati prima di spegnere.

# Struttura dei dischi magnetici

La struttura dei dischi ha sempre influenzato la gestione di quest'ultimi nei sistemi operativi.
I dischi possono essere visti come **un vettore di blocchi logici di memoria**. Occorre che il vettore venga **mappato** in un memoria fisica (descritte da tracce, settori, facce... numerati da 0 in avanti ) 

Il **Disk attachment** è una struttura che descrive come il disco si interfaccia con il resto del sistema: SATA, SAS, USB sono solo alcuni di esempi di bus che si usano per interconnettere memorie secondarie e computer.

### HDD scheduling

**HDD scheduling** è una componente del sistema operativo che si occupa delle decisione delle zone in cui scrivere su disco in modo da **minimizzare i tempi di accesso**.
Dato che possono esistere più processi che vogliono accedere a diverse zone del disco, è necessario scegliere una politica adatta per riordinare le richieste di accesso al disco in modo efficiente.

Consideriamo di volere accedere ai seguenti settori (si ordinano le richieste in ordine cronologico):
$$
98, 183, 37, 122, 14, 124, 65, 67
$$
Usando una politica **FCFS (first come first served, di fatto è FIFO)**: la testa è a 53

![[Pasted image 20240513144500.png]]

Usando un algoritmo **SCAN**, anche detto **algoritmo dell'ascensore**, ci si ferma in tutti i settori che voglio leggere mentre "salgo o scendo":

![[Pasted image 20240513144612.png]]

Usando una politica **C-SCAN** si servono le richieste solamente in una direzione, poi si fa una rapida discesa e si riparte.

![[Pasted image 20240513144658.png]]

Esiste poi lo **Shortest seek time first (SSTF)** che serve prima le richieste che hanno un tempo di seek minore.
Per evitare la **starvation**, in Linux è presente un meccanismo di **deadline** che separa le richieste in lettura e scrittura, con priorità alla lettura: ci sono 4 code, rispettivamente 2 per scrittura e 2 per lettura.

### Errori e correzioni

Nel momento in cui si verifica un errore, si cerca sempre di individuarlo ed eventualmente di correggerlo. Esistono svariate tecniche per la rilevazione degli errori, come i CRC, verifica di parità ecc...

### Gestione dello storage

Si parla di **formattazione fisica (o di basso livello)** quando si divide il disco in settori che un controller può leggere e scrivere: ogni settore può contenere dati addizionali.
Per organizzare un disco in modo coerente occorre un **file system**: di fatto occorre dividere il disco in cilindri trattati come memorie logiche (si parla di **formattazione logica**).

La partizione logica di **root** è quella che in genere contiene il sistema operativo. Altre partizioni contengono il file system, oppure altri sistemi operativi.
Ogni partizione può essere **mounted** o meno (ovvero agganciata al sistema operativo): le partizioni sono montate al bootstrap dove si verifica inoltre la consistenza dei metadati.

```ad-note
title: Bootstrap
All'avvio di una macchina si esegue il MBR che contiene del codice da eseguire al boot e la tabella delle varie partizioni che vanno caricate.
Nei sistemi moderni il tutto è gestito dal **boot loader**.
```

# RAID

E' una particolare configurazione di più dischi, che forniscono **affidabilità** tramite **ridondanza**.
La sigla RAID significa infatti **Redundant array of inexpensive disks**: in sostanza combiniamo più dischi che però sono viste come un unico disco.
L'obiettivo è incrementare il **mean time to failure**:

Esistono diversi schemi di RAID:

![[Pasted image 20240513150908.png]]
