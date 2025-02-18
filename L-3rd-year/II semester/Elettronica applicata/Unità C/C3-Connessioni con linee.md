# Introduzione
---
L'obiettivo di questo capitolo è quello di **minimizzare lo skew**: esso dipende da parametri del ricevitore e del trasmettitore, dalle propagazioni sulla linea e relative riflessioni, dai carichi capacitivi e dal rumore di massa e commutazione.

E' possibile effettuare collegamenti:
- **Punto-punto**, dove è presente un driver e un solo receiver
- **a Bus**, dove sono presenti alcuni receiver prima e dopo la linea
In ogni caso cerchiamo una struttura di riferimento per calcolare lo skew e progettare la linea in moda da minimizzarlo:

![[Pasted image 20230426123031.png]]

Possiamo calcolare sia lo **skew locale** considerando i tempi di trasmissione in una singola zona di interessa, o possiamo farlo in modo **globale** tenendo conto dei tempi di trasmissioni massimi e minimi su tutta la struttura.

Minimizzando lo switch si mantiene l'integrità del **livello di ciclo** che si occupa di garantire una corretta trasmissione delle informazioni nonostante lo skew.

**Incident ware Switching IWS** significa che la transizione da 0 a 1 viene fatto con l'onda incidente, quindi il primo gradino dev'essere sufficientemente grande.
**Reflected wave switching RWS** significa che la commutazione avviene con la riflessione indietro della tensione.

# Primo gradino
---
Occorre che per lo studio del sistema il primo gradino debba essere necessariamente abbastanza alto. Siccome esso dipende dal partitore di tensione fra la linea e il driver allora studieremo i 3 casi in cui:
- $\frac{R_{0}}{Z_{\infty}}$ basso, ovvero **bus veloci**
- $\frac{R_{0}}{Z_{\infty}}$ alto, ovvero **bus lenti**
- $R_{0}=Z_{\infty}$ ovvero un **driver adattato**

| Bus veloci                                                          | Bus lenti                                                 | 
| ------------------------------------------------------------------- | --------------------------------------------------------- |
| Il primo gradino è ampio                                            | Il primo gradino sarà bassa, ma necessarie le riflessioni |                 
| Commutazione su onda incidente (IWS)                                | Commutazione su onda riflessa                             |                 
| Basso $t_{TX}$                                                      | Alto $t_{TX}$                                             |                 
| Coefficiente di riflessione negativo, dunque presente osciallazioni | Salita lenta, senza oscillazioni                          |                 
| Richiede opportuna terminazione al lato remoto $R_{T}=Z_{\infty}$ perchè non necessarie le riflessioni   | Non sono necessarie terminazioni                          |     

### Basso $\frac{R_{0}}{Z_{\infty}}$

Sono presenti **commutazioni su onda incidente (IWS)**.
Già il primo gradino supera in genere la soglia di 1 logico e viene interpretato correttamente.
Questa soluzione è usata nei **bus ad alta velocità**.

![[Pasted image 20230426124745.png]]

### Driver adattato

Primo gradino non molto alto, ma alla terminazione il segnale si riflette e a quel punto, siccome $R_{0}=Z_{\infty}$, avrò una sola riflessione che mi farà superare la soglia di 1 logico e poi nessun'altra.
Questa configurazione è usata per collegamenti punto punto.

![[Pasted image 20230426124719.png]]

### Alto $\frac{R_{0}}{Z_{\infty}}$

Il primo gradino è molto basso, quindi necessario attendere un tempo molto lungo per la commutazione. Occorre che **le onde riflesse abbiano la stessa polarità**.

![[Pasted image 20230426124839.png]]


### Tipi di terminazione

Un primo tipo è **terminazione parallelo**, che ha un elevato consumo (passa sempre corrente nella linea), ma utile per bus veloci.
La terminazione è collegata a una tensione di 1 logico, ma dal punto di vista della riflessione non cambia nulla.
Inoltre questa configurazione può essere gestita a livello intermedio.

![[Pasted image 20230426124936.png]]

Un secondo tipo è la **terminazione serie** che prevede l'aggiunta di una resistenza in serie fra il driver e la linea.
Siccome la terminazione è aperta la potenza dissipata staticamente è nulla.
Dal lato negativa è più lenta.
Non possibile la gestione intermedie (resistenze diverse viste da diversi punti).

![[Pasted image 20230426125325.png]]

Cercheremo una resistenza $R_{S}$ tale per cui il coefficiente di riflessione verso la linea sia 0, ovvero vogliamo che la somma di $R_{0}+R_{S}$ sia uguale a $Z_{\infty}$

### Effetti del carico capacitivo

Considerando una topologia a bus, le schede collegate in diversi punti sulla linea crea un carico capacitivo che va a modificare i parametri della linea.
In particolare un **carico capacitivo**:
- Riduce l'impedenza $Z_{\infty}$
- Riduce la velocità di propagazione $P$
- Riduce l'ampiezza del primo gradino
- Aumenta il consumo dinamico

Occorre dunque modificare i carichi capacitivi, usando tracciati di lunghezza limitata, fare accorgimenti di layout, usare buffer di separazione:

![[Pasted image 20230426125752.png]]

# Sommario
---
- IWS
- Driver adattato
- RWS

![[Pasted image 20230426125936.png]]
