# DNS
---
**Domain Name System** è uno dei protocolli/sistemi che troviamo nell'application layer, ma di supporto al protocollo IP.

Permette di mappare un "nome" o "dominio" con un corrispondente indirizzo IP, e per fare ciò necessitiamo di un sistema di **Database distribuito** organizzato tramite **gerarchia di name server**.
La struttura centralizzata non è consigliata per tolleranza ai guasti, dimensioni eccessive, distanza fisica del server e manutenzione.
Come per altri protocolli l'obiettivo è di confinare la **complessità al bordo** della rete, lasciando al singolo utente un metodo semplice da gestire.

![[Pasted image 20221116114445.png]]

Sotto i **root DNS servers** abbiamo quelli che vengono definiti i **Top Level Domain**, tipo com, net, org, edu, mentre sotto i TLD abbiamo i server **autoritativi**.

| TDL                                                                 | Autoritativo                                         | Root                                               |
| ------------------------------------------------------------------- | ---------------------------------------------------- | -------------------------------------------------- |
| Sono i domini come com, net, org, ecc...                            | Server DNS proprietari dell'azienda                  | Sono circa una ventina di server in tutto il mondo |
| Non contengono il mapping ma i puntatori ai DNS server autoritativi | Fornisce le informazioni relative al mapping nome-IP | Corrispondono al punto di ingresso alla gerarchia dei name server                                                   |

```ad-info
Mantenere aggiornati i root server DNS è semplice in quanto le informazioni che posseggono sono molto statiche, ad esempio il server che gestisce il TDL .com non cambia mai!
```

Esiste anche il **Local DNS server**, fuori dalla gerarchia, che è responsabile di mantenere una **Cache** per usi futuri di quel mapping.
Ne esiste uno integrato nei router oppure gestiti da un ISP.

## Risoluzione DNS

| Iterativa | Ricorsiva |
| --------- | --------- |
|    ![[Pasted image 20221116120914.png]]       | ![[Pasted image 20221116121159.png]]          |

 
I record di un local DNS server hanno un formato detto **RR, Resource Record**:

| name | value | type | TTL | 
| ---- | ----- | ---- | --- |

Il campo type specifica la relazione fra `name` e `value`

| Type  | Significato                                                                         |
| ----- | ----------------------------------------------------------------------------------- |
| A     | Il name è l'hostname, mentre il value è un IP address                               |
| NS    | Name è il dominio, mentre value è l'hostname del corrispondente server autoritativo |
| CNAME | Name è un alias per un canonico dominio (east.backup2.ibm.com sta per ibm.com)      |
| MX    | Value è il nome di un mailserver associato a name                                                                                    |

Il formato dei pacchetti DNS è la stessa sia per la **query** che per la **response**.

![[Pasted image 20221116123735.png]]

# Strato applicazione
---
Le applicazioni possono seguire due paradigmi per le architetture:
- Modello client server 
- Modello P2P

Nel P2P i nodi coinvolti sono "alla pari" e io che voglio usare un'app sono il client ma questo servizio è fornito da un altro host (altro peer) che fungerà da server.
In pratica i peer possono fungere da server (non always on).
L'architettura è **self-scalable** in quanto si auto gestisce in quanto i nodi fungono sia da client che da server.

Il modello Client server è già stato ampliamente discusso. Unica nota è che non è self-scalable perchè aumentano i client appensantisco il server e dovrò aumentare i server.

Distinguiamo il **client process**, ovvero colui che inizia la comunicazione e il **server process** che vuole essere contattato.
Questi due tipi di processi comunicano tramite **socket**.

Lo strato applicativo è necessario per definire:
- tipo di messaggio che si vuole scambiare
- la sintassi del messaggio
- la semantica del messaggio
- le regole che i processi devono seguire
- i protocolli open (es. http)
- i protocolli proprietari

L'applicazione sfrutta i servizi offerti dal [[OSI 4 - Strato Trasporto]] e in particolare:
- integrità dei dati (tcp)
- tempistiche (udp)
- throughput (udp)
- sicurezza (tcp)

In particolare per la sicurezza in TCP e UDP si usa un protocollo SSL che garantisce una trasmissione criptata e la end-point authentication.


# HTTP
---
Abbreviazione per **Hypertext transfer protocol** è un protocollo che si occupa di fornire la pagina HTML del server a cui si fa una **HTTP Request**.
Il server manda una **HTTP Response** contenente la pagina HTML.

![[Pasted image 20221128211455.png]]

E' un protocollo **Stateless** dunque il server non memorizza le richieste precedenti.
Inoltra usa **TCP sulla porta 80**, perciò la connessione va aperta e poi chiusa.

| Persistent HTTP                                                       | Non Persistend HTTP                                                                            |
| --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| Una volta aperta la connessione TCP si possono richiedere più oggetti | Ogni volta che si richiede un oggetto la connessione va aperta e poi subito chiusa ogni volta! |


### Formato del protocollo HTTP

E' un protocollo di **tipo testuale** ovvero codificato in ASCII (a differenza dei livelli sotto dove è binario). Questo perchè è un protocollo molto vicino al programmatore, quindi deve ben capire come funziona e come si va a utilizzare.
Il formato della richiesta è il seguente

![[Pasted image 20221130103221.png]]

