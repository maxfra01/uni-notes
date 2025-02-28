# Problemi dello strato 2
---
Perchè necessitiamo di un terzo strato?
3 principali problematiche legate allo strato collegamento:
- **Inefficienza della gestione della ridondanza**: in generale è bene che i collegamenti siano ridondanti in un rete per aumentare la tolleranza ai guasti. E' pero possibile che in una trasmissione broadcast si crei una **broadcast storm** e dunque la gestione dei pacchetti diventa scomoda. Inoltre l'algoritmo di spanning tree su scala geografica non è efficiente.
- **Saturazione dei database di forwarding degli switch**: su reti a larga scala la dimensione delle tabelle è eccessiva. L'aggregazione di entry (basata ad esempio sui primi bit del MAC) non è possibile per le struttura del MAC (la prima parte del MAC è decisa dal costruttore).
- **Propagazione del traffico broadcast**: in generale una trasmissione broadcast appesantisce la rete, perciò cerco di limitarlo.

# Instradamento
---
Una delle funzione dello [[OSI 3 - Strato Rete]] è il routing dei pacchetti (per ogni PDU).
L'instradamento è effettuato leggendo informazioni da tabelle che contengono il **next-hop** cioè prossimo router.

- **Protocollo di instradamento**: definizione delle modalità di scambio di informazioni sullo stato della rete al fine di costruire tabelle di instradamento.
- **Algoritmi di instradamento**: operazioni necessarie per scegliere il percorso verso la destinazione
- **Forwarding**: operazioni per instradare i pacchetti sulla porta di uscita giusta.

Il costo degli algoritmi di instradamento dipendono da distanze, ritardo, euro, livello di congestione e dipendono dal variabile stato della rete.

I complesso algoritmi si dividono in due categorie:
- **centralizzati**, dove un nodo si occupa di raccogliere informazioni su tutti gli altri, calcola i percorsi e restituisce il risultato (troppo scomodo e complesso, vulnerabile ai guasti)
- **distribuiti**, dove la complessità è distribuita su tutta la rete, ovviamente si richiede che i nodi siano intelligenti e abbiamo più resistenza ai guasti.

Gli algoritmi **distribuiti** si dividono a loro volta in globali e parziali:
- nel primo caso ogni nodo conosce tutta la topologia, troppo complesso
- nel secondo ogni nodo conosce solo quelli adiacenti e c'è uno scambio di info solo tra questi ultimi.

Esempi di algortimi: Dijkstra, Distance vector

# Strato rete: Introduzione
---
Quando vogliamo realizzare reti ben oltre la scala locale dobbiamo per forza ricorrere a un nuovo strato con nuovi protocolli.

 Il compito principale dello strato rete è quello di trasportare **pacchetti o datagram** da un host a un altro, tramite i router IP che leggono l'indirizzo di destinazione e instradano correttamente i dati.

Differenza fra i termini *Router* e *Forwarding*:

| Forwaring | Routing |
| --------- | -------- |
| Trasferire i pacchetti correttamente fra router          | Processo di scrittura delle tabelle per realizzare il forwarding         |

Diciamo che prima di fare forwarding occorre fare routing per determinare le "strade".

## Virtual Circuit

Normalmente il livello tre è di tipo datagram non orientata alla connessione, tuttavia è possibile implementare un network layer connection-oriented

![[Pasted image 20221109110234.png]]

L'idea è di costruire una rete riservandomi delle risorse e creare un circuito virtuale, come fosse un'evoluzione della rete teleofonica.

| Datagram                                                | Virtual Circuit                                                       |
| ------------------------------------------------------- | --------------------------------------------------------------------- |
| Informazioni scambiate fra host, il servizio è elastico | Evoluzione dalla telefonia, basato sul modello di comunicazione umano |
| Pratica BEST EFFORT, qualità non sempre alta            | Si vuole garantire qualità sempre alta                                |
| Logica interna semplice, complessità al 'bordo'         | Complessità interna molto alta                                                                      |


# Protocollo IP
---
All'interno dello strato rete troviamo le seguenti componenti:

