# Lo strato 2
---
Storicamento lo strato due ha due principali applicazioni:
- Nelle reti di accesso tra l'apparato che abbiamo in casa (router) e l'apparato in centrale, lo strato 2 realizza collegamenti punto punto
- Nelle reti private per ad esempio indirizzamento locale e protocollo di accesso multiplo

### Formato PDU comune (pubbliche e private)

![[Pasted image 20230112145827.png]]

All'inizio e alla fine c'è il **flag di delimitazione**, (tutto orientato al byte).
**Indirizzo** per reti pubbliche è ignorato, nelle private serve per indirizzamento.
**Controllo** serve a differenziare il protocollo.
I dati (ovvero una 2-SDU) sono 3-PDU.

Occorre proteggere la trama da bit uguali al flag (**trasparenza**):
- **bit stuffing**
- **byte stuffing**: ogni volta che incontriamo una sequenza uguale al flag (o uguale all'escape), il ricevitore mette un byte di escape. Il ricevitore toglierà il primo escape di ogni coppia.

# Reti pubbliche
---

### PPP

E' usato ad esempio per effettuare collegamenti su linea telefonica o adsl per host di utenza residenziale (anche SONET).
Il compito principale è la **delimitazione delle trame, byte stuffing, riconosce errori (CRC), multiplazione ecc...**.

**NON FA CORREZIONE ERRORI, MANTENIMENTO SEQUENZA, CONTROLLO DI FLUSSO** (è punto-punto, le trame non possono andare da nessuna parte)

![[Pasted image 20230112150722.png]]

control si ignora ma serve per anticipare il protocollo di strato superiore.

Il protocollo PPP si occupa anche di instaurare e abbattare la connessione gestendone i parametri  ![[Pasted image 20230112150906.png]]
NCP negozia anche l'indirizzo IP

### ATM

Ha delle PDU di dimensioni fissa (chiamate CELLE di 53 byte)
Usate tra il DSLAM e la centrale dell'operatore

![[Pasted image 20230112151319.png]]

ATM fa frammentazione da se, senza far fare nulla allo strato 3.

# Reti private/locali/LAN
---
All'interno di questo modello, lo strato OSI del collegamento è divisio in due sottolivelli:
- **LLC Logical Link Control**
- **MAC Medium Access Control**

Il Logical Link Controlo LLC è anche il nome del protocollo che regola l'omonimo strato. Serve per fare **Multiplazione di protocolli**
E' anche detto standard IEEE 802.2 ed ha le seguenti caratteristiche:
- orientato al byte
- non si controllano gli errori
- non c'è delimatazione

Il suo compito è quello di fornire informazioni allo strato superiore, tramite appositi campi (si noti la dimensione variabile per il campo informazioni).

![[Pasted image 20221028160115.png]]

Ma a questa struttura dsi cerca di aggiungere un campo supplementare necessario per lo strato superiore (3).

![[Pasted image 20221028160431.png]]

Si aggiunge perciò quella che viene denominata **SNAP PDU**.


# Caratteristiche di una LAN
---
Local Area Network hanno una piccola estensione geografica,e hanno la principale caratteristica di avere un **traffico impulsivo**, ovvero che non trasmettono sempre ma quando lo fanno desideriamo alta velocità.

Utilizza un mezzo radio condiviso, dunque trasmette un solo nodo alla volta, altrimenti spreco banda.

Topologie utilizzate: bus (vecchia), stella (oggi)

Il problema che sorge da ciò è quello dell'accesso multiplo al canale.
- Multiplazione: problema concentrato, tutti i flussi convergono in un punto (tipo switch)
- Accesso multiplo: flussi in punti diversi della rete
Quindi come affrontare il problema?

Esistono 3 principali famiglie di protocolli per la gestione di una rete locale:
- **Accesso casuale o contesa**, come Ethernet e WiFi
- Accesso ordinato, ad esempio Token Ring e FDDI
- a slot con prenotazione, es DQDB


# Protocolli ad accesso casuale
---
Quando un nodo trasmetto lo fa **alla massima velocità** e senza coordinarsi con nessuno!
Se due o più nodi trasmettono contemporaneamte si ha una **Collisione**.
Successivamente analisi della collisione:
- come evitarla
- come riconoscerla
- gestirla, ovvero ritrasmettere

## Aloha
Primo esempio di protocollo ad accesso casuale
Ogni antenna trasmette al master e il master manda a tutte in broadcast.
La vera destinataria del pacchetto lo riceve, e l'antenna che ha trasmesso per prima, riceve il proprio pacchetto e lo usa da ACK.
Semplice, non richiede sincronizzazione, ma la probabilità di collisione è alta.
Utilizza una strategia i stop&wiat (dopo timeout scade e ritrasmette).

![[Pasted image 20221028164925.png]]

Problema: se ritrasmetto dopo un tempo fisso e si è verificata una collisione continuerà a verificarsi la stessa collisione.
Per risolvere uso la tecnica deel **BACKOFF** ovvero ritrasmettere dopo un tempo casuale, e in caso di ulteriore collisione si raddoppia il massimo tempo di attesa casuale (backoff esponenziale).


## Slotted Aloha
Versione di Aloha ma con tempo diviso in slot (egual dimensione).
I nodi trasmettono all'inizio di ogni slot (le trame hanno durata equivalente allo slot).
Se ho una collisione ritrasmetto in un altro slot con probabilità p fino al successo.

![[Pasted image 20221028165455.png]]

Tuttavia questi protocolli sono instabili e limitati per valori bassi, serve qualcosa di più efficiente.

![[Pasted image 20221028165625.png]]


## CSMA Carrier Sense Multiple Access
Variante moderna dell'aloha in cui prima di trasmettere si ascolta il canale (carrier sense) per vedere una portante. Se è dunque **occupato** ho alcuni comportamenti possibili:
- CSMA 1-persistente: ascolto sempre e aspetto che si liberi il canale, poi trasmetto subito
- CSMA non persistente: riprovo a sentire il canale dopo un tempo casuale, e vedrò se trasmettere

Nonostante ciò le collisioni sono inevitabili e sono dovute ai ritardi di propagazione. C'è dunque uno spreco di tempo per la trasmissione della trama.
- la distanza fisica è determinante per le collisioni, se due stazioni sono più lontane e trasmettono c'è più possibilità di collisioni.
- a parità di traffico, trame lunghe nel tempo diminusicono la probabilità di collisione.


Ne esitono due varianti di CSMA:
- CSMA/CD **Collisione Detection** adatti a mezzi cablati
- CSMA/CA **Collision Avoidance**, adatti a mezzi radio

## CSMA/CD
In sostanza mentre trasmetto ascolto il canale, se sento solo la propria trasmissione allora proseguo.
Altrimenti se sente altri segnali smette immediatamente.
In caso di successo nel primo caso per tutta la trasmissione allora non è nemmeno necessario un ACK purchè valga:
- la durata minima della trama sia superiore al doppio del tempo massimo di propagazione nella rete

**Dominio di collisione**: Porzione di rete in cui, se due stazioni CSMA trasmettono simultaneamente, le trame collidono.

![[Pasted image 20221028180219.png]]

Vantaggi su CSMA classico:
- Se mi accorgo della collisione e interrompo non spreco risorse e tempo
- Facile da implementare su reti cablate, confronto potenza segnale inviato e ricevuto
- Non possibili in wireless perchè canale half duplex (quando trasmetto non posso ascoltare contemporaneamente).

Prestazioni del CD migliori:
- su reti piccoli (la distanza influenza)
- se $a=\frac{t_{P}}{T_{TX}}$ è piccolo
- quando la velocità di trasmissione è bassa, almeno spreco pochi bit se mi accorgo della collisione
- qunado uso 1-persistente
- in reti ethernet

## CSMA/CA
Non potendo ascoltare mentre si trasmette si usa un approccio conservativo.
Le stazioni usano CSMA non persistente, e il ricevitore deve inviare un ACK. 

Il trasmettitore se nota il canale libero libero per un tempo DIFS, allora trasmette.
Se è occupato (o diventa occupato durante DIFS) allora rimanda la trasmissione per il tempo di backoff.
Quando il canale è libero la stazione decrementa il backoff, qunado torna a 0 la trasmissione riparte.

In ricezione si verifica la correttezza della trama, se lo è si manda l'ACK dopo un tempo SIFS.
Se dopo un countdown il trasmettitore non riceve l'ack fa partire il backoff

In caso di inizio trasmissione simultaneo si possono comunque avere collisioni.

![[Pasted image 20221028183543.png]]

Si hanno ottime prestazioni nelle reti piccole

E' il protocollo usato nella reti WiFi 802.11


# Indirizzo MAC
---
E' un identificativo che permette di individuare un determinato dispositivo (scheda di rete) tra i nodi della rete locale.
Sono lunghi tipicamente 6 byte, e ad oggi sono configurabili via software altrimente sono decisi dal costruttore della scheda.

I 3 byte più significati sono l'**Organization Unique ID**, e identificano la marca del costruttore, mentre i 3 byte meno significativi sono progressivi 

![[Pasted image 20221102121203.png]]

Gli indirizza MAC sono **Unicast, multicast o broadcasta**, a seconda se siano riferiti a una, più o tutte le stazione nella rete.

Le modalità di multicasting sono:
- Solicitation
- Advertising


# Ethernet
---
Standard di cablaggio principale per reti MAN e LAN, con le seguenti caratteristiche:
- un formato della trama ben definito
- commutazione di pacchetto a livello collegamento
- CSMA/CD

IEEE definisce a partire da ethernet lo **standard 802.3**
Esaminiamo ora i formati del pacchetto dei due standard:

| Ethernet                             | IEEE 802.3                           |
| ------------------------------------ | ------------------------------------ |
| ![[Pasted image 20221102175343.png]] | ![[Pasted image 20221102175414.png]] |

Il campo **Preambolo** di lunghezza di 7 byte permette la sincronizzazione del clock del RX e del TX.
Il **SFD Start of Frame Delimiter**, da 1 byte, determina la fine della sincronizzazione, la sequenza alternata di 1 e 0 viene interrotta da un doppio.
La prima differenza fra le 2 è la presenza del campo per il **tipo di protocollo livello superiore** in uno e la **lunghezza del campo dati**.
Il campo **Padding** serve solo a raggiungere la dimensione minima di 46 byte.
La **Frame Check Sequence FCS**, 4 byte, occorre per il controllo degli errori e infine il **Silenzio infrapacchetto** di 12 byte.

## Ethernet classico
Utilizza il protocollo CSMA/CD 1-persistente.
Se durante la trasmissione si rileva una collisione si interrompe la trasmissione e si invia una **sequenza di jamming**.
Il tempo per ritentare la trasmissione non può essere inferiore al RTT.

Il protocollo è semplice e distribuito, ha buone prestazione se la rete non è sovraccarica.

Non c'è ACK.

Relativamente economico, non gestisce priorità.

A livello fisico si usano cavi di diverse categorie e si usa la codifica di Manchester.

## Ethernet ad oggi
Ad oggi si utilizzano cavi in fibra ottica e topologie a centro stella ATTIVO.
Le comunicazioni avvengono in pratica solo in modalità full-duplex.


# Commutatori a livello collegamento
---
Per passare da una topologia a bus a uno a stella gerarchica occorre un centro stella, che si concretizza in due dispositivi:
- hub
- switch

### Hub
Apparato multiporta per l'interconnessione di LAN che opera a livello 1 [[OSI 1 - Strato Fisico]].

Il suo modello è quello del centro stella passivo, il suo compito è rigenerare su tutte le uscite la codifica di linea sulla porta in entrata.

Non fanno altro, non riconoscono le trame e non separano i domani di collisione.

![[Pasted image 20221107142516.png]]


### Switch
Apparato multiporta per l'interconnesione di LAN che opera al livello OSI 2 [[OSI 2 - Strato Collegamento]].

E' di fatto un nodo store & forward in grado di riconoscere (non modificare) le trame dei pacchetti.
E' possibile che delle trame si perdano per overflow della memoria interna.

Il modello è un centro stella attivo e instradano le trame in base al protocollo MAC, il tutto in modo trasparente all'utente e prestazioni elevate.

![[Pasted image 20221107142853.png]]

Ogni porta dello switch corrisponde a un dominio di collisione a se stante e lo switch è in grado di **eliminare le collisioni**, di fatto eliminando la necessità del CSMA/CD.

# Instradamento
---
Se connettiamo tramite switch segmenti di LAN otteniamo ancora una LAN.
Vogliamo che i comportamenti dei vari dispositivi non cambino (trasparenza) perciò la complessità dell'instradamento sarà nello switch.
- Address learning
- Frame forwarding
- spanning tree

## Address learning
Lo switch memorizza entry di una tabella contenente per ogni porta dello switch gli indirizzi MAC dei dispostivi raggiungibili.

Ogni volta che lo switch riceva la trama legge il MAC sorgente e lo memorizza nella tabella insieme alla porta.
Algoritmo di **backward learning**

## Frame Forwarding
Quando ricevo una trama cerco a quale porta è associato il MAC destinazione:
- Se è associato alla porta da cui è arrivata la trama, allora si scarta (non doveva arrivare lì)
- Altrimenti inoltro su porta corretta.
In Multicast inoltro su tutte le porte tranne quella di ingresso.