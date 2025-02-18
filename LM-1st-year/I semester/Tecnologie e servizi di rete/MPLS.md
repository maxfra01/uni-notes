# MPLS

**MPLS** è una importante tecnologia che permette la realizzazione di una **rete pubblica basata su IP**, dove con rete 'pubblica' si intende una rete con traffico di diversi utenti e aziende su cui è possibile vendere servizi.
In passato per mettere d'accordo tutte le possibili tecnologie (e metterle d'accordo con IP) si usava una struttura 'a cipolla' : MPLS si pone come obiettivo **eliminare la 'cipolla'** abbattendo i costi dei provider.

![[Pasted image 20231219150952.png]]

La promessa di MPLS:

![[Pasted image 20231219151027.png]]

### Funzionamento di MPLS

Normalmente un router quando riceve un pacchetto fa un **longest prefix matching** (LPM), ma un lookup in una routing table grande è molto costoso: MPLS va ad aggiungere al pacchetto un **etichetta** (label), che viene usata per fare forwarding del pacchetto (al posto dell'IP destinazione). Nel caso di una **label** piccola si vanno ad aumentare notevolmente le prestazioni.

![[Pasted image 20231219151853.png]]

Questa label può essere associata a più cose: non solo a indirizzi IP destinazione. Di fatto MPLS trasforma **IP** da connection-less a **connection oriented**.

### Architettura di rete

- Una porzione di rete che usa MPLS si chiama **MPLS Cloud**. 
- I dispositivi intermedi (router) del cloud sono detti **Label Switch Router** (LSR) ed essi si occupano di cambiare la label quando ricevono un pacchetto: commutazione di pacchetto basato su label
- I router posti a bordo della rete sono i **Label Edge Router** (Ingress / egress LSR): sono coloro che appendano la label all'ingresso e la tolgono all'uscita.
- Il percorso che il pacchetto segue è detto **Label Switch path** (LSP)

![[Pasted image 20231219152923.png]]

Così facendo l'etichetta viene cambiata ad ogni nodo, e il **vantaggio** è quello di usare **un unico protocollo per gestire la comunicazione tra i nodi e verso l'esterno**: i nodi intermedi usano solo MPLS, mentre quelli al confine della rete 'parlano' sia IP che MPLS.

Riassumendo ci sono 3 elementi chiave in MPLS:
- **Header MPLS**, che contiene l'etichetta
- **Protocolli di distribuzione delle etichette** (non vanno bene gli algoritmi di routing, perchè funzionano solo con gli Ip di destinazione)
- **Protocolli di routing migliorati** 

### Storia di MPLS

- Inizialmente CISCO propose un approccio simile basato su **tag switching**.

- Poi si passò all'idea di avere **IP sopra un'infrastruttura ATM**, ma i costi sono troppo elevati (nessun problema di risoluzione indirizzi, signaling semplificato e un solo piano di controllo).

- **MP$\lambda$S** è una versione di MPLS che funziona con gli switch ottici.

- Infine abbiamo **G-MPLS** (generalized) che introduce tutte le funzionalità dell'MPLS odierno.

# Header MPLS

L’**header MPLS** è di livello 2 ed è composta da più moduli uniti che per tale motivo viene chiamato anche **shim** header. Ogni modulo è composto da:
- **label**: 20 bit, l’etichetta da trasmettere. La lunghezza consente il passaggio fino a un milione di flussi sullo stesso link.
- **exp**: experimental bits, 3 bit. Utilizzati per il Class Of Service, ovvero per dividere in classi di servizio.
- **S**: bottom of stack, 1 bit, è posto a 1 se è l’ultimo modulo dello stack, 0 altrimenti. I moduli vengono gestiti come uno stack e l’aggiunta / rimozione avviene sempre dalla testa. Questo suggerisce che un pacchetto può avere più header
• **TTL**: time to live, 8 bit.

![[Pasted image 20231221114146.png]]

# Instradamento dei pacchetti

Una **FEC**, Forwarding Equivalence Class, è un insieme di pacchetti che hanno lo stesso destinatario, dunque un LSP è un percorso di comunicazione che viene utilizzato per trasportare un FEC: **pacchetti appartenenti allo stesso FEC ricevono la stessa label**.

Quando creiamo un LSP sono necessarie 3 azioni:
- **label binding**: Ogni LSR decide l'abbinamento fra FEC ed etichetta
- **label mapping**: si crea la riga nella tabella di forwarding
- **label distribution**: comunicazione dell'etichetta ai vicini

### Binding

La modalità più semplice è il **downstream**: tra due nodi che sono collegati, il binding è eseguito da quello a valle, dopodichè il nodo a monte viene avvisato della scelta. Ci sono due modi per notificare il nodo a monte:
- **unsolicited**: senza richiesta diretta
- **on-demand**: con richiesta esplicita

### Mapping

Occorre fare associazioni fra input label, output label e next hop: 
- L'output label è decisa dal downstream
- L'input label è scelta dal nodo corrente
- Il next hop è scelto sulla base dei protocolli di router (ancora presenti)

### Distribution

Quando un router ha effettuato il binding di una etichetta, deve comunicare tale etichetta ai **nodi vicini**, in modo che questi possano fare il mapping (almeno al nodo di upstream). Tale operazione è detta label distribution e serve a notificare l’etichetta scelta per una data FEC, in seguito al label
binding.

```ad-important
La Label **deve essere univoca sul link** (meglio che IP, infatti è di soli 20 bit).

```


### Label binding (e mapping) statico

Il label binding statico avviene attraverso un gestore di rete (in modo equivalente al PVC in ATM) che stabilisce e impone l’etichetta ai nodi della rete.
Non è scalabile e non è c’è interoperabilità tra i sistemi di controllo. Inoltre, è impossibile avere un LSP che attraversa più operatori.

### Label binding dinamico

Innescato in due modi:
- **data/traffic driver**: innescato dall'arrivo di un pacchetto
- **control driven**: innescato da messaggi di controllo

Per quanto riguarda il control driver abbiamo due alternative:
- **Topology based**: il router scopre che esiste una destinazione, in base alla topologia della rete, dei percorsi e delle destinazioni dunque gli LSR (Label Switching Router) creano degli LSP (Label Switch Path) per le destinazioni.
- creazione **esplicita** degli LSP: avviene una segnalazione esplicita, inizializzata dai LER (Label Edge router). Avviene on‑demand.

### Protocolli per label distribution

La distribuzione delle etichette avviene attraverso dei protocolli, in particolare ne esistono 3 (non compatibili tra di loro):
- BGP: utilizzo di un protocollo di routing, solo topology based (quando vengono segnalate le destinazioni vengono mandate anche le etichette).
- LDP: Label Distribution Protocol, è un’evoluzione del Tag Labelling di Cisco, attualmente deprecato. Poco utilizzato perché, essendo un sistema proprietario, si aveva paura di avvantaggiare Cisco.
- RSVP: Resource reSerVation Protocol, utilizzato per l’allocazione di servizi integrati all’interno delle reti.

### Protocolli di Routing

I protocolli di routing servono per determinare il percorso che sarà compiuto da un LSP, attraverso i quali verranno realizzate le tabelle per il label mapping e verrà deciso il next hop mediante il packet
routing.

I protocolli di routing utilizzati sono in realtà quelli già esistenti:
- OSPF
- IS‑IS
- BGP‑4

Tali protocolli vengono modificati per trasportare informazioni riguardo alle scelte di routing, oltre a quelle topologiche, e porre dei vincoli come:
- capacità dei link
- utilizzo dei link
- dipendenze tra i link (utilizzato per il recupero dei guasti)

Tali vincoli verranno utilizzati per eseguire il Constraint based routing, ovvero un protocollo di routing di controllo che permette di scegliere il percorso più adatto in base ai vincoli imposti.
Abbiamo quindi dei protocolli potenziati (OSPF-TE, IS-IS-TE) che servono per il constraint based routing.

### Modalità di routing: Hop by Hop

Ogni router decide il next hop, quindi ci si basa sulla routing table (come in IP).
- Si sceglie una label per l'upstream link
- La label viene mappata all'indirizzo dell'interfaccia del prossima LSR (next hop)
- La label è annunciata dal LSR che segue

### Modalità di routing: Explicit constraint based

Nel Explicit constraint based routing un singolo switch sceglie il percorso per l’intera LSP, oltre al FEC. Il percorso potrebbe non essere ottimale, ma almeno si evita il rischio di fare percorsi circolari (il nodo deve avere le informazioni su tutta la rete). Viene poi condiviso il percorso esplicito da compiere.

La scelta del percorso avviene mediante Constraint Based Routing (mediante vincoli), ma la distribuzione delle operazioni tra nodi è impossibile in quanto non c’è un unico criterio per scegliere il
percorso e possono esserci vincoli in conflitto. Inoltre potrebbe essere difficoltoso mantenere i vincoli e le informazioni sincronizzate, in quanto variano più velocemente delle informazioni relative alla topologia.

Per tale motivo i protocolli per la distribuzione delle etichette sono modificati in modo da supportare informazioni su quale è il percorso scelto.
In particolare sono utilizzati:
- CR‑LDP: Constraint based Routing Label Distribution Protocol
- RSVP‑TE: Resource Reservation Protocol Traffic Engineering

Questi vengono utilizzati con OSPF‑TE e IS‑IS‑TE.
Le modifiche adoperate permettono:
- Traffic Engineering
- garantire la qualità del servizio (QoS)
- per‑class traffic engineering (in sinergia con DiffServ)
- recupero dai guasti rapido, meno di 50 ms

# Traffic engineering

I pacchetti, quando spediti mediante IP, vengono inviati verso le destinazioni realizzando quello che è un fenomeno a “imbuto” , comportando aggregazione che sfocia in una riduzione delle prestazioni. Una soluzione potrebbe essere comprare nuovi router, ma questi diverrebbero inutili al termine dell’intasamento.

Il **traffic engineering** si pone come soluzione per consentire la ridistribuzione del traffico non in base alla destinazione, ma in modo **omogeneo** evitando la **congestione**.

In IP ogni volta che i percorsi sono ricalcolati la routing table è modificata e questo porta instabilità.
In MPLS  l’inoltro basato sulle etichette permette di separare il piano di
controllo dal piano dati. Nel piano di controllo i router raccolgono le informazioni di routing e calcolano le routing table. I pacchetti, però, non vengono inoltrati in base al contenuto delle routing table, ma in base alle forwarding table che si sono create facendo il mapping (realizzate durante la distribuzione delle etichette nel setup del LSP). Anche se la routing table viene aggiornata, la forwarding table non cambia.

# Cos e Qos

La CoS (Class of Service) è un insieme di parametri che descrivono il servizio richiesto. Consente una priorità relativa tra FEC differenti ed è in grado di fornire un garanzia assoluta.

Supporta il modello DiffServ con un comportamento per‑hop, EF (expedite forwarding) e AF (assured forwarding), oltre al class traffic engineering (ds‑aware traffic engineering).

La QoS (Quality of Service) garantisce specificatamente:
• bandwitdh
• Delay
• burst size
I vantaggi di QoS in MPLS sono vari, tra questi vi è la possibilità di avere una rete unificata in grado di supportare tutti i tipi di servizi (messaggio di marketing).
Il supporto per QoS e i servizi real time su IP non è ancora pronto.
Molte reti multi servizio utilizzano ora un paradigma “ships in the night”, dove i protocolli ATM sono per servizi tipici di ATM ed MPLS control plan è utilizzato per i servizi IP.

# Fast Fault Recovery

In MPLS è garantito il recupero rapido dai guasti mediante link re‑routing e edge‑to‑edge re‑routing.
Una volta creato un LSP, se un link si rompe il protocollo di routing se ne accorge e ricalcola la route, ma il piano dati continua a inviare pacchetti in base alla tabella di forwarding che non cambia. Per risolvere a tale problema, si può allora creare un ulteriore LSP che funge da backup del link, fatto non quando il link si rompe ma a priori (e utilizzato al momento della rottura).
Quando due nodi si scoprono, si crea un LSP lungo la rete che permettano a questi due nodi di inviarsi pacchetti. Se il link diretto si rompe, sarà presente un LSP che permetterà di andare verso Y.
Quando il link si rompe, verrà aggiunta un’ulteriore etichetta a quella già presente. Y saprà che quando arrivano con una certa etichetta (cioè sono per Y) dovrà rimuovere l’etichetta più esterna e procedere normalmente. Tale processo è molto veloce ed è detto link re‑routing.

![[Pasted image 20231221135326.png]]

# Gerarchie e scalabilità

Utilizzare più etichette aiuta a generare una gerarchia tra reti MPLS diverse che contribuisce alla scalabilità della rete. Utile soprattutto per instradare pacchetti in punti geografici molto lontani, quando un ISP non ha connettività diretta verso tale destinazione. Ciò comporta una riduzione delle routing table e delle forwarding table.

![[Pasted image 20231221135421.png]]

# Penultimate Hop Popping (PHP)

Nel Penultimate Hop Popping (PHP), il penultimo nodo esegue il pop della label dal LSP, in modo da non doverlo fare il nodo di destinazione. Il Label Edge Router (LER) indirizza il pacchetto in base all’indirizzo IP (o la prossima label nello stack).
La distribuzione di label 3 indica un implicito PHP, in quanto l’ edge router vede che il next hop è all’esterno.

Per qualsiasi router sull’ultimo hop avviene lo swap sull’etichetta 0.
Il PHP si divide in:
- PHP implicito: l’etichetta più esterna viene rimossa
- PHP esplicito: l’etichetta non viene rimossa, ma ci si scrive “0” all’interno
