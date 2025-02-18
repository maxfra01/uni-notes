# Introduzione

Il **Routing** è il processo con cui si **determina il percorso** da una sorgente a una destinazione per un pacchetto trasmesso su una rete.
Questa operazione avviene nel control plane.
Con il termine **forwarding** (data plane) si intende il processo per cui un pacchetto viene inoltrato al next hop, sulla base delle decisioni prese nelle fase di routing.

### Forwarding

- L'approccio più comune è quello basato sul Forwarding by network address (IP [[Ipv4]] ).
- L'approccio di **label swapping** è basato sul protocollo MPLS (trattato più avanti).
- L'approccio di **source routing** permette all'host di mandare un pacchetto specificando che percorso vuole fare, ma questo richiede che il mittente conosca la topologia della rete.

I costruttori dei dispositivi possono ottimizzare il forwarding: al posto che usare la classica routing table (Routing Information Base, RIB) posso usare una tabella personalizzata per rendere l'operazione più efficiente (**Forwarding information base, FIB**).

Possiamo quindi dividere il forwarding in 3 fasi:
- Scelta del next-hop (tramite FIB o RIB)
- Switching alla posta selezionata
- Trasmissione

### Routing Algorithm classification

Due principali famiglie di algoritmi:
- **Non adaptive algorithms (statico)**
- **Adaptive algorithms (dynamic)**

# Non-Adaptive routing algorithms

In questa famiglia di algoritmi rientrano gli approcci equivalenti alla scrittura statica di una routing table.
- La **configurazione manuale** è la prima opzione
- Il **Flooding** è un approccio molto semplice in cui si inoltra il pacchetto su tutte le porte, tranne quella sorgente
- Posso scegliere di inoltrare su una porta **random**
- **"Hot potato"** consiste nell'inoltrare sulla porta meno carica, per liberarmi del pacchetto il prima possibile

![[Pasted image 20231122163348.png]]

Nell'immagine sopra abbiamo un approccio di configurazione manuale.
Questi approcci sono comodi per avere pieno controllo sulla topologia di rete, al costo di non essere resistenti agli errori e di non adattarsi ai cambi di topologia.

# Adaptive routing algorithms

### Centralized routing

Il primo algoritmo **Centralized routing** consiste nell'uso di un nodo centralizzato che si occupa di calcolare le routing table e di distribuirle fra i nodi che appartengono alla rete. Il nodo viene chiamato **Routing Control Center, RCC**.

![[Pasted image 20231122164512.png]]

Dato che RCC conosce sempre la topologia, il centralized routing è efficiente per trovare percorsi alternativi nel caso di un link guasto. Purtroppo però RCC costituisce un single point of failure, un collo di bottiglia e non si adatta a network di grandi dimensioni.

### Isolated routing

L'approccio è opposto al routing centralizzato, infatti **ogni nodo decide in modo indipendente** dove mandare i pacchetti: non c'è un RCC e nemmeno scambio di informazioni nella rete.
Ci si basa sul backward learning, come gli switch.

### Distributed routing

Combinando il meglio dai due approcci precedente si ottiene l'approccio distribuito: ogni **router sceglie in modo indipendente ma coerente**, grazie a una condivisione di informazioni fra i router.
Distinguiamo due categorie:
- **Distance Vector**
- **Link State**

# Funzionamento di un algoritmo di routing

1. Ogni router **genera informazioni** riguardo che reti locali che può raggiungere
2. Ogni router **riceve informazioni** riguardo a reti locali che gli altri router possono raggiungere
3. Si **propagano** queste informazioni sulla rete

Gli algoritmi devono funzionare in modo automatico, devono garantire consistenza della rete, eliminando loop, valutando diverse metriche.
In particolare le **metriche** ci informano sul costo di un certo percorso: ovviamente si preferiscono i percorsi con costo minore.
Possiamo scegliere come metriche diversi parametri:
- Numero di hops
- Minor latenza
- Più criteri combinati tramite somme pesate
- Altri più specifici
E' importante scegliere le metriche con cura, in quanto più criteri corrispondono a una maggiore complessità, che possono sovraccaricare il dispositivo.

# Distance Vector

Ogni nodo **informa periodicamente** gli altri nodi riguardo le destinazioni che lui può raggiungere, fornendo anche il costo per ogni destinazione.
Ogni nodo quindi mantiene due strutture dati:
- **Distance Table**: Per ogni destinazione (raggiungibile tramite i vicini) si ha il costo necessario per raggiungerla, associato al next hope. La Distance table DT va mantenuta per ogni vicino che un router ha.
- **Routing table**: Accorpando le varie DT si riesce a scegliere il percorso migliore e costruire/aggiornare la routing table

