# Repeater

Si tratta di un dispositivo che opera a livello 1: si occupa semplicemente di ripetere **bit che riceve e di propagarli**.
Inoltre è in grado di passare da un mezzo fisico all'altro (da rame a fibra).
Si utilizza per connettere delle reti caratterizzate **dallo stesso indirizzo MAC**, oppure un'altra importante applicazione è quella di ripristinare un segnale degradato su lunghe distanze.

![[Pasted image 20231113134237.png]]

In altre parole, un repeater è in grado di far comunicare due domini fisici differenti: purtroppo però **unisce anche i domini di collisione**.
Quando un repeater ha più di 2 porte di parla di **Hub**.

# Bridge/Switch

E' un dispositivo che lavora a livello 2 che serve per connettere diversi protocolli di livello 2 (ad esempio si passa da ethernet a fast ethernet).
Inoltre posso passare da un tipo di trama a un'altra.

Il funzionamento di un bridge si divide in 3 fasi:
1. Si memorizza il pacchetto (è un nodo **store and forward**)
2. Si modifica opportunamente la trama per cambiare il protocollo (ad esempio da Ethernet a Token Ring)
3. Si inoltra il pacchetto

I Bridge sono in grado di **dividere i domini di collisione**, andando inoltre a lasciar intatto il broadcast che continuerà a funzionare correttamente. 

![[Pasted image 20231113140651.png]]

