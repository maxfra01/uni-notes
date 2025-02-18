# Intro e richiami
---
Fino ad ora abbiamo sempre ignorato le interconnessioni all'interno di circuiti. In realtà nei IC le interconessioni definiscono le prestazioni del circuito, perciò è fondamentale modellizzarle e studiarle.

Al giorno d'oggi il principale fenomeno che causa colli di bottiglia è la distribuzione delle energie e dei segnali.

Questo discorso vale per far si che sia garantita **l'integrità di segnale**.
Occorre studiare:
- come i segnali si propagano e come farli arrivare integri a destinazione
- come distribuire l'energia per l'alimentazione

### Richiami su stato H,L

Abbiamo precedentemente visto che 0 e 1 sono in realtà codifiche di tensioni a livello alto e basso:

![[Pasted image 20230418165020.png]]

Le soglie che abbiamo fissato affinchè si possa interpretare il segnale sono state chiamate $V_{OH}$ e $V_{OL}$

Il modello che usiamo per una porta a cui arrivare la tensione è il seguente

![[Pasted image 20230418165132.png]]

Se applichiamo un segnale che ha un valore di tensione fra le due soglie allora l'uscita è indefinita, ma nel caso di due porte si può realizzare un data split:

![[Pasted image 20230418165515.png]]

In alcuni casi le interconessioni usano una codifica differenziale.
Uno 0 o 1 viene memorizzata come differenza fra le tensioni di due conduttori:

![[Pasted image 20230418165610.png]]

Uno svantaggio è dato dal fatto che due fili al posto di uno causano problemi di costo e spazio. Il vantaggio risiede nel fatto che questa configurazione ha una forte tolleranza ai rumori, dato che questi ultimi agiscono sulla tensione di modo comune e non sulla differenza.
Il **modo comune non è significativo** anche se deve rientrare in certi margini. In ogni caso sono disposto ad accettare margini di rumori maggiori.

### Richiami su parametri temporali

![[Pasted image 20230418165848.png]]

Abbiamo i $t_{rise}$ e $t_{fall}$ che sono parametri critici da valutare (sono valutati da 10% al 90%).
Poi abbiamo i $t_{H}$ e $t_{L}$ ovvero i tempi di fase alta e fase bassa (presi al 50%).
Nel caso di periodici abbiamo come parametri periodo e duty cicle.

![[Pasted image 20230418170026.png]]

Abbiamo poi i tempi di propagazione da stato alto e basso. $t_{PHL}$ quando l'ingresso sale e l'uscita scende. $t_{PLH}$ è l'inverso.

Poi abbiamo i parametri per i FF, ovvero il tempo di setup $t_{su}$ e il tempo di hold $t_{H}$ affinchè si eviti la metastabilità.

### Nuovi parametri temporali

E' possibile che il CLOCK non sia perfettamente regolare:
![[Pasted image 20230418170313.png]]
Questo fenomeno è noto come **clock jitter** e dunque durante il calcolo di massima frequenza di funzionamento è necessario tenere in conto questo nuovo parametro temporale $T_{j}$

In una logica sequenziale, ho quindi che la massima frequenza di funzionamento è funzione di questi parametri:
$$
F_{max}(T_{CO},T_{L},T_{su}, T_{L})
$$
Un altro parametro che va tenuto in considerazione sono i ritardi **delle interconnessioni**, ogni singolo tratto di filo avrà un suo ritardo.

![[Pasted image 20230418170655.png]]

Un altro parametro simile al jitter è noto come **tempo di skew** ed è definito sempre fra due segnali $t_{K}$

![[Pasted image 20230418170751.png]]

Nominalmente i segnali sono identici ma a causa delle interconnessioni diverse si ha un ritardo di fase.
Sia lo skew che il jitter sono variabili casuali e di solito sono noti i margini superiori (min=0).