Quindi quando un nodo riceve un nuovo vettore si va ad aggiungere la destinazione, si modificano i percorsi se sono più corti e si modificano i costi se necessario.

Dato un nodo sorgente X, destinazione Y, e nodo intermedio adiacente Z, definiamo il costo per andare da X a Y, passando per Z come:
$$
D^X(Y,Z) = c(X,Z) + \min_{w} \{D^Z(Y,w)\}
$$
![[Pasted image 20231122180911.png]]

Scegliendo per ogni destinazione il percorso più corto si costruisce la **Routing table**.
Complessità del Distance vector: $L$ numero di Link, $N$ numero di router.
$$
O(N \cdot L)
$$
### Problemi del distance vector

Problemi legati a questo algoritmo:
- **Black hole**: Se un router non sa più come arrivare a una destinazione, allora diventa un "buco nero" per i pacchetti verso quella destinazione, andando perciò a scartare molti pacchetti. E' una situazione che si verifica nel transitorio dell'algoritmo, quando non è stata completata la mappatura dei nodi.

- **Count to infinity**: A causa della lenta convergenza dell'algoritmo è possibile non avere consistenza nelle rotte. In particolare in presenza di guasti non tutti i nodi sono informati di quest'ultimo, quindi si continua a incrementare la distanza dal nodo "guasto"
  ![[Pasted image 20231122184034.png]]

- **Bouncing effect**: Un problema derivato dal problema di un guasto a un link è proprio il bouncing effect, dove due nodi hanno routing table inconsistenti e si scambiano lo stesso pacchetto all'infinito
  ![[Pasted image 20231122184212.png]]

### Soluzioni parziali

Soluzioni **parziali** a questi problemi:

- **Split horizon**: "se C raggiunge A attraverso B, allora è inutile che comunichi a B che può raggiungere A tramite C". Per implementare questa scelta bisogna creare dei DV personalizzati per ogni vicino.
  Così facendo però andiamo a rimuovere i loop fra due nodi e si velocizza la convergenza.
  ![[Pasted image 20231122184857.png]]

- **Part Hold Down**: "Se un link L si guasta, allora tutti i nodi raggiungibili tramite L sono considerati non raggiungibili per un certo periodo di tempo"
  ![[Pasted image 20231122184926.png]]

- **Split horizon with Poisonous Reverse**: al posto che non dichiarare un link guasto, lo si dichiara a distanza infinita. Soluzione che porta a una convergenza più veloce
  ![[Pasted image 20231122185414.png]]

### Algoritmo di Path Vector

L’algoritmo Path Vector elimina i cicli inviando, in aggiunta alle informazioni sulla distanza, le informazioni sui nodi attraversati per raggiungere una determinata destinazione. In questo modo si evitano i loop all’interno dei transitori.

# Link state

A differenza del distance vector, con questa tecnica vengono inviate informazioni riguardo **tutta la rete** e di tutti i nodi.
Ogni router manda in broadcast informazioni sui suoi link: così facendo ogni router sarà in grado di realizzare una mappa locale della rete, inviando informazioni tramite **selective floating**.
Sarà compito dei router costruire i cammini minimi per ogni altro nodo nella rete, tramite l'algoritmo di **Dijkstra**.
Teoricamente le informazioni link state vengono condivise quando si modifica la topologia: nella pratica si mandano link state periodicamente per aumentare l'affidabilità.

```ad-note
title: Note sul link state
- Genera loop **raramente**, in caso di guasti o grandi scenari
- Per reti piccoli non è conveniente usare questo metodo
- La convergenza è rapida

```

**Dijkstra (Shortest path first)** ha una complessità logaritmica, e questo lo rende più scalabile per grandi reti:
$$
O(L \cdot \log N)
$$
Per i motivi elencati sopra è molto efficiente, tuttavia la prima implementazione è lunga, e la complessità del sistema è alta.

# Internet Routing Architecture

Il **dominio di routing** è un insieme di router che utilizzano lo stesso protocollo di routing, che sono connessi a una porzione della rete. Un router potrebbe far parte di più routing domains (utilizzando più protocolli di routing) e può **ridistribuire** le informazioni imparate con un protocollo mediante uno differente. Questo processo è possibile attraverso una conversione delle metriche, utilizzo di filtri di advertisement e information source priority tramite una configurazione dell’amministratore.

### Autonomous system

