# Necessità di IPv6

Il principale motivo responsabile dell'introduzione di IPv6 è **un maggiore spazio di indirizzamento**.
[[Ipv4]] fornisce solamente 3.5 miliardi di indirizzi IP: durante gli anni si sono trovate diverse soluzioni per mitigare questa 'carenza' di indirizzi, come il NAS, l'introduzione delle Netmask, indirizzamento privato...

L'ente IANA assegna gli indirizzi IP su base geografica: ogni macro zona (**RIR, Regional Internet Registry**) possiede una porzione degli indirizzi IP /8, che vanno poi ripartiti in tutto il territorio.

![[Pasted image 20231018163347.png]]

Gli indirizzi IPv4 possono trovarsi in uno di questi 5 stadi:
- **Riservati** per usi speciali
- **Non allocati**, mantenuti da IANA
- **Non assegnati**, mantenuti da uno dei RIR
- **Assegnate ma non annunciate** da BGP
- **Assegnate e annunciate** da BGP

![[Pasted image 20231018164124.png]]

L'obiettivo del protocollo IP è fare in modo che un qualunque nodo possa raggiungerne un altro, indipendentemente da dove si trovi.
Tuttavia la dimensione delle tabelle di BGP negli anni è in crescita, e i router possono gestire solo una certa quantità di dati.

# IPv6

L'indirizzo **IPv6** è composto da **128 bits** separati in **8 gruppi da 4 cifre esadecimali** (4 bit per ogni cifra esadecimale) da un ':'. 

E' permesso accorciare l'indirizzo andando a **rimuovere gli zero più significativi** del gruppo.
Una seconda scorciatoia prevede si **sostituire 0000 con ::**, ma va fatto solo una volta per indirizzo.

```
FEDC:0000:0876:45FA:0562:0000:3DAF:BB01
1080:0:0:7:200:A00C:3423:A089 #prima regola
FEDC::0876:45FA:0562:0:3DAF:BB01 # seconda regola
```

Come per la v4, abbiamo l'indirizzo diviso in due parti: **Prefix** e **Interface ID**:

![[Pasted image 20231018173748.png]]

Attualmente la **lunghezza del prefisso è fissata a 64 bit** per tutte le reti IPv6.

Il **principio di funzionamento è il medesimo** ma la nomenclatura è diversa:
- Una rete fisica è detta **link**
- Un **subnetwork** è un link, quindi un gruppo di host con stesso prefisso.
- La comunicazione diretta si chiama **on-link communication**
- La connessione indiretta **off-link**

Le tipologie di indirizzi IPv6 sono le seguenti:

![[Pasted image 20231018174507.png]]

# Multicast IPv6

Nel v6 manca il concetto di Broadcast limitato (in v4 255.255.255.255).
I **Multicast address IPv6** sono caratterizzati da **FF sul primo byte**: quindi ha forma generale `FF00::/8`.
Il multicast è diviso ulteriormente in altre sottocategorie.
- **Well known multicast**: `FF00::/12` assegnati per un certo gruppo di dispostivi, sono usati per comunicazioni di servizio 
- **Transient**: `FF10::/12` sono assegnati dinamicamente dalle applicazioni
- **Solicited-node Multicast**: `FF02:0:0:0:0:1:FF00::/104` sono simili a dei broadcast address in ARP

```ad-note
title: ARM in IPv6
Il Protocollo ARM in IPv6 è stato inglobato all'interno di ICMP.
```

La struttura di un indirizzo multicast è la seguente:

![[Pasted image 20231018175148.png]]

I primi 8 bit (il prefisso) corrispondono alle cifre hex `FF`, successivamente il **T Flag** (assegnato da IANA, lungo 4 bit) specifica se si tratta di well known o transient (ecco perchè varia da /8 a /12 nei casi specifici).
Lo **scope permette di specificare il range** dei pacchetti multicast.
Infine è presente il **Group ID**.

```ad-danger
title: Broadcast in IPv6
Non esiste un concetto come il broadcast IPv4, quindi un pacchetto multicast che arriva a uno switch non viene inoltrato su tutte le porte, ma solo a quelle che fanno parte del GROUP ID.

```

