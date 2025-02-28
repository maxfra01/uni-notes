# Introduzione

Le principali componenti delle reti wireless:
- **Wireless hosts**
- **Base station**: esistenti in tutte le tipologie di connessioni mobili (in WiFi si chiamano Access Point), sono incaricate di scambiare pacchetti con tutti gli hosts nell'area
- **Wireless link**: è il collegamento fra gli hosts e la base station, bisogna opportunamente gestire il problema degli accessi multipli

Ogni tipo di tecnologia wireless è pensata per un dato scopo, e quindi ogni tipo ha le sue caratteristiche:

![[Pasted image 20231025174735.png]]

Le **base station** sono connesse alla rete cablata, e il loro scopo è proprio quello di connettere gli hosts a questa rete. Il meccanismo di **Handoff** prevede che a un host venga fornita l'accesso alla rete cablata (tramite base station) anche se quest'ultimo cambia area (e quindi base station).
Esiste un'altra modalità di collegamento wireless senza base station: i vari nodi si scambiano i dati direttamente fra loro e devono essere nella reciproca area di copertura.

Nonostante la comodità del mezzo le connessioni wireless hanno importanti differenze con la rete cablate:
- Man mano che il segnale si propaga nel mezzo, esso si **degrada** molto facilmente (ostacoli nel mezzo, interferenze)
- **Interferenze con altre sorgenti**, ad esempio la frequenza 2,4 GHz è condivisa da molti dispositivi
- **Multipath propagation** (fading): i segnali tendono a rimbalzare sugli ostacoli, arrivando a destinazione in copie e tempi diversi.

Definisco i seguenti indici: **SNR (Signal Noise Ratio)** e **BER(Bit Error Rate)**.
Quando mando informazioni in wireless devo effettuare delle modulazioni più o meno aggressive.
Devo selezionare una modulazione che vada a definire un trade off tra SNR e BER per l'applicazione che devo realizzare. E' anche possibile usare tecniche che cambino **real-time** la modulazione a seconda delle condizioni del canale.

Un altro problema delle reti wireless è quello del **terminale nascosto e delle distanze**:

![[Pasted image 20231025180919.png]]

# Wireless LAN

Negli anni si sono sviluppati diverse standard (802.11.xx), andando anche a modificare la frequenza alla quale si opera. In ogni caso tutte le tecniche usano **CSMA/CA**.
L'infrastruttura base di una wireless LAN è costituita da una base station, detta **Access Point (AP)**: l'insieme di host wireless e access point viene detta **Basic Service Set (BSS, 'cella')**.

La connessione avviene dividente lo spettro delle frequenze in diversi **canali**:
1. L'access point andrà a selezionare quello più libero (per evitare interferenze) a una determinata frequenza.
2. Fatto ciò l'AP manda continuamente un **beacon frames** (contiene il SSID, nome della rete wireless)
3. Gli host fanno scan dei vari canali e (sentendo il beacon) scelgono se connettersi a quella rete
4. Eventualmente dovranno fare una autenticazione
5. Infine tramite DHCP l'host ottiene una configurazione

Questo processo descritto sopra è il **passive Scanning**, ma esiste anche **active scanning**: è l'host a mandare una **Probe Request** per attivare l'access point e iniziare la configurazione.

### CSMA/CA

Il principio base consiste nell'**ascoltare il canale prima di trasmettere**. Il rilevamento della collisione è difficile da realizzare in wireless, perciò si parla di 'collision avoidance':

1. Il mittente ascolta il canale per un tempo detto **DIFS**, e se lo trova libero inizia a trasmettere.
	- Se lo trova occupato inizia un **random exponential backoff**, che consiste in un tempo casuale che aumenta ad ogni rilevamento di canale occupato
	
2. Se il frame è ricevuto dalla destinazione, allora quest'ultima manda un **ACK** dopo un tempo **SIFS** (tempo minore di DIFS, per avere priorità su gli altri nodi che vogliono trasmettere).

