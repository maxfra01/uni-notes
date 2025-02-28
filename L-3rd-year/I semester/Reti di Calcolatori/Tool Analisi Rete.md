# Ping
---
Comando che permette di verificare se un network remoto è attivo.

```bash
ping [options] destination
```

Il ping si basa sul protocollo accessorio ICMP (ovvero il protocollo di controllo dell'IP) e in particolare sull'**ICMP Echo Request** che chiede se IP su un certo server sta funzionando correttamente.
L'host remoto risponde con una **ICMP Echo Reply**.

![[Pasted image 20221115140304.png]]

| Options  | Significato                                                              |
| -------- | ------------------------------------------------------------------------ |
| -t       | (Windows) manda l'ICMP finchè non premo Ctrl+C, di norma ne manda solo 4 |
| -c count | Specifica il numero di Echo Request da mandare                                                                         |

Se il Ping fallisce **non posso concludere nulla**:
- ci possono essere problemi nel percoeso intermedio
- ci possono essere problemi sulla via di ritorno della Reply
- la request si può esser persa
- magari un firewall blocca ICMP
- il server contattato non ha ICMP abilitato
- ecc...
- 

Nell'output del comando abbiamo una metrica tramite **RTT round trip time** ovvero il tempo che passa dall'invio della request all'arrivo della reply.



# Traceroute
---
Mostra la probabile lista degli hop che un pacchetto attraversa per raggiungere una certa destinazione.

```bash
traceroute [options] destination
```

Si basa sul messaggio ICMP Time Exceeded quando su un router scade il TTL.
Allora TraceRoute manda pacchetti con TTL incrementali così ad ogni messaggio Time exceeded che ottengo conosco un nuovo hop della rotta.


# Arp
---
Il comando permette di modificare la **arp cache** dell'host.

```bash
arp [options] [IPAddr] [EthAddr]
```

| Opzione           | Significato                                  |
| ----------------- | -------------------------------------------- |
| -a                | Mostra la cache arp                          |
| -d IPAddr         | Rimuove dalla cache il corrispondente IPAddr |
| -s IPAddr EthAddr | Aggiunge un'associazione statica fra IP e MAC                                             |


# Route
---
Mostra e permette di modificare la **routing table**

```bash
route [options] [command] [parameters]
```

| Opzioni  | Significato                       |
| -------- | --------------------------------- |
| print    | Mostra la routing table (windows) |
| -n       | Mostra la routing table (UNIX)    |
| -add ... | Aggiungo una route statica nella routing table                                  |


# Ipconfig
---
Mostra una serie di informazioni legate alla rete

```bash
ip
```

Nei sistemi UNIX abbiamo solamente due opzioni:
- 'address' permette di modificare le interfaccie
- 'route' permette di modificare la routing table


# tcpdump/ windump
---
Sono semplici **network analizer** che catturano i pacchetti e ne mostrano informazioni. Sono anche detti **packet sniffer**.

```bash
tcpdump [options] [filters] 
```

Grazie ai **driver di cattura/libpcap** è possibile intercettare il traffico appena arrivato sulla scheda di rete e lo manda direttamente a livello applicazione (saltando i controlli del livello 3).

```ad-note
Esistono software grafici che fanno esattamente la stessa cosa ma mostrando meglio il contenuto dei pacchetti: es. www.wireshark.org
```


# Nslookup
---
Vedi slide
