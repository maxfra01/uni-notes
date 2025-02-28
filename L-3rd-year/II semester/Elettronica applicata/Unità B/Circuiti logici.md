# Introduzione
---
Interconnessione di **transistor** per creare porte logiche combinatorie; occorre rispettare vincoli di vario genere, come parametri statici, interfacciamento, valutazione dei ritardi e consumo di potenza.

Considerando un modulo digitale qualunque: ![[Pasted image 20230228170732.png]]
Possiamo sempre individuare una tensione di alimentazione e un riferimento GND; in ogni caso si cerca di usare tensioni basse per risparmio di energia.
L'input/output può essere parallelo o seriale.

Gli **stati logici** 0,1 sono rappresentati da tensioni e si usa una convenzione "logica positiva", oppure il livello basso è vicino alla tensione di GND, mentre un livello alto è vicino a $V_{AL}$
Al'uscita di circuiti logici troviamo l'equivalente del seguente schema:![[Pasted image 20230228171420.png]]
Spesso però a causa di imprecisioni occorre impostare intervalli (e non valori) per decidere se il livello è alto oppure basso.
E' necessario spostare il problema anche all'ingresso di una porta logica, dunque: ![[Pasted image 20230228172116.png]]
Facendo attenzione al fatto che bisogna rispettare: $$
V_{OL}<V_{IL} \cap V_{OH}>V_{IH}
$$Per far sì che questa relazione sia rispettata occorre che il rumore non superi mai la differenza fra i valori delle due soglie (**Noise Margin**):$$
\begin{cases}
NM_{L}=|V_{OL}-V_{IL}| \\
NM_{H}=|V_{OH}-V_{IH}|
\end{cases}
$$
La struttura dei circuiti digitali è la seguente:

![[Pasted image 20230228173228.png]]


# Uso dei transistor
---

Con singole porte MOS possiamo genericamente fare roba negata, nel senso che ingressi di stato HIGH in ingresso corrispondono a stati LOW in uscita, in quanto gli ingressi alti tendono ad accendere i transistor.

## Inverter CMOS

Combinando un transistore PMOS e un NMOS possiamo ottenere un **inverter CMOS**:

![[Pasted image 20230228180614.png]]

Considerando poi il modello base del transistore allora otteniamo i due seguenti modelli:

![[Pasted image 20230228180724.png]]

Nel primo caso l'ingresso è pari a $V_{DD}$ dunque il PMOS è aperto e NMOS si comporta in modo lineare.
Nel secondo caso l'ingresso è pari a 0, dunque NMOS sarà spento mentre PMOS si comporterà in maniera lineare.

E' importante notare che i due transistor competono per il comportamente del circuito complessivo:
![[Pasted image 20230228181450.png]]
La zona grigia è quella che determina la $V_{T}$ ovvero la soglia per decidere lo stato H o L.

Nel caso di molteplici transistori di P o NMOS occorre che siano rispettate le condizione:
- Quando il pull-down è acceso allora il pull-up dev'essere spento, e viciversa.

![[Pasted image 20230228182023.png]]

## Porta NAND e NOR

Per il caso di due ingressi e un'uscita
Abbiamo due PMOS in parallelo e due NMOS in serie, mentre per la porta NOR il contrario, cioè due PMOS in serie e due NMOS in parallelo.
Da qui deduciamo la presenza di una **dualità** fra i transistor in parallelo e quelli in serie.

![[Pasted image 20230228182852.png]]
![[Pasted image 20230228182903.png]]

## CMOS complessi

A partire da qualsiasi funzione logica da realizzare, si parte dal pull-down (NMOS) e ogni volta che c'è un + allora c'è un parallelo di NMOS, poi per ogni AND abbiamo una serie di NMOS.
Poi si traccia il pull-up sostituendo le serie con parallelo e viciversa.

## Multiplexer con pass gate e porte

Realizzare un multiplexer con porte classiche è costoso, perciò si utilizza un interruttore detto **pass gate** che corrisponde a una coppia di transistori che si aprono entrambi grazie alla configurazione:

![[Pasted image 20230228184014.png]]

Occorrono due transistori per ingresso in modo che si riesca a far passare correttamente il segnale senza degradarlo.

# Stadi in uscita
---
### Correnti in uscita: stato H

Occorre inoltre verificare se le correnti sono compatibili con il funzionamento dei transistor.
Consideriamo un'uscita di una porta a livello H, modellizzato come segue:![[Pasted image 20230301113926.png]]
Possiamo usare la maglia per dire che: $V_{O}=V_{AL}+R_{OH}\cdot I_{O}$
Per un corrretto funzionamento devo avere: $$
V_{O}>V_{OH} \implies I_{O}>I_{OH}
$$![[Pasted image 20230301114633.png]]

```ad-important
Per convenzione la corrente viene sempre rappresentata entrante nelle porte
```

### Corrente in uscita: stato L

Modellizziamo il problema come: ![[Pasted image 20230301114703.png]]
Possiamo notare che per il resistore $R_{OL}$ vale la legge di Ohm: $V_{O}=I_{O}R_{OL}$
Per garantire il funzionamento dobbiamo far sì che $$
V_{O}<V_{OL} \implies I_{O}<I_{OL}
$$![[Pasted image 20230301114928.png]]

Le due condizioni nei due stati formano un intervallo valido che la corrente può assumere per il corretto funzionamento.

## Circuito di uscita Totem Pole

Possiamo interpretare l'uscita di una porta logica come un deviatore fra tensione di alimentazione e massa.
![[Pasted image 20230301115351.png]]
Ma questo stadio non modella bene $R_{ON}$ e $I_{OFF}$