Questa modalità **non garantisce l'assenza di collisioni**.
La modalità RTS (request to send) permette di prenotare il mezzo per mandare frame e quindi evitare collisioni.
Il messaggio di CTS (clear to sent) mandato dall'AP specifica il proprietario del canale per quel tempo.

![[Pasted image 20231025182858.png]]

```ad-note
title: RTS-CTS
Questo meccanisco effettivamente elimina le collisioni ma non tutte le reti lo implementano.

```

### Frame addressing

![[Pasted image 20231030131802.png]]

La prima cosa che notiamo è un quadruplo campo per l'indirizzamento.
Il frame contiene:
- **frame control**: 

	![[Pasted image 20231030132530.png]]

	Il campo type specificano se si tratta di una trama RTS, CTS, ACK, data.
	Power Mgt serve per implementare funzioni di risparmio energetico

- **duration**: è il tempo riservato da una certa stazione quando usa la procedure RTS,CTS per evitare le collisioni.
- **address** 1: mac address del host wireless o Access Point che deve ricevere il frame
- **address** 2: MAC address del host wireless o Access Point che deve trasmettere il frame
- **address** 3: MAC address dell’interfaccia del router a cui l’access point è connesso
- **seq control**: necessari per gli ack
-  **address** 4: usato solo in modalità ad hoc
- **payload**
- **CRC**: controllo di errore

### Mobilità nella stessa sottorete

Solitamente per le reti wireless l’host rimane all’interno della stessa subnet IP, motivo per cui è possibile riutilizzare lo stesso indirizzo.
Spesso gli switch sono **self learning**, ovvero quando vedono un frame transitare verso un host ne memorizzano la porta a cui è stato inviato.

Dal punto di vista energetico, esiste il **node‑to‑AP** attraverso il quale l’Access Point viene a conoscenza del fatto che non deve inoltrare i frame al nodo, il quale si sveglierà prima del prossimo beacon frame (ha al suo interno la lista dei dispositive con gli AP‑to‑mobile frames in attesa di essere inviati).

# Reti cellulari

La principale caratteristica delle reti cellulari è l'estensione della copertura sulla stessa BSS: sono pensate per coprire aree geografiche molto estese, grazie a celle più grandi.
In questo contesto diventa una priorità garantire continuità di servizio, grazie al meccanismo di **handover**.

Le celle sono approssimativamente **esagonali**, con un antenna **isotropica** (omnidirezionale), oppure secondo un'altra configurazione si hanno 3 antenne unidirezionali a 120°.
Nella realtà le celle non sono esagonali, a causa di **ostacoli** geografici nell'area interessata, la potenza dell'antenna, il suo guadagno e le condizioni atmosferiche.

Come nelle reti wireless, è nuovamente presente il problema di accesso multiplo condiviso sul canale, che viene risolto attraverso varie tecniche (corrispondo alle modulazioni):
- **FDMA**: viene scelto una frequenza in cui trasmettere.
- **TDMA**: viene scelto uno slot temporale in cui trasmettere.
- **CDMA**: viene assegnato a ogni stazione un codice ortogonale agli altri, ovvero un gruppo di segnali da cui è possibile recuperare ogni singolo segnale.
- **SDMA**: ogni frequenza viene riutilizzata, a condizione che i luoghi siano fisicamente molto distanti tra loro.

Verranno riutilizzate le stesse frequenze in posti diversi in modo da non causare interferenze. Questo viene fatto a causa del ridotto numero di risorse, nel tentativo di coprire un’area più ampia e servire un maggior numero di utenti.

### Cluster di celle

Un gruppo di celle vicini viene detto **cluster**, e le celle che fanno parte dello steso cluster sono dette **co-channel**.
Con la variazione della dimensione delle celle R cambia la capacità, ovvero il numero di utenti che questa è in grado di soddisfare. Il numero di celle G impatta invece sul costo, in quanto un numero maggiore di celle ha dei costi maggiori. Aumentando il cluster aumento la qualità, aumentando anche
G aumenta la qualità ma diminuisce la capacità.

![[Pasted image 20231030140827.png]]