Un Autonomous System (AS) è un **gruppo di router e sottoreti raggruppate** in base alla topologia o un criterio organizzativo (ad esempio una subnet di un grande ISP) sotto il controllo di una singola e ben definita autorità.
L’indirizzamento e l’instradamento sono strettamente coordinati e l’interfaccia AS è controllata (data, informazioni di routing). Dal punto di vista amministrativo è possibile indicare delle scelte di routing interno autonome e negoziare scelte di routing esterno. E’ scalabile, in quanto nessuna delle informa zioni è propagata “ovunque” ma è il singolo AS a decidere dove far passare i propri dati.

E’ identificato da **due byte numerici** assegnati dalla IANA (Internet Assigned Numbers Authority). Il range di numeri privati va da 64512 a 65534, lo scambio di informazioni di routing è controllato. All’interno di un AS sono presenti dei dispositivi detti **border gateway**, posizionati al confine tra differenti AS. Questi dovranno comunicare tra loro e con i dispositivi interni alla rete, per tale motivo distinguiamo i protocolli in **iBGP** (intra Border Gateway Protocol) per la comunicazione all’interno della rete e **eBGP** (Exterior Border Gateway Protocol) per le comunicazioni tra gli AS.

![[Pasted image 20231125180838.png]]

### Protocolli IGP

Gli algoritmi di tipo **Interior Gateway Protocol** si distinguono in:
- Distance Vector: comprende RIP (Routing Information Protocol) e IGRP (Interior Gateway Routing Protocol).
- Link State: comprende **OSPF** e Integrated IS‑IS.

### Open Shortest Path First

OSPF fa parte degli algoritmi di link state, utilizza un routing di tipo gerarchico. Il routing domain è diviso in aree, in ciascuna delle quali avviene una aggregazione delle informazioni. I router sanno tutti i dettagli delle zone/domain/area, ma non sanno nulla o hanno informazioni limitate relative all’esterno. Può essere iterato. 

Nello strictly hierarchical routing i router non hanno informazioni sull’esterno della rete, motivo per cui quando il destinatario del pacchetto non è nella stessa area viene eseguito il forwarding verso un edge router, ovvero un router di confine con l’esterno della rete. Il routing è limitato in termini di efficacia, ma è altamente scalabile. I percorsi sono sub‑ottimali, ma si ha perdita di connettività in caso di errori.

Nel loosely hierarchical routing si ha minore scalabilità in quanto i router devono mantenere e scambiare più informazioni, ma non è richiede strictly hierarchical addressing. Tutti gli host nel dominio B non hanno bisogno di un identificatore comune, bensì vengono utilizzati dei prefissi. E’ possibile
implementarlo in IPv4.

Ogni area avrà una visione completa della propria topologia interna, ma verso l’esterno soltanto i collegamenti per parlare con le altre aree, vedendone una visione aggregata conoscendone i router di frontiera.
Per 𝑁 router si hanno 𝑁 2 adiacenze (dunque link). La complessità di Dijkstra è lineare nel numero di link.

### Protocollo BGP

Nasce dall'esigenza di fornire connettività fra le reti, insieme a delle decisioni di policy da parte delle aziende.
Si chiama **Border Gateway Protocol**, ed è lo standard di fatto per il routing fra domini diversi.
Per ogni AS esso permette di:
- Ottenere informazioni su AS vicini (**eBGP**)
- Condividere le informazioni con i sistemi appartenenti all'AS (**iBGP**)

Per il routing non si sceglierà necessariamente il percorso più corto, ma quello concordato fra due parti.

![[Pasted image 20231129165449.png]]

Il funzionamento base del protocollo è il seguente: i router BGP, detti **peers**, appartenenti a diversi AS si scambiano messaggi tramite **TCP**: si annunciano dei percorsi tramite **Path Vectors** (si indicano quindi **costo**, **next hop** e lista di **AS attraversati**).

```ad-note
title: Next Hop in BGP
Il next hop è fondamentale per implementare il policy routing: condividendo il next hop io forzo un router a scegliere la strada che gli ho imposto.

```

Comandi utili del BGP sono: UPDATE (per aggiornare i percorsi) e il KEEPALIVE(tiene la connessione attiva anche se non si fanno update).

Tramite questo protocollo un certo router può venire a conoscenza di **più di un percorso** per una certa destinazione: si sceglie in base a una serie di fattori come policy, costo, next hop più vicino...

I Tipi del connessione del BGP sono due:
- **Transit**, normale endpoint per tutto internet
- **Peering**; due AS si scambiano traffico che viene generato e distrutto all'interno di una rete.

In particolare per il peering possono esserci degli Internet Exchange Point **IXP**, dove le connessioni di più AS convergono.