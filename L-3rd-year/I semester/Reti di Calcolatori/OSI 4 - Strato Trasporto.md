# Servizi dello strato trasporto
---
Lo strato 4 provvede a fornire la **comunicazione tramite canale logico fra diversi processi app su hosts differenti**.
E' importante notare che questi protocolli agiscono solo sugli enti finali della comunicazione (end-end).
In particolare i protocolli in questione creano **segmenti** in trasmissione che vengono mandati al livello 3, mentre in ricezioni i segmenti vengono riassemblati e mandati allo strato app.

Le funzioni che esamineremo saranno: multiplexing e demultiplexing, controllo di congestione, affidabiilità della trasmissione.

I principali protocolli in gioco hanno caratteristihce differenti:

| TCP                          | UDP           |
| ---------------------------- | ------------- |
| controllo di congestione     | non presente  |
| controllo di flusso          | non presente  |
| setup di connessione         | non presente  |
| garantita consegna in ordine | non garantita |
| affidabile                   | non affidabile              |


# Multiplexing/demultiplexing
---
Il **multiplexing** consiste nel gestire diversi segmenti provenienti da diverse app per combinarli e aggiungere l'header.
Il **demultiplexing** è il processo opposto.
Al fine si eseguire il demultiplexing ci si basa sull'intestazione del segmento TCP/UDP e ne si controlla in particolare la porta di destinazione, la porta sorgente e l'indirizzo IP (allo strato prima) per indirizzare correttamente il segmento verso l'applicazione giusta.

Nel caso UDP quando si crea un pacchetto occorre specificare IP destinazione e porta destinazione (ovvero il **Socket**: collegamento end to end fra due processi in esecuzione sulla rete).
In ricezione invece si controlla la porta destinazione e si manda all' applicazione.

Nel caso TCP bisogna controllare tutti i parametri (IP source, dest  PORT source, dest) necessari a garantire tutte le sicurezze che il protocollo offre.

## Porte

Come fa un client a sapere qual è la porta destinazione corretta da contattare?
Esiste una categoria di porte dette **well-known port** le quali sono sempre associate a ben precisi servizi.
Ad esempio il web, basato su HTTP è associato alla porta TCP 80.
Le porte nel range 0-1024 sono riservate per UDP/TCP.
I server di posta elettronica SMTP è associato a porta 25.

Mentre la porta sorgente, essa è scelta in **maniera casuale**.


# UDP: User Datagram Protocol
---
E' un servizio che rispetto a IP non fa granchè di più, infatti le sue uniche features sono l'aggiunta dell'**intestazione UDP** e il **checksum** (che controlla la correttezza dell'effettivo payload).

![[Pasted image 20221123111830.png]]

UDP si usa sostanzialmente quando non necessito di garanzie o quando fornirle provoca un peggioramento delle prestazioni (in particolare nello streaming multimediale).

UDP è **connectionless**, dunque non c'è handshaking, e tutti i pacchetti sono considerati indipendenti

UDP è usato per DHCP, RTP...


# TCP: Transmission Control Protocol
---
I principi della trasmissione sicura si basano sull'utlizzo di [[Protocolli a finestra]], come Go Back N e Selective Repeat.

Il protocollo TCP è ideato per comunicazioni **point-to-point** (motivo per cui DHCP è UDP, perchè uso il broadcast) in maniera **affidabile**, con protocollo a finestra per controllo di flusso (il tx non intaserà il rx) e congestione.
E' un protocollo full-duplex, parametro importante **MSS maximum segment size**
E' **connection oriented** quindi c'è handshaking prima di iniziare la connessione.

![[Pasted image 20221123114934.png]]

- ACK number e sequence number servono per implementare il protocollo a finestra
- Flag U sta per urgent
- Flag A dice se l'ack inviata ha veramente senso 
- Flag RST serve per terminare la connessione in maniera non brusca
- Flag SYN serve per iniziare la connessione
- Flag FIN serve per terminare la connessione
- Receive windows serve per il controllo di flusso, corrisponde al valore della finestra del ricevitore

Il Sequence number è la posizione del primo byte nel segmento dati.
Gli ACK sono i sequence number dei pacchetti che mi aspetto di ricevere dopo (usa ACK cumulativi)

![[Pasted image 20221123120214.png]]

I triggering per la ritrasmissione sono ACK multipli e eventi di timeouts.

### Trasmettitore TCP

Vengono ricevuti dati dall'app, crea una segmento con un numero di sequenza, e starto un timer se non ce n'è già uno in esecuzione (quello del primo segmento di cui non si è ricevuto ack).
Il valore del timer è fissato al **timeoutinterval**.
Se finisce il timeout si ritrasmette **solo il segmento che ha causato il timeout**, dopodichè si ristarta il timer.
Quando ricevo un ack aggiorno le informazioni su ciò che so e se ci sono pacchetti ancora non confermati devo far ripartire il timer.