# Unicast IPv6

Gli indirizzi unicast, o meglio **Global unicast addresses** sono indirizzi di tipo aggregato, utilizzati in modo equivalente agli indirizzi pubblici in IPv4. Hanno la caratteristica di essere raggiungibili e indirizzabili globalmente, oltre a essere **plug and play**. Attualmente sono disponibili in un range definito tra `3FFF::` e `2000::`. 
Questi indirizzi hanno **i primi 3 bit (più significativi) posti a 001**.
Hanno la caratteristica di essere distribuiti geograficamente in modo gerarchico.

![[Pasted image 20231018175406.png]]

Il **Global routing prefix** è di lunghezza fissa $n=45$ bits
Il **Subnet ID** è composto da $m=16$ bits e viene usato dagli ISP per assegnare sottoreti ai clienti.

Formalmente gli indirizzi unicast sono scelti sulla base di una gerarchia di ISP:

![[Pasted image 20231018175800.png]]

Altri tipi di indirizzi unicast sono gli **indirizzi privati**: c'è altra probabilità che siano univoci, ma non possono essere utilizzati fuori dal link.
### Link Local / Site Local

- **Link local**: `FE80::/64` sono indirizzi di autoconfigurazione che hanno validità solo sul link, ovvero sulla rete fisica. Genericamente si assegnato sulla base dell'indirizzo MAC della scheda di rete, a cui si aggiunge un prefisso.

- **Site local**: Sono di fatto indirizzi privati riconfigurabili, in forma `FEC0::/10` ma sono **deprecati**.

### Unique Local Addresses

Sono privati e non vanno routati su internet. Sono in forma `FC00::/7` e sono usati per dispositivi che non devono mai accedere ad internet e mai essere raggiunti da internet.

![[Pasted image 20231018181036.png]]

### IPv4 embedded addresses

Questi speciali indirizzi servono per rappresentare gli indirizzi IPv4 in IPv6:

![[Pasted image 20231018181401.png]]

In particolare l'**indirizzo v4 è scritto nei 32 bit meno significativi**, i primi 80 bit invece sono settati a 0, a cui seguono 16 bit a 1, ovvero `FFFF`.

# Anycast address

Sono indirizzi che **possono rappresentare più interfacce (genericamente di dispositivi diversi)**, esistono poi tecniche per raggiungere solo particolare host

# Struttura Header IPv6

La struttura è la seguente:

![[Pasted image 20231023131656.png]]

Una vera sostanziale differenza sta nell'introduzione del **flow label**, utile ad amministratori di rete per garantire una **maggiore qualità del servizio**.
I restanti campi sono simili o uguali a quelli che c'erano in IPv4.
**Hop Limiti** equivale al TTL, poi sono presenti l'indirizzo sorgente e destinazione.

### Extension header

Il campo Protocol IPv4 che conteneva il contenuto del payload ora si chiama **Next Header** che specifica il formato di header aggiuntivi agganciati al pacchetto.
L'ultimo next header conterrà il nome del protocollo realmente presente nel pacchetto:

![[Pasted image 20231023132458.png]]

Il formato di un **extension header** è il seguente:

![[Pasted image 20231023132547.png]]

### Hop-by-hop extension header

In particolare un **Hop By Hop Extension header** è un header che contiene informazioni che vengono analizzate da ogni router attraversato dal pacchetto.
Se presente è attaccato direttamente dopo l'intestazione IPv6: contiene i campi next header e lunghezza, seguiti da un campo per opzione e la relativa lunghezza 
Ha una struttura prefissata nella forma:

![[Pasted image 20231023132916.png]]

Altri extension header sono: Authentication, Encapsulating security payload, Routing, Fragment.

# Interfacciamento con i livelli inferiori

### Incapsulamento

I pacchetti IPv6 sono incapsulati all'interno dello strato 2 (collegamento). Approccio Dual Stack: le cose possono funzionare sia per v4 che per v6.