# Interconnessioni
---
Consideriamo due dispositivi **driver e receiver** che comunicano livelli di tensione diversi.
Una prima modellizzazione di interconnessione è un filo ideale **equipotenziali**, ma nella pratica questo non avviene e l'andamento delle tensione è di questo tipo:

![[Pasted image 20230418171101.png]]

La tensione rossa sul receiver è un po' traslata a destra a causa del ritardo di trasmissione, inoltre sono presenti delle variazioni di forma sull'uscita.

Come possiamo studiare le interconnessioni? Approccio simile al modello ISO OSI, e in queste lezioni ci concentreremo sugli aspetti fisici-elettrici, come per i primi due strati fisico e collegamento.
L'obiettivo in questo capitiìolo è l'**integrità di segnale**.

### Modellazione con interconnessione IDEALE

![[Pasted image 20230418171751.png]]

Per il driver usiamo il modello con un generatore che sarà acceso a $V_{DD}$ o spento a seconda se vogliamo trasmettere 0 o 1.
Il ricevitore ha una resistesta infinita ideale e un condensatore.
Per prima cosa consideriamo l'interconnessione come un punto equipotenzale $B =C$.
Possiamo già individuare il $T_{TX}$ che è il tempo di trasmissione e lo skew $T_{K}$

In questo caso unendo tutto abbiamo modellato un filtro **passa basso RC**:

![[Pasted image 20230418172039.png]]

E questo si traduce in una risposta sul receiver di tipo esponenziale:

![[Pasted image 20230418172141.png]]

Notiamo che la tensione $V_{T}$ influenza il tempo di trasmissione, inoltre dipende da: livelli di uscita finale e iniziale dell'uscita del driver, soglia del receiver, resistenza di uscita del driver e capacità del receiver.
Questo tempo varierà fra un minimo e un massimo e questo corrisponderà a uno skew:
$$
T_{K}=T_{TX,max}-T_{TX,min}
$$
![[Pasted image 20230418172524.png]]

Lo **skew può modificare le relazioni temporali fra segnali** e bisogna tenerne conto.

# Esercizio C1.2
---

![[Pasted image 20230418174659.png]]

Considero l'ingresso del driver che chiamo punto A.
Ho un segnale logico che va da L a H su $V_{A}$
$V_{B}$ ovvero l'uscita del drivere sarà simile ma traslato di poco verso destra per tener conto del ritardo.
$V_{C}$, ingresso del receiver non avrà ritardo in quanto interconnessione idelae e $V_{D}$, uscita del receiver sarà traslata verso destra.

$V_{B}$ in particolare sarà di tipo esponenziale, a causa del filtro passa basso. Ci chiediamo in che istante il segnale $V_{B}$ attraversa la soglia alta e bassa $V_{IL}$ e $V_{IH}$ in modo da ricavare rispettivamente $T_{TX,min}$ e $T_{TX,max}$

L'andamento di $V_{B}$ è del tipo:
$$
V_{B}(t)=V_{\infty}+(V_{0}-V_{\infty})e^{-t/\tau}
$$
Possiamo ($V_{0}=0, V_{\infty}=V_{DD}, \tau=RC$) poi ricavare il tempo come:
$$
t=\tau \ln\left(  \frac{V_{DD}}{V_{DD}-V_{L}} \right)
$$
Dove $V_{DD}$ corrisponde a $V_{\infty}$ e $V_{L}$ la tensione che voglio andare a considerare.
Mettendo al posto che $V_{L}$, prima $V_{IH}$ e poi $V_{IL}$ otteniamo i tempi massimi e minimi di trasmissione, successivamente possiamo farne la differenza per trovare lo skew.

### Topic unità

[[C2-Modelli a linea]]
[[C3-Connessioni con linee]]
[[C4-Cicli di trasferimento]]
[[C5-Protocolli di Bus]]
[[C6-Collegamenti seriali]]
[[C7-Integrità di segnale]]