Non esiste una legge assoluta per definire i due parametri, ma è possibile sfruttare alcune tecniche per diminuire le interferenze ed aumentare la capacità:
- **splitting**: non utilizzare celle delle stesse dimensioni, ma basarsi sulle necessità specifiche.
- **sectoring**: utilizzare delle antenne non omnidirezionali per ridurre le interferenze e ridurre solo nelle direzioni in cui non è necessario.
- **tilting**: non usare un angolo a 90 gradi per la trasmissione 8cercando di variare l’angolo).
- **Creazione di femptocelle**: possiamo creare delle celle non fisse in base alle necessità (esempio stadio o concerti).

E' possibile anche dare forme particolare alle celle per usi specifici (ad esempio per coprire un autostrada).
Un'altra tecnica è l'**Umbrella-shaping** andando a sovrapporre più livello di copertura, andando poi ad agganciarsi alla cella più conveniente.

### Power control

Il Power Control mira al gestire al meglio le capacità delle batterie con l’obbiettivo è di ridurre l’utilizzo di potenza in base alle necessità e in relazione alla qualità di trasmissione e alla distanza tra cellulare e antenna. Per effettuare la regolazione della potenza necessaria si utilizzano strategie di
due tipi:
- a catena **aperta** (open loop): sistema senza reazione
- a catena **chiusa** (closed loop): sistema con reazione (feedback)

### Allocazione delle frequenze

Diverse metodologie per allocare le frequenze nelle celle:
- **FCA Fixed channel allocation**: l'operatore in maniera statica sceglie le frequenze che preferisce anche in maniera non uniforme
- **DCA Dynamic channel allocation**: Approccio molto flessibile che consente di scegliere e cambiare le frequenze
- **HCS Hybrid Channel allocation Scheme**: soluzione intermedia, una porzione in FCA e un'altra in DCA

### Architettura di rete

I dispostivi mobile, **MT** si connettono alle base station **BSS** (su cui si montano le antenne). I BSS sono collegati ai **core network** tramite collegamenti via cavo, così come la core network che si gestisce quasi totalmente a cavo.

![[Pasted image 20231105163739.png]]

### Roaming

Il **roaming è la capacità di un terminale di essere tracciabile quando si sposta nella rete**. Il sistema deve memorizzare la posizione in un database e localizzare l’utente quando necessario. Per salvare tali informazioni, la rete viene divisa in location areas (LAs), ovvero gruppi di celle adiacenti ciascuna
con un identificativo univoco

Tramite il **Location updating** si tiene informato il database quando un utente cambia cella.

### Paging

Il **Paging** è la **procedura attraverso la quale il sistema notifica un terminale mobile di una chiamata o data delivery**.

Il sistema manda la richiesta in broadcast a tutti i terminali della location area, e il terminale che riceve la richiesta risponde con un messaggio di conferma.

### Handover

La procedura di Handover abilita il **trasferimento di una connessione attiva da una cella verso un’altra**, mentre il terminale mobile si sposta nella rete. Questa procedura è molto complessa e richiede una rete ben architettata, con protocolli e segnali adeguati.
Si classifica nei seguenti tipi:
- Intra vs. Inter Cell: Indica se l’handover avviene tra frequenze all’interno della stessa cella o di celle diverse.
- Soft vs. Hard: Indica se durante l’handover sono attivi entrambi i canali radio (soft) o solo uno alla volta è attivo (hard).
- MTvs.BSinitiated: Indica se il primo messaggio di controllo per l'avvio di un handover è invitato dal terminale mobile (MT initiated) o dalla BSS (BS initiated), ovvero quale entità esegue le misure per capire dove e quando deve essere eseguito un handover.
- Backward vs. Forward: Indica se la segnalazione di handover avviene tramite la BS di origine (backward) o la BS di destinazione (forwarding).

# GSM

Il **GSM** è una rete a circuito con full rate di 13 kbit/s e half rate di 6.5Kbit/s. Consente l’invio di SMS e servizi supplementari come call forward, recall, e busy tone.