## Uscita a 3 stati

Un altro modo di modellare l'uscita è quello a 3 stadi dove aggiungo uno stadio di alta impedenza, ovvero un circutio aperto:
![[Pasted image 20230301115458.png]]

Un altro modello è il seguente, che implementa il **output enable OE**:
![[Pasted image 20230301115636.png]]

E' opportuno avere anche lo stato di alta impedenza se vogliamo collegare più porte logiche insieme, in questo modo si decide chi pilota il risultato in uscita.

### Open collector

In questo stadio di uscita è presente un unico transistore NMOS verso GND:
![[Pasted image 20230301120423.png]]
IL simbolo rombo sottolineato indica un uscita open drain.
Questa configurazione può produrre solo lo stato LOW quando è chiuso, mentre fornisce alta impedenza quando è aperto.
Per generare il valore logico alto c'è bisogno di una **resistenza di pull-up**::
![[Pasted image 20230301120623.png]]

L'open drain è utile per realizzare l'**OR cablato**.

# Parametri dinamici
---

### Salita e discesa, singolo segnale

Sono i tempi di transizione, ovvero il tempo che impiega il segnale a passare da livello alto a basso e viceversa.
Spesso sono definiti fra il 10% e il 90% del segnale: quindi il tempo di rise o fall è valutato da quando il segnale è al suo 10% in ampiezza a quando raggiunge il suo 90%![[Pasted image 20230301122517.png]]

### Ritardi fra coppie di segnali

Definiamo una soglia al 50% del segnale durante la transizione da alto a basso.
Per definire il segnale confrontiamo il valore fra le due soglie al 50:![[Pasted image 20230301122634.png]]

### Capacità dei transistor

I circuiti in ingresso dei CMOS possono essere modellati considerando una capacità equivalente:![[Pasted image 20230301123108.png]]
Se considero un collegamento in serie fra due porte, allora questo condensatore determinarà un andamento della tensione in entrata nella seconda porta di tipo esponenziale:
![[Pasted image 20230301123308.png]]
Questo andamento esponenziale determina un ritardo costituito dalla salita della tensione.
Spesso siamo interessati a sapere a quale tempo passiamo dal valore logico alto a basso, per farlo imponiamo l'esponziale uguale a un mezzo di $V_{AL}$ e troviamo il tempo.

### Effetti del carico

Consideriamo una porta che pilota molte altre porte.
Il problema è che aumentando le porte si aumenta la capacità equivalente del sistema, dunque **si aumentano i ritardi e i tempi di transizione**.
Dunque il **fanout** è limitato dal carico capacitivo.

### Ritardo con pass-gate

![[Pasted image 20230301124556.png]]

### Segnali differenziali

A scapito dello sdoppiamento dei canali questa configurazione è preferibile in quanto resistente al rumore, inoltre permette di lavorare con tensione più basse.

# Potenza
---
Si parla di **potenza statica** quando facciamo riferimento alla potenza assorbita in assenza di commutazione.
Inoltre dipende dalla corrente sotto soglia $V_{TH}$.
La potenza assorbita è circa costante, che varia con la temperatura e la $V_{AL}$ ed è modellabile come una corrente continua fra $V_{AL}$ e GND.
![[Pasted image 20230307161409.png]]

Il contributo **dinamico** della potenza dipende dalle commutazione di stato. E' modellabile come la corrente che carica/scarica la capacità in uscita.
Dipende dalle capacità, tensione di alimentazione, clock.
![[Pasted image 20230307161521.png]]
Considerano una carica $Q=CV$ e scaricando/caricando $F$ volte al secondo allora$$
P_{D}=VI=VFQ=VVFC=V^2FC
$$Ottenendo una dipendenza al quadrato dalla tensione di alimentazione, e una diretta dalla frequenza di clock.
Per risparmiare potenza abbiamo dunque due strade:
- Abbassare la $V_{H}-V_{L}$, ma rispettare i margini di rumore
- Abbassare la frequenza del clock, rendendo più lente le operazioni (rimediamo riducendo il numero delle operazioni, dunque va migliorato "l'algoritmo" per l'uscita)
- Ridurre le dimensioni e quindi abbassare le capacità.

Considerando una porta con consumo di potenza dissipata $P_{D}$ e con ritardo $T_{P}$ allora una metrica di qualità è il prodotto potenza-ritardo:
$$
P_{D} \cdot T_{P}
$$
Quanto più questa quantità è bassa maggiore sarà la qualità della tecnologia.

![[Pasted image 20230307162726.png]]

### Esempio

Considero un circuito con $V_{AL}$ di 2V, con $10^8$ ingressi con capacità 1fF, in media 50000000 transistor spenti in ogni istante.
Calcolare il consumo di potenza statica e dinamica con una frequenza media è 200MHz e Ioff=1 nA.

Per la potenza statica abbiamo 
$$P_{S}=n_{spenti}V_{AL} I_{off}= 50000000 \cdot 2 \cdot 10^{-9}
$$Mentre per la dinamica abbiamo 
$$
P_{D}=FC_{tot}V^2=200 \cdot 10^6 \cdot 4 \cdot 10^{-15} \cdot 100000000
$$
Dove $C_{tot}$ è il contributo di capacità di **tutti gli ingressi**.


### Topic unità

[[Circuiti sequenziali]]
[[Flip Flop]]
[[Logiche programmabili]]
[[Memorie volatili]]
[[Memorie non volatili]]
[[Oscillatori]]
[[Verilog]]
[[Esempi]]