Un'altra che posso fare per aumentare la velocità di comunicazione è usare due link per permettere la trasmissione e la ricezione simultaneo (in sostanza il **full duplex**): quest'ultimo è implementabile solo nel caso di un nodo che possa temporaneamente memorizzare il pacchetto e non inoltrarlo subito.
Così facendo, la banda raddoppia a livello teorica (in realtà gli hosts saturano il downlink, mentre i server l'uplink) e inoltre si eliminano le collisioni (il CSMA/CD non è più necessario).

Nel caso di un bridge multi-porta, si parla di **switch**.

### Bridge trasparenti

All'interno di una LAN vengono definiti **switch trasparenti**. In particolare per trasparenza si intende che:
- Lo switch deve essere plug and play e non deve richiedere riconfigurazioni negli hosts.
- Le performance del sistema possono essere diverse, ma le funzionalità devono rimanere le stesse

Quindi gli end systems (ovvero gli hosts) opereranno nella stessa maniera di sempre, ovvero usando gli stessi protocolli, stessi formati, stesse trame.

Ogni interfaccia degli switch ha un indirizzo MAC, ma non viene mai usato se non per messaggi generati dagli switch stessi per motivi di configurazione.

### Forwarding process

Le regole sono semplicemente due:
- Unicast: lo switch è in grado di fare forwarding **solo sulla porta dove si trova la destinazione**.
- Multicast o Broadcast: si esegue il **flooding**, ovvero si inoltra su tutte le porte tranne quella sorgente (Spesso non sempre in modo simultaneo).

Per implementare queste regole servono 3 componenti:
- Una tabella locale di forwarding ([[#Filtering Database]])
- Stazioni che imparano autonomamente ([[#Backward learning]])
- Individuazione dei loop (**Spanning tree algorithm**)

### Filtering Database

Si tratta di una tabella con le posizioni di ogni MAC address presente nella rete locale.
Per ogni entry si tiene traccia del MAC, della porta tramite cui raggiungerlo e l'**ageing time**.

![[Pasted image 20231113145819.png]]

Questa tabella può essere configurata **dinamicamente** tramite gli algoritmi di backward learning, oppure **staticamente** (a mano).

### Backward learning

L'idea di base è quella di **investigare sui frame ricevuti** dallo switch: se ricevo un pacchetto con sorgente H1 dalla porta P1, allora significa che H1 è raggiungibile dalla porta P1. L'indirizzo destinazione viene ignorato da questo algoritmo.

Questo principio funziona anche in presenza di switch multipli nella stessa LAN:

![[Pasted image 20231113150342.png]]

E' possibile che gli host si spostino all'interno della rete, e se quest'ultimi non generano traffico è difficile che le tabelle si aggiornino.
Nel caso si generino dei pacchetti broadcast, allora sono sempre ricevuti correttamente in quanto raggiungono tutta la rete e quindi si mantengono le reti aggiornate.
Nel caso si unicast spesso alcune porzioni della rete non sono aggiornate e quindi è possibile che i pacchetti si perdano.

```ad-danger
title: MAC Flooding attack
Se si generano tantissimi pacchetti con informazioni casuali e MAC sorgente randomici è possibile **riempire la forwarding table** di uno switch. Così facendo lo switch sarà costretto a fare flooding dei nuovi pacchetti che riceve, e sarà possibile (da parte dell'attaccante) di intercettare il traffico sparso sulla rete (Oltre che di rallentarla).

```

### Spanning tree algorithm

Consideriamo che l'host H1 mandi un pacchetto in broadcast su una rete in cui sono presenti più switch: si genera la seguente situazione.

![[Pasted image 20231113151320.png]]

Questo perchè alcuni switch possono avere delle forwarding table **inconsistenti**. La soluzione consiste nell'evitare che nella rete fisica si creino dei loop:
- Posso creare fisicamente delle reti senza loop
- Posso avere un **algoritmo che disabilita temporaneamente i loop** (meglio)

L'algoritmo è definito dallo standard 802.1D: si identificano i nodi e la **rete diventa un MST che ha un percorso unico fra qualsiasi sorgente e la relativa destinazione**.

# Router

Sono dispositivi al livello 3 che connettono diverse reti e servono per interfacciarsi a internet. Funzionano col protocollo [[Ipv4]] oppure [[IPv6]]
Non sono dispositivi trasparenti: **separano i domini di broadcast**.

![[Pasted image 20231113152018.png]]

# L2 vs L3

Consideriamo le caratteristiche viste finora e notiamo che i pro del livello 2 sono superiori a quelli del livello 3: di base funziona tutto con qualsiasi protocollo di rete e ovunque io mi trovi nella rete.

Quindi **scegliamo di usare L2**: ora il problema diventa scegliere se usare una singola LAN o più LANs connesse in qualche modo.
Per ragioni di sicurezza e performance la strada più conveniente è quella di usare LAN multiple.
Per la sicurezza si pensi al MAC flooding attack, mentre per le performance si pensi al traffico broadcast di una singola rete LAN con tantissimi dispositivi.

# VLAN

Le **Virtual LANs** consentono a un certo insieme di porte di uno switch di simulare di far parte di domini di broadcast separati, nonostante facciano parte della stesse rete fisica.
Per consentire la comunicazione fra le VLAN è necessario un router (se le sottoreti sono collegate a un'unica interfaccia allora si parla di **one arm router**).

```ad-danger
title: Frame L2 fra VLAN 
Le trame di livello L2 non possono attraversare le VLAN, in quanto i domini sono separati.

```

![[Pasted image 20231115162111.png]]

Quando un pacchetto attraversa il router si modifica la trama, mettendo come destinazione (ipotizzo H1 -> H2) il MAC di H2.
Il **broadcast non può attraversa i confini delle VLAN**, inoltre anche **ARP non può intervenire**, infine dispositivi connessi a VLAN diverse devono avere un indirizzo IP di rete differente.

### Associare i frame alle VLAN

Il problema che si genera è quello di identificare da quale VLAN un frame provenga: quando uno switch riceve un frame può segnarsi da quale VLAN proviene, ma non si altera la trama l'informazione è memorizzata solo in uno switch e non in tutti.
Si introduce perciò il campo **tagging** di 4 byte che contiene il **VLAN-ID** 

![[Pasted image 20231115162933.png]]

Dobbiamo perciò effettuare piccole modifiche, come estendere la lunghezza della trama di 4 byte.

Le **porte** si dividono in:
- **Access port**: ricevono e trasmettono frame **untagged**, spesso è la configurazione standard su host, switch e inoltre si usano spesso per connettere gli end system alla rete.
  Queste informazioni **non vengono propagate** al di fuori dello switch considerato  
  ![[Pasted image 20231115175804.png]]

- **Trunk port**: trasmettono e ricevono i frame **tagged**, devono essere configurate esplicitamente e sono spesso usate per connettere due switch.
  ![[Pasted image 20231115163958.png]]

### Assegnare gli host alle VLAN

Un importante problema è la **mobilità degli host**, che possono cambiare luogo d'accesso. Per mantenere un mapping costante posso **associare il MAC address** per farmi riconoscere sempre: questo richiede una struttura dati che tenga in memoria i mapping (MAC address, VLAN-id) e ogni volta che un host si connette allo switch, quest'ultimo deve fare il controllo.

Un secondo modo è il **per-user assignment** dove per connettersi alla rete occorre **autenticarsi** con username e password, e dunque potrò accedere alle VLAN corrette.

Un terzo ed ultimo tentativo è quello del **cooperative assignment**, dove è l'host stesso a 'taggare' la sua trama (comporta problemi di consistenze e di sicurezza).

E' possibile che un host debba essere assegnato a più VLAN: sono dunque necessarie delle **trunks ports sul dispositivo stesso** (usando cooperative assignment); spesso questo scenario è per dispositivi particolari gestiti manualmente.

### VLAN e Spanning tree

In pratica le due tecnologie sono spesso indipendenti: quindi lo spanning tree abilita e disabilita certi link; successivamente sulla topologia ottenuta si costruiscono le VLAN.

Spesso si usano soluzioni proprietarie: ci possono essere problemi di incompatibilità.

### VLAN isolation

Nonostante le trame non possano attraversare i bordi delle VLAN, i collegamenti fisici sono condivisi: ciò significa che comunque una broadcast storm affligge i canali condivisi della VLAN (Consuma risorse, un collegamento potrebbe risultare comunque congestionato e non poter trasmettere). Così come un guasto su un link di una VLAN affligge anche le altre.

Per ovviare a ciò usiamo dei meccanismi di **QoS** (quality of service) per VLAN: un esempio di questo è il round robin, dove prima trasmette uno, poi l'altro, poi uno, l'altro...

### Switch types in VLAN

- **VLAN aware switch**: possono gestire frame tagged
- **VLAN unaware switch**: non accettano frame tagged

La maggior parte dei dispositivi domestici non hanno il supporto per le VLAN, quindi le VLAN non sono più tecnologie plug and play perchè in genere gli utenti non hanno le competenze per configurarle.

Normalmente in aziende è molto comune combinare l'uso di aware e unaware VLAN switch.

![[Pasted image 20231115182728.png]]