I **Mobile Station** (MS), ovvero i dispositivi, sono quelli in grado di connettersi alla rete GSM.
La MS è però unicamente hardware, per connettersi alla rete è necessaria una **SIM**, ovvero una smart card con un processore e una memoria in grado di memorizzare, crittografate, le informazioni dell’utente come il numero di telefono, i servizi accessibili, parametri di sicurezza ecc. L’identificativo univoco della SIM si chiama **MSI**.

### BSS

La Base Station Subsystem (BSS) comprende:

- **Base Transceiver Station** (BTS): interfaccia fisica con il compito di trasmettere e ricevere. Rappresenta il punto d’accesso per i dispositivi e a differenza di altri sorgenti di segnale (ad esempio radio e TV) trasmette segnale solo verso gli utenti attivi. Arriva fino a 32 canali FDM per BTS.

- **Base station controller** (BSC): gestisce il controllo delle risorse sull’interfaccia radio. I BSC e i BTS comunicano mediante un collegamento cablato. Un BSC controlla un alto numero di BTS (da decine a centinaia). Tipicamente, i BSC sono collocati con un MSC (Mobile Switching Center, ovvero switch a circuito per la creazione di un circuito end‑to‑end), invece di essere allocate vicino ai BTS.

### Network and Switching Subsystem (NSS)

Questo sistema composto da più componenti si occupa di **gestire le chiamate, mobility e service support e l'autenticazione**.

![[Pasted image 20231103114919.png]]

- **MSC mobile switching center**: deve gestire il mobility support e il **call routing** tra un MT e GSMC (interfaccia fra GSM e altre reti)
- **HLR home location register**: ha il compito di salvare in database dati importanti per gestire le comunicazioni (anche dati permanenti, servizi, parametri di sicurezza)
- **VLR visitors location register**: salva nel database le  informazioni relative a dove si trova il dispositivo (MT) attualmente nell’area controllata dal MSC (come id, stato on/of, LAI, informazioni di routing e sicurezza)
- **AUC authentication center**: si occupa dell'autenticazione tramite protocolli come **challenge and response** con generazione di chiavi crittografate.
- **EIR equipment identity register**: memorizza informazioni dei dispositivi rubati.
### Canali fisici

Le frequenze effettive usate per il GSM sono 850, 900, 1800, 1900 MHz. Inoltre si utilizzano tecniche di **FDD (Frequency Division Duplex)** che prevede due frequenze diverse per downlink e uplink.

I canali GSM sono composti da una **frequenza** e uno **slot**, che identificano un canale fisico. Le trasmissioni sono organizzate in **burst** (da non confondere con pacchetti), ovvero blocchi di dati trasmessi su canali fisici. Sono simili ai pacchetti, ma funzionano su switching a circuito. La velocità di trasmissione è di 272 kbit/s.
SI può accedere ai canali tramite Frequency division multiple access (FDMA) o Time division (TDMA).
Le frequenze sono divise in **FDM Channels** (200 kHz ciascuno), a loro volta divisi in **TDM frames** (8 slot da 0.577 ms) 

![[Pasted image 20231103120318.png]]

Il GSM prevede una comunicazione in cui un MT manda un **burst in un singolo slot, restando silenzioso per i successivi 7 slot**.

![[Pasted image 20231103120554.png]]

# LTE

Caratterizzato di connessioni dell'ordine di 300 Mbit/s per il downlink, è stato standardizzato da 3GPP.
Questa generazione di connessione è caratterizzata dall'uso di antenne di tipo **MIMO**. Per l'accesso al mezzo abbiamo l'utilizzo di **OFDMA (Orthogonal)**.

La rete LTE è totalmente basata sull'uso di **IP**.
Le frequenze usate per LTE vanno dai 2600 Mhz fino a 800 MHz: le frequenze più alte hanno maggiori velocità ma peggior copertura (usate per fare microcelle in zone molto popolate).

L'architettura non cambia, la RAN si chiama **E-UTRAN**, mentre la core network in LTE si chiama **EPC**.
I mobile terminal di chiamano **UE User Equipment**, mentre i BSS si chiamano **eNodeB**.