La prima riga specifica il **metodo** che può essere GET, POST, HEAD poi si specifica il **path del file html richiesto** e l'**hostname** del server che si vuole contattare.
Il campo **Connection** e **keep-alive** permettono di implementare HTTP persistente.
Questa richiesta è "inscatolata" nel seguente pacchetto.

![[Pasted image 20221128212121.png]]

### Metodi di HTTP

- Il metodo POST si usa per compilare i form mettendo i dati nella **entity body** del pacchetto HTTP
- Il metodo **URL-Method** basato du **GET** mette i parametri del form nel URL identificati da ? (scomodo, poco sicuro)
- Il metodo HEAD
- Il metodo PUT per caricare un file nel pathname specificato
- Il metodo DELETE si usa per eliminare cosa dal server in un path specifico
- Il **Conditional get** manda una request con i dati aggiornati solamente se sono stati modificati dopo una certa data

### HTTP Response

Il messaggio di risposta contiene come prima riga la **Versione** di HTTP e un **codice che indica lo stato della risposta** (200 successo, 404 not found ecc ecc).

### Cookie

I cookie sono un modo per memorizzare lo stato di un contenuto sul web (es. implementare il carrello di una pagina).
Viene generato un **codice** al primo accesso di una pagina web e si tiene traccia in un db dell'associazione cookie-utente. Quando riaccediamo il server può mandare pagine html personalizzate in risposta.

### Web Caches

E' possibile che il browser implementi una cache per le pagine già richieste.
Oppure la presenza di **proxy server** rappresenta la presenza di una cache a livello più distribuito.



# SMTP Simple Mail Transfer Protocol
---
Ci sono 3 componenti nella comunicazione mail:
- **User agent** come gmail, thunderbird che sono tipo i client mail
- **Mail server** che gestiscono determinati dominio di mail
- Un **protocollo** che regola la comunicazione

I Mail server gestiscono alcuni domini di mail e possoggono al loro interno una coda di messaggi che sono in attesa di essere spediti.

```ad-note
Spesso in un mail server si vuole tenere traccia di tutto ciò che succede per evitare mail fraudolente: perciò anche se voglio inviare un messaggio a @polito.it da @gmail.com il politecnico esige che si usi il mail server polito, così posso controllare traffico e applicare filtri per spam ecc...
```

SMTP usa **TCP** per la trasmissione sulla **porta 25** quindi c'è handshaking, tx, e chiusura.
I messaggi di comunicazione devono essere su **ASCII a 7 bit** 

La comunicazione fra client e server avviene nel seguente formato:

![[Pasted image 20221130111319.png]]

Il formato del messaggio MAIL è standardizzato dal RFC 882

![[Pasted image 20221130112038.png]]

Formato da un **Header** che si suddivide in To: From: Subject: che conosciamo nella quotidianità e infine il body codificato in ASCII e contiene solamente del normale testo

### MIME

Per inviare altro oltre al testo si ricorre al protocollo **MIME Multimedial Mail Extension** che prevede la definizione di header prima di questi contenuti multimediali:

![[Pasted image 20221130112455.png]]

I formati supportati da MIME sono: testo, immagine, audio, video e applicazione.

### POP e IMAP

Ora fino a questo punto della comunicazione siamo arrivati al mail server del destinatario, ma per far sì che l'utente finale possa leggere la mail dal suo relativo web server occorre un protocollo di **accesso al mail** che vanno a richiedere la mail al server.

Il protocollo **POP3** Post Office Protocol è composto da una fase di Autenticazione e quella di Transazione dove si ottiene effettivamente la mail.

![[Pasted image 20221130113016.png]]

Nella seconda fase il comando `list` mostra tutte le email che sono in attesa di essere lette; il comando `retr` recupera il comando tramite numero, e la `delete` elimina i messaggi dal server.
La modalità è quindi download e delete, ma è possibile anche fare download e keep
POP3 è stateless come HTTP, per risolvere ciò c'è IMAP.

IMAP non è stateless e contiene tutte le email nel server, permette l'organizzazione in cartelle e tiene traccia delle sessioni.

# BitTorrent
---
I file sono divisi in chunk, si usa un sistema di indexing per individuarli.
L'indexing è gestita dal **tracker**, un server che va popolato grazie a comunicazioni iniziali quando un utente diventa peer.

Quando un download finisce il peer diventa seed.
Come scelgo i chuncks da scaricare?
- **Local Rarest First**, periodicamente si chiede ai vicini la lista dei chunks che offrono, dunque si richiedono i chunks mancanti, dando priorità a quelli rari.
Come inviare i chunks?
- **tit-for-tat** ogni host manda i suoi chunks ai 4 peer che hanno offerto più roba in passato (molto gentile). Ogni 30 secondi però si sceglie un peer a caso con cui condividere per favorire quelli appena entrati nella rete.

# DHT Distributed Hash Table
---
Il sistema di indexing che era centralizzato nel tracker ora si sposta nei peer stessi (strano).
Definisco un ID NUMERICO e lo assegno a ogni peer e a ogni risorsa e poi assegno la coppia al peer che è più vicino alla chiave.

Se numero su $n$ bit allora possono numerare i peer con $2^n-1$ valori.

Esempio:
Chunck='prova'
key=hash(Chunk)
//key=15
assign(peer 16, key)

```ad-danger
Il peer 16 non contiene il chunk 15 ma contiene la lista indexing del chunk 15
```

Come mi muovo all'interno di questa rete di hash tables?
**Uso una circular DHT**, eventualmente ottimizzata con shortcuts.