### Indirizzo MAC

Quando in un pacchetto a livello 2 contiene un header IPv6, allora dobbiamo mappare l'indirizzo IPv6 in un indirizzo MAC fisico.
Nel caso di indirizzo Unicast è presente un protocollo detto **neighbor discovery** mentre nel caso multicast esistono tecniche specifiche.

Per i multicast Ipv6 possiamo mappare il MAC address come segue:

![[Pasted image 20231023134231.png]]

Il prefisso 33-33 è riservato per il MAC address quando dentro è contenuto un Ipv6 multicast. I restanti 32 bit sono copiati.

### Neighbor discovery

In ICMPv6 è disponibile una nuova funzione che va a sostituire ARP: per non disturbare il traffico sulla rete al posto del broadcast si usa il multicast per raggiungere più stazioni, tramite un solicited node multicast address.
Si **presuppone** che solo una stazione verrà raggiunta dal multicast.
La strategia prevede di iscrivere ogni stazione a un gruppo multicast (dove c'è iscritto solo la stazione stessa).

Quando devo ottenere un MAC a partire da indirizzo IPv6 devo costruire un solicited node address nel seguente modo: i 24 bit meno significativi sono copiati, gli altri 104 sono fissi. 

![[Pasted image 20231023134905.png]]

Quando una stazione si configura a una rete si calcola il solicited node address e avvisa la scheda di rete che fa vuole fare parte di un gruppo multicast (associato al solicited node address).

In questo contesto la ARP request si chiama **ICMP Neighbor solicitation**: il destinatario della richiesta è proprio il solicited node address associato all'indirizzo IPv6 di cui voglio sapere il MAC.

```ad-warning
title: Probabilità di funzionamento ICMP Neighbor solicitation
Dato che gli indirizzi solicited node sono mappati sulla base dei 24 bit meno significativi, se ho due stazioni che condividono questi ultimi allora verranno mappati sullo stesso MAC.
In quel caso entrambe le stazioni riceveranno un pacchetto ma andando a spacchettare solamente la stazione corretta conserverà il pacchetto.

```

Quando la Neighbor solicitation è ricevuto, allora si manderà in risposta un **ICMP neighbor advertisement**, mandata in **unicast**.
I mapping (come per ARP) sono memorizzati in una cache temporanea.

# ICMPv6

Il pacchetto ICMPv6 ha il seguente formato:

![[Pasted image 20231023140230.png]]

Il campo **tipo** ha valori prefissati che specificano il tipo di richiesta/messaggio:

| Codice | Nome                     |
| ------ | ------------------------ |
| 1      | Destination unreachable  |
| 2      | Packet too big           |
| 3      | Time excedeed            |
| 4      | Parameter problem        |
| 128    | Echo request             |
| 129    | Echo reply               |
| 130    | Multicast Listener Query |
| 131    | M. L. Report             |
| 132    | M. L. Done               |
| 133    | Router Solicitation      |
| 134    | Router Advertisement     |
| 135    | Neighbor solicitation    |
| 136    | Neighbor Advertisement   |
| 137    | Redirect                         |

In particolare il pacchetto ICMPv6 Neighbor solicitation ha la seguente struttura:

![[Pasted image 20231023140906.png]]

dove all'interno di **target address** si inserisce l'IPv6 del destinatario.
La risposta ha la seguente struttura: (**Il MAC che ho richiesto si trova nelle opzioni**):

![[Pasted image 20231023141041.png]]

### Iscrizione ad un gruppo multicast

La **Multicast Listener Query** è una domanda che il router manda ai suoi host per verificare se sono interessati a far parte di un gruppo multicast, ponendosi in attesa di una risposta. La risposta con la quale un host comunica al router tale interesse è detto **Multicast Listener Report**.
La **Multicast Listener Done** consiste in un host che manda un messaggio di fine per avvisare che non è più interessato a ricevere i pacchetti multicast.

# Device configuration in IPv6

Sono necessarie le seguenti informazioni, ottenibili in diversi modi:
- Configurazione manuale (scomodo)
- **Stateful configuration** attraverso DHCP
- **Stateless configuration** senza l'intervento di un server 
- **Stateless DHCP (hybrid)**

Per la configurazione del **Interface ID** posso scegliere 3 modalità: in maniera manuale, ottenuto tramite DHCPv6 oppure tramite autogenerazione a partire fa EUI-64 MAC:

![[Pasted image 20231023144847.png]]

E' importante **cambiare il settimo bit del MAC a 1**, in quanto è stato scelto in IPv6 di avere il settimo bit di un interface ID univoco settato a 1.
Con questa modalità sarei facilmente rintracciabile in quanto cambierebbe solo il prefisso se cambio rete.

La modalità **privacy aware** genera tramite un certo standard un IPv6 a partire da una stringa random di 64 bit oppure da un precedente interface ID:

![[Pasted image 20231023145858.png]]

Occorre fare un bitwise AND con una maschera per garantire la correttezza del settimo bit.

Per configurare il prefisso:
- Configurato manualmente
- Generato dal DHCPv6
- Automatically generated (link local)
- Stateless (dal router)

Quest'ultima modalità sui messaggi ICMPv6 Router Solicitation e Router Advertisement: la richiesta viene mandata a tutti i router raggiungibili in locale.
Il formato della risposta è il seguente:

![[Pasted image 20231023150745.png]]

Nel messaggio di advertisement sono rilevanti alcuni parametri:
- M flag (Managed address Configuration): se è settato a 1 significa che l’indirizzo è stato configurato tramite DHCPv6.
- O flag (other configuration): se è settato a 1 sono presenti altre configurazioni, ad esempio DNS server.
- reachable time: tempo in millisecondi che il router impiega per raggiungere un host.
- retrans timer: intervallo di tempo per cui ritenere l’indirizzo valido.
- Option: sono presenti delle opzioni, in formato generico ovvero type, length (multipli di 8) e value.

![[Pasted image 20231023151308.png]]

Tra le opzioni c’è il prefix information option che ha sempre:
- lifetime: tempo di vita dell’indirizzo.
- preferred lifetime: periodo in cui non dovrei più utilizzarlo.
- L: se è utilizzato all’interno di un on‑link.
- A: il prefisso può essere utilizzato per una configurazione automatica.
- prefix: il prefisso.

Link layer address option: indirizzo MAC del mio default gateway. Se il default gateway invia il messaggio perché lo inserisco? per comodità dello stack iso/osi

### ICMP Redirect

Il redirect viene utilizzato per informare, all’interno di una stessa sottorete, un host A che per raggiungere un determinato host B è più conveniente utilizzare un altro router.
Se la comunicazione è a livello globale questo solitamente non avviene.

![[Pasted image 20231023152001.png]]

Le opzioni avranno il seguente formato:

![[Pasted image 20231023152024.png]]

### Duplicate Address Detection (DAD)

Il **Duplicate Address Detection** (DAD) è un meccanismo che permette di verificare che un indirizzo IPv6 sia unico all’interno della rete.
Il funzionamento è molto semplice: un host manda un messaggio ICMPv6 a tutti gli host con destinazione all nodes al IPv6 Solicited Node Multicast Address e al corrispondente MAC multicast address, con il payload contenente l’indirizzo che si vuole utilizzare. Se non vi è nessuna risposta entro 1 secondo, l’indirizzo viene considerato valido, se invece vi è risposta significa che l’indirizzo è già utilizzato in quanto un host ha risposto con un messaggio ICMPv6 di tipo DAD con il payload contenente l’indirizzo da utilizzare.

### Step per la configurazione stateless

- Genero un **link local address**
- Faccio la **DAD** per verificare l'unicità
- Mi iscrivo al gruppo multicast: primo creo il multicast solicited address e poi mando una multicast listener report (anche se inutile, in quanto quel nodo sarà l'unico di quel gruppo multicast).
- A questo punto la comunicazione on-link è abilitata

Nel caso mandassi una router solicitation o di ascoltare una router advertisement allora posso decidere di accettare un prefisso:
- Mi creo un nuovo indirizzo IP
- Verifico tramite DAD
- Mi iscrivo al gruppo multicast relativo a questo nuovo indirizzo.


### Step per configurazione stateful

Se nella router advertisement compare il Flag M pari a 1 allora procedo nel seguente modo:

```ad-note
title: Scoped addresses
Se in un dispositivo sono presenti più interfacce caratterizzate dallo stesso prefisso (e forse anche dallo stesso interface ID) allora si utilizza una notazione con il simbolo '%' per rendere gli indirizzi link local 'scoped'.

```


# Transizione v4 a v6

Inizialmente si è pensato di fare **tunnelling** tramite **GRE** oppure **IPv6 in IPv4**: prendo un pacchetto IPv6 e lo imbusto in uno IPv4: i pacchetti viaggiano sulla rete v4.
Sempre inizialmente si pensava di fare **dual stack**: supportare sia v4 che v6, ma ciò comporta la sdoppiatura di tutto lo stack protocollare.

### 6to4

![[Pasted image 20240116202147.png]]

Questa è una soluzione di tipo **network centered**:
- Si usa un indirizzo IPv6 particolare che embedda l'indirizzo IPv4 dell'interfaccia che mi consente di uscire dalla mia rete IPv6
- Se voglio contattare una rete IPv6 conosco il suo IPv4 perchè posso ricavarmelo
L'unico problema di questo approccio è il dover obbligatoriamente usare un indirizzo IPv6 dedicato 2002: ...

### Soluzioni carrier-grade, scalable

Normalmente ad oggi la maggior parte dei servizi in internet è raggiungibile sia in v4 che in v6 (dual stack): all'edge della rete vogliamo mantenere indirizzi privati, mentre nella rete dei provider si inizia ad usare IPv6.
(vedere immagine sotto)

![[Pasted image 20240116203715.png]]

Nella rete v6 del provider saranno presenti numerosi CPE (tanti clienti, tante aziende) ma il tunnel endpoint (per uscire su internet) è solamente uno (in realtà anche più di uno, ma non tantissimi)
Infine è presente l' **Address Family transition router** (AFTR).

### DS-Lite

Questa soluzione è simile allo scenario descritto sopra.
La soluzione adottata da DS‑Lite funziona come segue: da IPv4 privato si arriva sul home gateway (CPE) che effettua tunnel verso l’AFTR tramite una rete IPv6 su cui è installato un Large Scale NAT (LSN), in questo modo Un unico NAT gestisce tutti i clienti. 

![[Pasted image 20240116204626.png]]

- il cliente non ha controllo sul NAT.
- possono esserci problemi con i server in quanto static mapping e port forwarding non possono essere configurati.

### A + P

La soluzione A+P parte da un indirizzo IPv4 privato che arriva sul CPE e viene convertito in indirizzo pubblico e solo successivamente viene effettuato il tunnel IPv6 verso l’AFTR. Quando il pacchetto esce dal tunnel è già stato trattato dal NAT e dunque può andare verso la rete pubblica.

![[Pasted image 20240116204859.png]]

- Il vantaggio di A+P risiede nella possibilità per il cliente di avere sotto controllo il NAT
- nessun problema con la sovrapposizione degli indirizzi privati nello spazio di indirizzi dei customer

### NAT64 + DNS64

Il processo si divide nei seguenti passi:
1. Un host IPv6 esegue una query DNS di tipo IPv6 AAAA.
2. DNS64 inoltra la query dal DNS autoritativo verso il dominio della richiesta.
3. Il DNS autoritativo (che si trova in una rete IPv4) non può ricevere la richiesta, per questo motivo viene ripetuta per un indirizzo IPv4 (necessaria rete IPv4+IPv6 intermedia).
4. DNS64 riceve l’indirizzo IPv4 che risolve il dominio, ne esegue l’embedding in un indirizzo IPv6 con un prefix di default 64:FF9B.
5. L’indirizzo IPv6 viene restituito al host che ha fatto la richiesta.

![[Pasted image 20240116205340.png]]