![[Pasted image 20221109111154.png]]

Ovvero protocolli di Routing, protocollo IP e protocollo ICMP.
Inoltre notiamo che occorre una forwarding table opportunamente aggragata per ottimizzare la scalabilità.
OSPF implementa l'algoritmo di Dijkstra, mentre BGP implementa Bellman-Ford.

All'interno dello strato rete la struttura della trama è così composta:

![[Pasted image 20221109111529.png]]

Al posto di PDI e SDU usiamo HEADEAR e PAYLOAD.
- **data** è il payload, dove troveremo dei segmenti di livello 4 (TCP, UDP,ICMP)
- **ver** fornisce informazioni sulla versione del protocollo IP, ipv4 o ipv6
- **head. len** definisce la lunghezza dell'header, necessaria per sapere se ci sono opzioni e capire quando inizia il payload
- **type of service** serve per garantire un minimo di qualità per quanto possibile, ad oggi sostituito dal campo DS, standardizzato dal procollo 'DiffServ'. Serve per indicare la categoria di dati da trasferire e si possono indicare delle priorità nella rete.
- **lenght** è la lunghezza complessiva del pacchetto

## Frammentazione

I prossimi 4 campi (lenght, 16bit identifier, flags e fragment offset) servivano per eseguire la **frammentazione e riassemblaggio**:
Talvolta occorre frammentare un pacchetto se supera la dimensione massima di trasferimento di un altro strato (es. ethernet che supporta al massimo 1500 byte).
Se devo trasferire su infrastruttura ethernet un pacchetto da 3000 byte lo frammenterò in due.

Ipv6 ha **completamente rimosso** la frammentazione, quindi non trattiamo questi campi.

- **Time to leave** è il numero massimo di hop che può fare ancora il pacchetto prima di essere scartato.
- **upper layer** specifica il layer superiore a cui consegnare il payload
- **header checksum** serve per la RILEVAZIONE di errore all'interno dell'header, e se trova un errore il pacchetto viene direttamente scartato. I controlli sul payload saranno effettuati dalla destinazione. L'header checksum va ricalcolato a ogni nodo in quanto cambia il TTL.
- **32-bit source, destination IP address** sono i due campi più importanti al fine di routing 

## Indirizzo IP

Sono identificatori su 32 bit associati alle *interfacce* host e ruoter, ovvero la connessione esistente fra l'host e il link fisico (es. interfaccia associata alla porta ethernet del computer).

32 bit sono spesso raggruppati in 4 gruppi da 8 bit e convertiti in decimale.
Ad esempio 192.168.0.1 

l'IP non si preoccupa di che infrastruttura usi il livello sottostante, che sia wifi o ethernet non c'è differenza.
L'importarte è che IP assume che ci sia una logica a livello inferiore in grado di consegnare i pacchetti.

Ogni indirizzo IP è diviso in due parti: **subnet part e host part**.
La subnet è dunque un insieme di interfacce che condividono la stessa subnet part; queste interfacce dovrebbero essere in grado di raggiungersi a livello 2.

![[Pasted image 20221109120138.png]]

Inoltre introduciamo il concetto di **rete fisica**, ovvero che comunicano a livello collegamento.

Alcuni indirizzi IP sono riservati a particolari scopi:

![[Pasted image 20221109122616.png]]

IP assume una **corrispondenza biunivoca** fra reti logiche e subnet, ovvero per una subnet avrò una e una sola rete fisica.
Realizziamo una:
- connessione **diretta** se trasferiamo dati fra host della stessa subnet
- connessione di **routing, o indiretta**, se gli host fanno parti di subnet diverse
Se devo effettuare questa comunicazione diretta, l'host si rende conto che la destinazione è nella stessa subnet perciò dice allo strato 2 di raggiungere quella destinazione (Il router non viene nemmeno toccato).

Dunque i passi dell'algoritmo di IP sono riassumibili in:
1. leggo la destinazione del dataframe
2. mi chiedo se è nella stessa subnet
3. se lo è chiedo il trasferimento a livello 2 (connessione diretta)
4. se non lo è richiedo l'intervento del router

