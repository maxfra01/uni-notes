![[Pasted image 20230516162246.png]]

# Multiplexer
---
Se abbiamo un sistema con più canali (ovvero più ingressi di segnali analogici) andremo a selezionarne uno fra gli $N$.
Il multiplexer deve quindi modificare poco il segnale in transito e attenuare il più possibile gli altri.

La struttura del multiplexer è costituito idealmente da una serie di switch che vengono chiusi o aperti in base a un comando di controllo:

![[Pasted image 20230516171645.png]]

Quando un interruto è **chiuso ha una sua resistenza caratteristica** $R_{ON}$ che va a creare un partitore con altre resistenze in gioco:

![[Pasted image 20230516171803.png]]

Gli **switch che rimangono aperti** hanno una corrente di perdita caratteristica $I_{OFF}$, come conseguenza di ciò abbiamo un offset $V_{OFF}$ in uscita:

![[Pasted image 20230516171950.png]]

Sono poi inoltre presenti delle **capacità parassite**: in particolare $C_{DS}$ tra ingresso e uscita e $C_{GD}$ tra comando e uscita.
Queste capacità un **errore di offset dette piedistallo** e il **trasferimento di segnale non previso (feedthrough)**.

Infine $R_{ON},R_{S}, C_{p}$ formano un filtro passabasso che incide sulla banda del segnale trasferito.


# Sample and Hold
---
Vorremmo questo comportamento ideale:

![[Pasted image 20230516174927.png]]

Ma nella realtà l'inseguimento del segnale analogico richiede tempo e quindi l'andamento reale è simile al seguente:

![[Pasted image 20230516174953.png]]

Si parla di **track and hold**: ogni multiplo di $t_{S}$ si campiona il segnale con il valore corrente e lo si mantiene stabile per un tempo di hold $t_{h}<t_{S}$ poi c'è al fase di inseguimento in cui il segnale campionato insegue quello analogico.

Nella pratica realizziamo il circuito come segue;

![[Pasted image 20230516175442.png]]

Nel caso di interruttore chiuso siamo in fase di TRACK, mentre quando ho aperto allora è in HOLD.

### Errori in TRACK

In fase di acquisizione (TRACK) abbiamo un **errore di guadagno** a causa di un partitore e un **tempo di acquisizione** dovuto al condensatore:

![[Pasted image 20230516175645.png]]

### Errori in Campionamento

Nella fase di campionamento si ha una situazione del tipo:

![[Pasted image 20230516175730.png]]

$t_{A}$ è il tempo che impiega lo switch a commutare di posizione.
Dopo il tempo $t_{A}$ si ha l'istante di campionamento che varia a seconda del jitter $t_{JA}$
Poi abbiamo un errore per jitter di apertura: $\Delta V_{JA}=t_{JA}\cdot SR_{max}$

### Errori in HOLD

![[Pasted image 20230516180912.png]]

Abbiamo passaggio di cariche con switch aperto, decadimento della carica sul condensatore e il fenomeno del piedistallo.

Il tempo che il sistema impiega per inseguire il segnale (dopo una fase di hold) è detto **tempo di acquisizione** $T_{ACQ}$

Possiamo inoltre inserire due voltage follower per isolare il carico e il condensatore, in modo da risolvere alcuni errori che li coinvolgevano:

![[Pasted image 20230516181446.png]]

### Parametri

Tutti i problemi che abbiamo visto fino ad ora sono risolvibili, tranne per il jitter che quindi va considerato quando si calcola SNR e ENOB.

Per fare ciò si tratta il jitter come fosse un rumore.
Otteniamo così le formule per calcolare il SNR totale:
$$
SNR_{tot}=10\log_{10} \frac{P_{S}}{\sum P_{N_{i}}}=10 \log_{10} \frac{1}{\sum A_{i}}
$$
Dove $A_{i}=\frac{P_{N_{i}}}{P_{S}}$, inoltre notiamo che questo procedimento è valido perchè le fonti di rumore sono statisticamente indipendenti.

Una volta noto il $SNR_{tot}$ possiamo calcolare l'ENOB nel solito modo, ovvero:
$$
ENOB=\frac{SNR_{tot}}{6}-0.3
$$