```ad-note
Nel protocollo TCP deve sempre esserci un timer in esecuzione, dunque ogni possibile volta che il timer si interrompe se ne fa subito partire un altro.
```

Dato che il timeout è relativamente lungo è possibile usufruire di una modalità chiamata **fast retransmitting** che si basa sulla ricezione di ACK uguali multipli.
In particolare se un tx riceve 3 volte un ACK per un dato pacchetto allora esso lo ritrasmette.

Come settare un corretto Timeout?
Dev'essere in relazione al RTT ma non deve essere nè troppo corto nè troppo lungo, dunque
per stimare il timeout prendiamo diverse misurazioni di un sample RTT e  ci aggiungiamo un "margine di sicurezza" per fare un modo di avere un range più elevato
$$
Timeout= EstimatedRTT + SafetyMargin
$$
### Controllo di flusso

Dobbiamo fare in modo che il ricevitore dia controlli al trasmettitore in modo da non inviare troppi pacchetti o di inviarli troppo velocemente.

Per eseguire ciò il ricevitore ha due parametri:
- **rcvrBuffer** (tipicamente settato a 4096 bytes) che indica lo spazio totale che è in grado di gestire
- **rwnd** indica lo spazio libero che il trasmettitore può inviare
In questo modo il trasmettitore invia una quantità di dati (non confermati ,unacked) pari al massimo a **rwnd** e questo garantisce che il buffer del ricevitore non vada in **overflow**.

![[Pasted image 20221123154449.png]]

### Apertura e chiusura di una connessione

E' presente una fase di **handshake** precedente alla comunicazione fra un trasmettitore e un ricevitore.

![[Pasted image 20221128184036.png]]

Per l'avvio di una comunicazione il tx manda un pacchetto con un numero di sequenza iniziale (parte da un numero random) e il SYN bit impostato a 1.
Il ricevitore risponde con un ACK con ACKbit settato a 1, SYNbit a 1 e il numero di ACK è quello della sequenza ricevuta più uno.
Il trasmettitore a questo punto manda ACK con ACKbit a 1 al ricevitore.

Questa apertura con 3 messaggi è detta **3-way handshaking** (oppure SYN/SYNACK/ACK) e durante questo scambio iniziale si comunica anche MSS e Rwnd.

Per chiudere la comunicazione si fa uso del **FINbit**

![[Pasted image 20221128184333.png]]

Notiamo che in questo caso entrambi mandano pacchetti con FIN settato in quanto la comunicazione in TCP è full-duplex.

# Gestione della congestione in TCP
---
Quando ci sono troppe stazioni che trasmettono troppo e troppo velocemente allora è possibile che si verifichino **ritardo** o i buffer possono andare in overflow provocando lo **scartaggio dei pacchetti**.

La **network assisted congestion control** prevede che i router mandano dei feedback sul loro stato, o dettino un rateo a cui mandare i messaggi per il trasmettitore.

Molto più utilizzato ( e perchè vogliamo la complessità all'edge ) è la **end-end congestion control** cercando di modellare l'intera rete con un buffer:
- se il buffer di rete è vuoto allora il tx può accelerare la trasmissione
- se il buffer di rete è pieno allora tx dovrà rallentare
Abbiamo quindi introdotto la **cwnd ovvero congestion windows**
(E' lo stesso approccio del controllo di flusso)

Gli algoritmi con cui si implementa end end congestion sono:

### Additive Increase Multiplicative Decrease

Di categoria **congestion-avoidance** l'idea di fondo è quello di far crescere la cwnd poco alla volta in maniera lineare fino a quando non si ha un perdita:
- Ad ogni ACK rivevuto aumento MSS di 1/cwnd se non rilevo perdite (lineare)
- Dimezzo di netto la cwnd se non ricevo un ACK

![[Pasted image 20221128185724.png]]

**TCP Slow Start** è n'alternativa e parte con un cwnd uguale a 1 e poi aumenta di 1 a ogni ACK ricevuto (crescita esponenziale) fino a una soglia, poi diventa AIMD.

**Se scade il timeout**: la congestion windows è messa a 1 MSS, poi come slow start.
**TCP RENO** se riceve 3 ack duplicati taglia la congestion windows di metà, poi lineare.
in **TCP Tahoe sempre messo a 1 MSS** (per timeout o ack dup.)

### TCP Fair

Nel caso di multiple connessione TCP l'algoritmo TCP è equo, e le multiple connessioni condividono la banda in maniera **equa**.

![[Pasted image 20221128201515.png]]