In realtà la corrispondenza biunivoco non è obbligatoria, perchè è possibile trovare più subnet sotto un unico collegamennto fisico (**one-arm router**), ma anche una subnet che si estende su più collegamenti fisici (non usata in pratica).

Ogni interfaccia in una rete fisica possiede un indirizzo MAC
che è portatile, in qualunque parte della rete mi trovi, e tra una LAN e un'altra.
Questo non vale per un indirizzo IP, che **non è portatile**, in quanto dipende dalla subnet a cui connetto il mio dispositivo.

La struttura gerarchica dell'IP è possibile ottimizzare il processo di routing, raggruppando le entry per subnet ID.

## Protocollo ARP

Come fa un nodo a sapere l'indirizzo MAC dei dispositivi in una subnet a partire da un indirizzo IP.
Ci pensa il **protocollo ARP**.
Ogni dispositivo dotato di indirizzo IP possiede una **ARP table** contenente per qualche IP in LAN il suo corrispondente indirizzo MAC.
La ARP table è composta da entry da 3 colonne, IP, MAC e TTL oltre il quale la entry viene dimenticata.

Ma necessitiamo di un protocollo che sia in grado di mappare automaticamente l'arp table, e la soluzione di ciò è un approccio **broadcast**.

Come funziona il protocollo?
1. **ARP REQUEST**: il dispositivo che necessita di aggiornare la ARP table chiede in broadcast sulla LAN se qualcuno ha nella sua ARP table la traduzione di cui ho bisogno.
2. **ARP REPLY**: la risposta che arriva un dispostivo è inviata in **Unicast**

```ad-caution
Il protocollo ARP si colloca fra il livello 2 e 3, ma c'è da notare che i suoi, non sono pacchetti bensì trame di livello 2.
```

La forma della trama ARP è la seguente:

![[Pasted image 20221114180451.png]]

# Addressing IP
---
Affinchè ci sia trasferimento tra due LAN differenti è opportuno fare alcune assunzioni:
- La rete A deve conoscere l'indirizzo IP del destinatario (vedremo DNS)
- La rete A deve conoscere l'indirizzo IP del Default Gateway che le permette di fare comunicazioni indiretta. (vedremo DHCP)
- La rete A conosce l'indirizzo MAC del Default Gateway (grazie ad ARP).

![[Pasted image 20221114133721.png]]

Quando dobbiamo far partire la comunicazione:
1. L'host A creà il pacchetto con destinazione B, vedrà che non si trova nella stessa subnet e quindi chiede aiuto a ethernet
2. L'host A allo strato 2 creerà la trama con le informazioni fornite dal 3 per mandare il tutto al Gateway (tramite MAC).
3. Quando la trama arriva a R viene processato, R vedrà che l'indirizzo IP destinazione è quello di B, dunque creerà la trama con destinazione MAC B e inoltrerà la trama su ethernet.

## Classfull addressing

Sono suddivisi in 3 classi a seconda della divisione della subnet e dell'host part.
Distinguiamo la classe A,B,C 

| A                                                                                              | B                                                            | C                                |
| ---------------------------------------------------------------------------------------------- | ------------------------------------------------------------ | -------------------------------- |
| Hanno 8 bit nella parte di rete e i restanti 24 sono riservati alla parte host.                | Al fine di identificare la classe B occorre che inizi con 01 | Identificata da un inizio da 110 |
| Per riconoscerli basta notare che il primo bit è 0, in quanto rimangono 128 numeri disponibili | Posseggono una parte di rete sopra dal 128 al 140            | 8 bit per la parte di host       |
|                                                                                                |                                                              |                                  | 

Le classi D ed E sono poco usate:
- nella classe D gli indirizzi iniziano con 1110, e sono riservati per tecniche multicast
- nella classe E gli indirizzi iniziano con 1111,e sono riservati per scopi futuri.


## Subnetting

Si utilizza la tecnica del **VLSM variable lenght subnet mask**.
Una **subnet mask** è una stringa di bit messi a 1 in corrispondenza alla parte di subnet e 0 in corrsipondenza dei bit nella parte host.

