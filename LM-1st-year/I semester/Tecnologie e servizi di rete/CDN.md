# Content Delivery Network

### Perchè sono necessarie?

Obiettivo delle grandi aziende è progettare delle server farm efficienti: i server consumano molta energia quando sono congestionati (>90%) mentre consumano pochissimo (<20%) 

![[Pasted image 20240110162217.png]]

Esiste un componente chiamato **Scheduler (Load balancer)** che distribuisce il lavoro fra più macchine e si cerca il miglior compromesso per le esigenze dell'azienda (può essere non congestionare i server oppure consumare poco).

Quando facciamo una richiesta HTTP l'indirizzo IP che ci viene fornito è quello del load balancer: il traffico ad load balancer verrà poi distribuito (il LD fa 'da NAT' in quanto modifica i pacchetti in uscita per indirizzarli al server corretto in base ad algoritmi).

Un'altra soluzione è assegnare ai nodi S1,S2... un indirizzo IP pubblico, facciamo una richiesta e l'indirizzo IP di destinazione è direttamente il server: il LB diventa l'ultimo router che instrada i pacchetti.

### Web Caches

Negli anni passati si utilizzavano le **web caches**: in questo modo si manteneva il traffico locale (così non pago l'ISP per le infrastrutture), si aveva una esperienza utente più fluida e inoltre era possibile mantenere informazioni geografiche (ads personalizzati in base al luogo).

Questa strategia si integra bene con HTTP in quanto possiede dei campi per impostare la validità di un'informazione in cache.

Tuttavia per grandi player non è sostenibile usare cache per gestire la distribuzione dei loro servizi, in quanto molte informazioni sono un-cacheable.

### Architettura di una CDN

E' una rete di caches sparse in giro per la rete (**replicas**).
Si va quindi ad avvicinare i dati agli utenti, quindi diminuisco i costi, meno colli di bottiglia, miglioro la disponibilità dei servizi, ed effettuo load balancing a seconda di dove posizione le cache.
I proprietari delle CDN sono i proprietari dei servizi stessi e quindi hanno pieno controllo dei dati.

Il server 'centrale' è quello dove vengono caricati i dati, successivamente il server propaga le modifiche su tutta la gerarchia di server: in questo modo gli utenti si collegano al server più vicino a loro. 

![[Pasted image 20240110170843.png]]

| CDN                         | Web Caches      |
| --------------------------- | --------------- |
| Usate dai content providers | Usate dagli ISP |
| Proactive, non aspettano richieste                   | Reactive, aspettano richiesta da utenti        |
| Danno controllo al content provider                            | Non danno controllo al content provider, ma dipendono dall'utente                |

Quando un **distribution node** subisce una modifica allora propaga quest'ultima a tutti i server CDN:

![[Pasted image 20240110171531.png]]

Ora è necessario un meccanismo per puntare a una specifica CDN e nella maggioranza dei casi voglio contattare la più vicina.
Per indirizzare un preciso server CDN posso usare diverse 'metriche': quello con il minor carico, quello per migliorare le performane, un qualunque nodo attiva oppure in base alla geografia (RTT). Distinguiamo perciò 3 meccanismi:
- **Routing** based (IP anycast)
- **Application** based: basato su HTTP redirect
- **Naming** based: tramite DNS

### DNS based CDN

![[Pasted image 20240110175607.png]]

Il nome del host è usato per redigere il traffico al server CDN che rappresenta la migliora opzione:
- Viene quindi generato l'indirizzo IP destinazione
Il DNS server diventa di fatto un “Content Router” (CDN DNS) che misura varie metriche e memorizza la **replica routing table** ((DNS-names, IP-SA) → IP-DA)

![[Pasted image 20240110175844.png]]

Spesso solamente alcuni contenuti di una pagina HTML sono memorizzati nelle CDN: la pagina viene **"akamaizzata"** ovvero ad ogni contenuto che voglio mantenere in CDN appongo un URL che identifica correttamente la replica.

```
http://a836.g.akamaitech.net/7/836/123/e358f5db0045e/www.terena2000.com/logo.gif
```

