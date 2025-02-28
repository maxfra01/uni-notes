# Processo di numerizzazione
---
Nelle reti telematiche l'informazione si trasmette in forma **digitale** usando segnali digitali e analogico.
Se queste informazioni sono in forma analogica, occorre, prima di essere trasmessi un **processo di numerizzazione**. 
Ovvero tutto ciò visto nei corsi di SETM e TES, vedi appunti [[Conversione Analogico-Digitale]].

# Tipi di trasmissione
---
Due grosse categorie: parallela e seriale.
- Parallela: l'informazione è trasferita in parallelo su bus di comunicazione con segnali di dati e clock.
- Serializzata: l'informazione è appunto serializzata e trasmessa un bit alla volta.

In entrambi i casi la problematica maggiore è quella della **sincronizzazione**.
Disitnguiamo le trasmissioni:
- Asincrona, ogni byte è trasmesso separatamente dagli altri; ad ogni ricezione occorre recuperare il sincronismo
- Sincrona, le informazioni sono strutturate in **trame** (o slot) e prima di iniziare la trasmissione il tx e l'rx sincronizzano il loro clock, mantenendolo sincronizzato per tutta la durata.

# Modi di trasferimento (Multiplazione)
---
Come condividere le risorse (nodi e canali) per trasferire i dati?
La condivisione di canale avviene tramite **multiplazione** e **accesso multiplo**.
La condivisione dei nodi avviene tramite **commutazione**.
Si parla di multiplazioni quando più flussi sono disponibili al canale in un unico punto.
Diversi modi di realizzarla:

### Multiplazione di Frequenza

Separazione ottenuta usando bande di frequenze diverse, intervallate da bande di guardia (è il caso della radio FM). 
Le bande di guardia servono per prevenire l'effetto doppler. 
L'ampiezza della singola banda è proporzionale alla velocità di trasmissione.

![[Pasted image 20221003140606.png]]

### Multiplazione di Tempo

La separazione avviene mediante intervalli di tempi differenti, in cui la sorgente può usare tutte le frequenze che vuole ma deve rispettare il suo turno.
Anche in questo caso servono dei 'tempi di guardia'.

![[Pasted image 20221003140806.png]]

### Multiplazione di codice

La comunicazione può avvenire in presenza di più sorgenti, a patto che i le codifiche delle informazioni rappresentino codici ortogonali fra di loro, in modo da non disturbarsi.
I codici validi sono finiti e limitati, perciò la tecnica non è ottima.

![[Pasted image 20221003141506.png]]

### Multiplazione di spazio

Le reti permettono di sfruttare la diversità spaziale del sistema per far coesistere più flussi di informazione in punti diversi. 
Il territorio viene diviso in celle (cioè l’area intorno ad una antenna) e quando un ricevitore si sposta, non sente più l’antenna da cui partiva ma si aggancia ad un’altra. 
In questo modo viene utilizzata la stessa frequenza ma in più punti diversi senza collisioni.

### Multiplazione statistica

La multiplazione statistica è una modalità con cui si possono utilizzare tutte le precedenti multiplazioni. Infatti la multiplazione può essere predeterminata (cioè statica, fissa) oppure statistica.
In quest’ultimo caso le bande vengono allocate ma non utilizzate. 

Nominalmente una frequenza appartiene a un certo ente X, ma quest’ultimo non trasmette. In questo modo è possibile far utilizzare la frequenza ad un altro ente Y. In questo modo si ha una sola banda ma due sorgenti, scommettendo tutto sul fatto che la probabilità che X e Y trasmettano contemporaneamente sia nulla.

# Modi di trasferimento (commutazione)

### Commutazione di circuito
In questa modalità il circuito è di **uso esclusivo** dei due utenti in contatto per tutta la durata nella comunicazione.
Le risorse vengono rilasciate al termine della comunicazione.

![[Pasted image 20221003142619.png]]

- Impegno: prima fase della comunicazione, ovvero alzare la cornetta e digitare il numero, se l'utente 2 risponde allora c'è l'invio di una comunicazione nella direzione opposta: la comunicazione può iniziare e il circuito è riservato.
- trasferimento dati

Efficiente per sorgenti non intermittenti, il tempo di apertura del circuito è elevato, tariffazione in base al tempo di esistenza del circuito.

### Commutazione di pacchetto

Le informazioni sono organizzate in **Protocol Data Unit PDU** che contengono a loro volta una **PCI, protocol contron information** e una **SDU service data unit**.

```ad-note
Il termine 'pacchetto' è generico e cambia a seconda dello strato a cui si fa riferimento, ad esempio posso riferirmi ad esso con il nome di packet, cll, segment, frame, trama, datagram, message ecc...
```

La commutazione di pacchetto si basa sullo **Store and Forward** per cui ogni nodo che riceve un pacchetto:
1. lo memorizza
2. lo elabora
3. lo trasmette

I **Ritardi sul canale** che possono  verificarsi sono dovuti in trasmissione alla dimensione dei bit del pacchetto oppure alla propagazione (metri di distanza).
Per ogni **Ritardo sul nodo** è possibile che sia dovuto o a un ritardo di elaborazione o all'accodamento.

Pacchettizzazione in funzione di molte cose, pacchetti corti abbassano la probabilità di errore.

La commutazione di pacchetto può essere realizzata con due approcci

| Circuito virtuale                                                                             | Datagram mode                                                                                        |
| --------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| 3 fasi, apertura, trasferimento e chiusura, per cui avrò un accordo preliminare tra le entità | Non c'è un accordo preliminare, i pacchetti vengono inviati in rete e possono seguire diverse strade |
| Le risorse comunque non solo allocate staticamente                                            | no allocazione                                                                                       |
| Occorre un sistema di etichette per organizzare i flussi di dati                                                                                              |                                                                                                      |

Si noti che la commutazione di pacchetto a circuito virtuale richiede l'uso di etichette anche solo locali che permettono al nodo di instradare il pacchetto sull'interfaaccia corretta.


# Tecniche di segnalazione
---
Una **segnalazione** è uno scambio di informazione che riguardano l'apertura e la chiusura, nonchè la gestione di una rete di trasmissione.

Possiamo distinguere due strategie:
- **associata al canale**: c'è una corrispondenza biunivoca tra il canale controllante e quello controllato. Inoltre è possibile che i due canali coincidano (**in banda**). E' la modalità della telefonia mobile ![[Pasted image 20230117100550.png]]

- **a canale comune**: in questa segnalazione esiste un canale appunto comune che gestisce i flussi di controllo di più utenti.  E' la modalità in uso dalla commutazione di pacchetto. ![[Pasted image 20230117100852.png]]