![[Pasted image 20221115103908.png]]


## CIDR

Approccio di tipo classless, l'idea generale è che se devo definire una rete arbitraria non devo partire da nessuna classe "standard" di partenza ma definisco le reti in base a quanta dimensione mi serve.
**Classless InterDomain Routing** fa si che si abbiamo solo reti e non sottoreti.

E' presente una **netmask** oppure in alternativa una **prefix lenght** corrispondente al numero di 1 in una netmask.

![[Pasted image 20221115104654.png]]

I valori validi nei byte della netmask sono: 0 128 192 224 240 248 252 254 (non valido sul quarto byte) 255.

## Indirizzamento gerarchico

Quando abbiamo bisogno di indirizzi IP dobbiamo avere l'aiuto del provider, e quest'ultimo deve basarsi sulle regole stabilite da organizzazioni mondiali.
Gli ISP comprano indirizzi a seconda delle esigenze di mercato che si aspettano e poi possono riverderli a ISP minori o assegnarli IN ORDINE ai clienti.

Gli ISP assegnano un certi range contiguo di valori IP ai clienti che ne necessitano!

L'indirizzamento gerarchico non è possibile con il classfull approach!

## Funzionamento effettivo di IP

Come fa un dispositivo a sapere il proprio **network ID**:
Basta effettuare un bit-wise AND fra l'indirizzo IP dell'host e la sua net mask!

Come si ottiene il network ID del destinatario?
AND bit-wise fra la PROPRIA maschera e IP del destinatario!

Nel caso di questi due risultati siano uguali allora significa che IP può effettuare una comunicazione diretta, quindi chiama ethernet ecc...

![[Pasted image 20221115115432.png]]

In caso questo non sia verificato occorre comunicare indirettamente, dunque serve un router.

Come fa il router a selezionare la porta corretta per l'output?
AND bit-wise fra l'indirizzo IP destinazione e la maschera di ogni entry nella routing table, cercando quella entry che da come risultato l'IP corrispondente nella ruoting table stessa!

## Longest prefix Matching

Quando controlliamo una forwarding table per raggiungere una certa destinazione e troviamo dei match multipli allora si **usa quello con prefisso più lungo** 


# IP Routing
---
Esistono 3 tipi di **route**(ovvero le entry della routing table):
- **direct routes** sono quelle relative alle reti direttamente connesse al router ( ovvero le route per le quali il router può arrivare con collegamenti di livello 2)
- **static routes** sono quelle che il router non può raggiungere con pratiche di livello 2, e inoltre sono configurate a utente
- **dynamic routes** sono configurate automaticamente

Ci concentriamo in particolare sulle static routes.

## Routing table

La routing table contine le seguenti informazioni:

| Type | Destination    | Next-hop    | Cost |
| ---- | -------------- | ----------- | ---- |
| S    | 130.192.0.0/24 | 130.192.3.2 | 2    |
| S    | 130.192.1.0/24 | 130.192.3.2 | 3    |
| S    | ..             | ...         | ..   |
| S    | ..             | ..          | ..   |
| D    | ..             | ..          | ..   |
| D    | ..             | ..          | ..   |
| D    | ..             | ..          | ..   |

Per le route dirette abbiamo l'interfaccia locale del router!

```ad-important
Per quanto riguarda **Internet** possiamo usare l'indirizzo `0.0.0.0/0` in quanto qualunque pacchetto matcha con questa stringa (in caso di altri match per il longest prefix match si sceglierà quello) e se non ci sono match si manda il pacchetto alla cosiddetta **Default Route**.
```


```ad-attention
E' importante che il next hop sia un indirizzo IP (perchè il router lavora su quel livello) che però deve essere direttamente raggiungibile (a livello 2) dal router!
```



# DHCP
---
**Dynamic Host Configuration Protocol** è il protocollo che possiede come obiettivo quello di assegnare in modo automatico un indirizzo IP ad un host quando entra su una rete.

In realtà questo è il primo protocollo di livello 4 della pila  [[OSI 7 - Strato Applicazione]].