### EPC evolved packet core

E' stato completamente riprogettato da zero.

Le **funzioni principali** di EPC sono:
- **Network access control**: include network selection, authentication, authorization, admission control, policy e charge enforcement e infine lawful interception.
- **Routing** e trasferimento di pacchetti.
- **Sicurezza**: include cifratura, integrity protection e network interface physical link protection.
- Gestione della **mobilità** per tenere traccia della posizione corrente all’interno del User Equipment (UE).
- Radio resource management per assegnare, riassegnare e rilasciare le risorse radio prese dalle singole o multiple celle.
- Gestione della rete per operazioni di manutenzione.
- Funzionalità di networking IP per le connessioni di eNodeB, condivisione di E‑UTRAN, supporto in condizioni di emergenza e altre.

![[Pasted image 20231103122139.png]]

Principali componenti:
- Mobility Management Entity (**MME**): si trova all’interno del control plane, supporta equipment context, identity, authentication e authorization. Perlopiù esegue procedure di tipo Non Access Stratum che si dividono prevalentemente in funzioni relative al bearer management e funzioni relative alla connessione e alla gestione della mobilità.
- Serving Gateway (SGW): si trova all’interno del User Plane, riceve e invia i pacchetti tra gli eNodeB e la core network. Esegue il packet routing e forwarding tra gli EPC, oltre al lawful intercept. E’ uno dei punti chiave per la intra LTE‑mobility.
- Packet Data Network Gateway (PGW): si trova all’interno del user plane, connette l’EPC con le reti esterne/internet ed esegue operazioni di assegnamento UE IP, user packet filtering e servizi di NAT. E’ uno dei punti chiave per l’accesso di reti non 3GPP.
- Home Subscriber Server (HSS): database di informazioni relative all’utente e agli iscritti. Viene utilizzato, insieme al MME, per l’autorizzazione.  Funziona in modo simile al HLR dell’architettura GSM.

### LTE Bearers

Ogni volta che un utente si aggancia alla rete LTE allora si crea un **default bearer** che parte dal UE e arriva fino al P-GW.
All’interno della rete i tunnel possono essere creati per soddisfare dei requisiti in termini di qualità del servizio, creando bearer dedicati a servizi specifici.

- **S5 Bearer**: connette il Serving Gateway (S-GW) al P-GW
- **S1 Bearer**: connette gli eNodeB al S-GW
- **radio bearer**: connette un UE con un eNodeB, quando si cambia cella c'è handover.

![[Pasted image 20231103123457.png]]

### E-UTRAN

La E‑UTRAN consiste principalmente di eNodeB con un interfaccia X2 per connettere gli eNodeB (due tipologie: X2 control e X2 user).

Le funzioni principali sono:
- Gestione delle risorse radio come radio bearer control, radio mobility control, scheduling ed allocazione dinamica delle risorse radio per uplink e downlink.
- Compressione (senza perdita) degli header.
- Sicurezza
- Connettività verso EPC

### Data plane e Control plane

In LTE avviene una separazione netta tra il Control Plane e il Data Plane. Il Control Plane introduce nuovi protocolli per la gestione della mobilità, sicurezza e autenticazione.

Il Data Plane, analogamente, vede l’introduzione di nuovi protocolli al livello di data link (2) e fisico (1). Inoltre, è presente un uso esteso di tunnel per facilitare la mobilità.

![[Pasted image 20231103123920.png]]

A livello 3 è utilizzato IP mentre a quello data link (2) sono presenti tre sottolivelli:
- **medium access**: equivalente del sottolivello mac, si occupa dell’accesso al canale
- **Radio Link Control (RLC)**: si occupa della frammentazione e assemblaggio dei dati. Offre un reliable data transfer, ovvero si assicura che la comunicazione avvenga con successo.
- **Packet data convergence**: compressione dell’header e della cifratura.

