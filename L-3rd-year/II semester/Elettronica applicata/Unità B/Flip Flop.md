# Tipi di Flip-Flop
---
I circuiti **sequenziali** veri e propri sono dotati di memoria e quindi l'uscita dipende anche dallo stato del circuito stesso.
Alla base di tutto ci sono elementi di memoria

### Elemento di memoria base

![[Pasted image 20230307165748.png]]

Esistono due soli possibili stati in questa configurazione.
E' detto **latch** ed è costituito da due inverter collegati nel modo raffigurato.
Esiste un terzo modo di configurazione, che corrisponde a un punto di malfunzionamento:
![[Pasted image 20230307170100.png]]
Nella posizione Sx, la prima perturbazione (rumore) può modificare lo stato del latch verso il primo stato e il secondo.
Questo concetto è noto come **metastabilità**.

### FF set-reset

![[Pasted image 20230307170400.png]]

I comandi esterni permettono SET e RESET realizzano le varie funzionalità del FF:
- SET=0, RESET=0 rappresenta lo stato di memoria, in pratica il circuito risultante è il latch
- SET=1, RESET=0 l'uscita Qb risulta a 0, mentre Qa a 1
- SET=0, RESET=1 l'uscita Qb è a 1, mentre Qa è pari a 0
- SET=1, RESET=1 rappresenta una configurazione proibita perchè potrebbe portare a **metastabilità**.

Il simbolo funzione del FF set-reset è:
![[Pasted image 20230307171252.png]]



### Latch D

![[Pasted image 20230307175614.png]]

Gli ingressi set e reset sono collassati in un unico comando **D**, inoltre ci sono porte AND con il set, reset e il clock, in modo da rendere il FF sincrono.
- Con LE=0 siamo in condizione di memoria
- Con LE=1 siamo in condizione di **trasparenza** perchè l'uscita Q è pari a D.

### Comandi asincroni

Certe volte si vuole impostare un valore noto al flip flop e in maniera asincrona:
![[Pasted image 20230307180510.png]]
Se PRESET e CLEAR sono a 0 allora è come se non ci fossero, perchè in OR con l'altro ingresso.
Normalmente vengono usati per imporre condizioni iniziali.

### Master-slave tipo D

Di fatto sono una cascata di due latch con abilitazione complementare CK e CK*:
- CK=0 abilita il primo latch dunque master trasparente e slave in memoria
- CK=1 abilita il secondo latch dunque master in memoria e slave trasparente
![[Pasted image 20230307181208.png]]

### FF DDR Dual Edge

![[Pasted image 20230307182605.png]]

A differenza di un master slave, i due latch stanno in parallelo e l'uscita è prelevata tramite un multiplexer (comandato dal CLK).
Poichè ci sono meno commutazioni c'è una sensibile riduzione del consumo.

### FF JK

Due ingressi, attivo sui fronti di clock:

![[Pasted image 20230307183822.png]]

Nella configurazione proibita si ha lo scambio delle uscite.



# Temporizzazione
---
A causa delle porte NOR in un flip-flop set reset esistono ritardi nella commutazione.
I comandi devono dunque avere una durata minima, in caso contrario si avrà solo un transitorio ma il flip flop non commuterà il proprio stato.

Nei SR, chiamo $t_{p}$ il tempo di propagazione su una porta NOR.
Allora i comandi SET, RESET devono essere superiori a $2t_{p}$.

Nei MS occorre che il comando sia sufficientemente lungo, di una quantità detta **tempo di setup $t_{su}$** e successivamente ancora attivo per un tempo, detto **tempo di hold $t_{h}$** per confermare le modifiche dello stato.

![[Pasted image 20230308114645.png]]

Se non rispettiamo le condizioni, ci troveremo in **metastabilità**.
Il ritardo che permette a un'uscita di stabilizzarsi in uno stato corretto è detto **resolving time $T_{res}$**, quest'ultimo maggiore del tempo di propagazione e predicibile in modo **statistico**.

Si possono avere errori tra due ambiti con **clock diversi**: per effettuare il passaaggio ricorriamo a diverse tecniche:
- O usiamo lo stesso clock per tutti, quindi bassa probabilità di metastabilità
- Se invece vogliamo risincronizzare con due clock diversi possiamo usare clock **agganciati in fase**.

### Massima frequenza operativa

Ignorando ogni possibile logica combinatoria:
- considero i tempi per la transizione fra stati $t_{R},t_{F}$
- devo garantire i tempi di hold e setup $t_{h},t_{su}$

Il clock minimo avrà un tempo $t_{ck,min}$ dunque:
$$
f_{ck,max}=\frac{1}{t_{ck,min}}=\frac{1}{t_{h}+t_{su}+t_{R}+t_{F}}
$$


