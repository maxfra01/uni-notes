# Quality of Service

Obiettivo per **applicazioni multimediali** è avere:
- **poche perdite**
- **delay costante**
- **elevato throughput**
Per chiamate audio dobbiamo mantenere il delay al di sotto dei 100-150 ms, mentre per **tactile internet** si vuole stare sotto al millisecondo.

Per supportare il QoS:
- Traffic classification
- Scheduling
- Control on traffic
- QoS routing

### Traffic classification

Identifico i pacchetti che hanno qualità che deve essere garantita, quindi memorizzerò queste informazioni in buffer differenti nella rete che avranno maggiore priorità: ad esempio divido il buffer di un router in 2 e divido traffico best effort e traffico real time.

Per identificare il traffico usa la **5-tuple**:
- Destination IP
- Source IP
- Transport protocol
- Destination port
- Source Port

### Scheduling

Diverse strategie tra cui **FIFO**:

![[Pasted image 20240111152219.png]]

**Statistical multiplexing**:

![[Pasted image 20240111152236.png]]

i pacchetti vengono sequenziati sul link di uscita in modo casuale in base all’ordine di arrivo.

Per garantire la QoS è necessario analizzare tutti i pacchetti e inserirli in code **multiple** servite in base alla priorità utilizzando degli algoritmi di scheduling, come:
- Priority Queuing
- Round Robing
- Class Based Queuing (CBQ)
- Weighted Fair Queuing (WFQ): sapendo il tipo di traffico di ogni applicazione è possibile configurare i nodi in modo da rispettare i requisiti.
- Deadline queuing: è possibile impostare un deadline per ogni pacchetto, in modo da garantire che il pacchetto venga inviato entro un certo tempo.

### Traffic control

Al fine di garantire un QoS, è possibile definire un azione di policy e shaping per stabilire se il traffico in ingresso nella rete ha il profilo adatto per entrare. In particolare si può adottare la tecnica del leaky bucket che permette di controllare il flusso di ingresso.
I pacchetti non conformi vengono rinviati, diminuiti di priorità (best effort) oppure scartati.

![[Pasted image 20240111152808.png]]

### Call Admission Control (CAC)

La Call Admission Control (CAC) consente di eseguire signalling mediante la descrizione del traffico generato e del servizio richiesto ed effettua **reservation**.

### IntServ

**IntServ** garantisce il QoS, effettua la prenotazione delle risorse (RSVP) riuscendo a garantire QoS a ogni singolo flusso.
Ha come criticità la complessità e la bassa scalabilità (non utilizzabile su scala elevata). Per quanto lo standard sia pronto e implementato nei router, non viene utilizzato.
La soluzione è stata standardizzata in modo da consentire alle applicazioni di richiedere e ricevere dalla rete una qualità del servizio in accordo alle proprie esigenze.

### DiffServ

**DiffServ** non garantisce il QoS e non consente di riservare le risorse.
Distingue il traffico in classi diverse identificate dal campo DS Field. Combinando questa differenzia‑
zione, insieme al network/traffic engineering e all’accesso controllato, è possibile limitare i pacchetti
presenti nel buffer.
Purtroppo soffre di bassa efficienza (best effort), ma consente semplicità e scalabilità.