Per funzionare, questo protocollo ha bisogno di un server DHCP presente sulla rete. Quando un client arriva su una nuova rete l'idea è che il server assegni un IP all'host:
1. Appena l'host si connette alla nuova rete esso manda un **DHCP discover** all'indirizzo `255.255.255.255` (limited broadcast, ovvero solo sulla rete di livello 2). L'host avrà un indirizzo `0.0.0.0` (unspecified IP) 
2. Solo il DHCP si fa carico della discover, il quale manda a sua volta una **DHCP Offer** in **broadcast**. Nel campo **yiaddr** conterrà la proposta che il server fa all'host.
3. A questo punto l'Host manda una **DHCP Request** in **broadcast** (anche per informare gli altri possibili server DHCP) per far sapere quale IP abbia scelto l'host
4. Il server DHCP in questione manda in **broadcast** una **DHCP ACK** per confermare l'acquisizione dell'indirizzo

Le offerte, richieste e ack hanno un lifetime (3600s) oltre il quale il server DHCP suppone che l'host non stia più usando l'indirizzo.
Per continuare a usare un indirizzo IP bisogna rinnovare la **lease** per quell'indirizzo: in questo caso abbiamo solo la request e ack.

```ad-note
- Nella fase di DHCP discover avrò anche a livello 2 un broadcast MAC FF:FF:FF:FF:FF
- In caso di server DHCP multipli tutti questi manderanno una offerta
```

![[Pasted image 20221115132844.png]]

Oltre all'assegnazione di un indirizzo IP, il protocollo DHCP fornisce una **configurazione completa** per un host che si connette alla rete. (deafult gateway e dns)


# NAT
---
Alcuni Range di indirizzo sono riservati per delle **reti private**.
Se sono in questa condizione posso non preoccuparmi degli indirizzi che uso.

![[Pasted image 20221115134929.png]]

Per permetterci di usare indirizzi privati ma di navigare su internet esiste il **Network Address Translation NAT**.
Anche lui utilizza una parte del livello trasporto [[OSI 4 - Strato Trasporto]].

![[Pasted image 20221205161638.png]]

Ciò è molto comodo in quanto è possibile cambiare indirizzo IP nella rete locale senza dover avvisare il mondo intero, possono cambiare ISP senza cambiare tuti gli IP ma solamente uno.

E' **compito del router** implementare il NAT seguendo le seguenti fasi:
1. Ogni pacchetto in uscita va corretto sostituendo l'indirizzo IP sorgente con l'indirizzo IP NAT sorgente
2. Ricordarsi le coppie (IPNAT,port#)-(IP source,port#)
3. Per ogni pacchetto in entrata modificare la destinazione con il corretto IP della rete locale.

Sorge però un problema: supponiamo che un client voglia contattare un dato server, che però si trova in una rete locale, dunque il client non riesce a contattarlo perchè l'unico indirizzo che conosce è il NAT router di quella rete.
- Una **prima soluzione** consiste nel configurare staticamente il NAT in modo da associare una richiesta a una certa porta a un preciso server nella rete locale. (**port forwarding**)
- **relaying**: Una soluzione adottatta da Skype è l'uso di un server mediano per inoltrare i pacchetti: il client dentro alla LAN e quindi NAT client si connette a questo "ponte" e anche il client esterno lo fa, così il bridge è in grado di dirigere i pacchetti

![[Pasted image 20221230155839.png]]

# ICMP: Internet Control Message Protocol
---
Questo protocollo si basa sull'invio di messaggi ICMP formati da due campi principali: **type e code**

| Type | Code | Description               |
| ---- | ---- | ------------------------- |
| 0    | 0    | echo reply (ping)         |
| 3    | 0    | dest. network unreachable |
| 3    | 6    | dest. network unknown     |
| 5    | 0    | redirect                  |
| 8    | 0    | echo request              |
| 10   | 0    | route discovery           |
| 11   | 0    | TTL expired               |
| ecc  | ecc  | ecc                          |

ICMP è la base per il comando 'ping' nei [[Tool Analisi Rete]].