Il livello fisico è gestito attraverso **OFDM** (Orthogonal Frequency Division Multiplexing), ovvero tante frequenze ortogonali che minimizzano l’interferenza tra i canali. Inoltre, sono definiti degli slot **TDM** (non diversamente dalla gestione del canale link wireless su GSM).

Il downstream channel (canale di scaricamento) utilizza FDM, TDM within frequency channel (OFDM ‑ orthogonal frequency division multiplexing).

Per upstream si utilizza FDM, TDM similar to OFDM. Ciascun dispositivo attivo alloca uno o più slot di 0,5 ms su 12 frequenze. L’algoritmo di scheduling non è standardizzato, ma è lasciato all’operatore. E’ possibile raggiungere fino a 100 Mbps per dispositivo.

Qui abbiamo tanti piccoli slot che devono essere assegnati dalla rete in modo dinamico, in modo da adattarsi a quello che deve essere inviato in modo efficiente.

I bit trasmessi sono inseriti all’interno di un frame che ha una struttura suddivisa in modo predefinito denominata Physical channels. Ciascun channel ha informazioni specifiche relative a user data, TX/RX parameters, eNodeB identity, network control e molto altro, oltre al format del canale stesso. Ciascun canale fisico è mappato in una porzione di un subframe LTE. I canali fisici sono divisi in downlink e uplink channels, ciascun u/d channel è ulteriormente diviso in data e control.

In uplink è possibile utilizzare gruppi di 3 TTIs per aumentare la performance e ridurre l’overhead dei livelli superiori..

La tecnologia tunneling utilizzata per le reti cellulari si chiama GPRS Tunneling Protocol, ovvero tunnel realizzati su UDP.

# Mobilità in 4G/5G

Nelle reti cellulari la mobilità è gestita chiedendo alla rete di riferimento dove l’utente si trovi (stesso approccio di trovare una persona di cui non si conosce la persona, come chiamare a casa per chiedere ai genitori dove sia). E’ presente una home network e una visited network dove faccio roaming. Quando accedo alla visiting network la nuova rete mi assegna un indirizzo (spesso privato). Devo dunque dialogare con MME di quella rete in modo che possa indicare al HSS che mi trovo attualmente nella sua rete. 

Quando un **utente si sposta** devo gestire 4 fasi:
1. associazione alla nuova base station
2. configurare la control plane informando la rete dove si trova il dispositivo
3. configurazione della data plane per la creazione dei tunnel
4. mobile handover, se la cella dovesse cambiare (ad esempio durante la chiamata) dovrebbe essere eseguito l’handover

La **configurazione della data plane tunnel** per i dispositivi avviene:
- S‑GW a BS tunnel: quando il dispositivo cambia base station, semplicemente cambia l’endopoint IP address del tunnel
- S‑GW a home P‑GW tunnel: implementazione del routing indiretto
- tunneling via GPT (GPRS tunneling protocol): i datagrammi del dispositivo vengono inviati allo streaming server incapsulati utilizzando GTP inside UDP, all’interno del datagramma.

![[Pasted image 20231103124937.png]]

L’**handover** attraverso le base station all’interno della stessa rete cellulare avviene in quattro step:
1. il source BS seleziona il target BS, invia un Handover Request message al target BS
2. Il target BS prealloca un radio time slots, risponde con HR ACK con le informazioni del dispositivo
3. Il source BS informa il dispositivo del nuovo BS (ora il dispositivo può inviare e ricevere attraverso la nuova BS) e l’handover risulta completato agli occhi del dispositivo
4. Il source BS smette di inviare i datagrammi al dispositivo, invece li inoltra alla nuova base station (che li inoltrerà al dispositivo attraverso il radio channel)
5. Il target Bs informa MME che del nuovo BS per il dispositivo (MME istruisce S‑GW di cambiare l’endpoint del tunnel al nuovo BS)
6. La base station target inoltra un ack alla base station sorgente informando che l’handover è completato e la bs sorgente può rilasciare le sue risorse.
7. I datagrammi del dispositivo possono ora utilizzare il nuovo tunnel dal target BS al S‑GW

[[Ipv4]]
[[LAN]]