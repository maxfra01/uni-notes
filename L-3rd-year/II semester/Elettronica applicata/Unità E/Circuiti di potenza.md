# Gestione dell'energia
---
Modalità per fornire energia a un circuito:
- Alimentatori
- Batterie primarie e secondarie (ricaricabili)

Per fare ciò ci sono dei principali elementi:
- transistori di potenza
- transistore BJT
- Diodi raddrizzati e Zener

Si definisce una **SOA Safe Operating Area** come una zona in cui i dispositivi lavorano in sicurezza, senza la possibilità di danneggiarsi.
Stessa cosa per la temperatura di lavoro, limite superiore per non danneggiarsi.

# Conversione dell'energia
---

### Conversione AC to DC

E' Presente nel circuito un **diodo raddrizzatore** che permette alla corrente di passare solamente in un solo verso.
La tensione ai capi del diodo attraversato da corrente si comporta in maniera ideale come un **cortocircuito** (nella realtà la tensione è molto piccola) indipendentemente dalla corrente.

![[Pasted image 20230524120159.png]]

Da questa caratteristica tensione corrente del diodo vediamo che per valori di tensione negativi si ha corrente nulla (ovvero non può condurre, circuito aperto).
Nel caso reale quando ci sono tensioni negative c'è un piccola **corrente di saturazione**, mentre il punto dal quale inizia a condurre è leggermente sopra gli $0V$.

![[Pasted image 20230524115040.png]]

Quando la tensione sinusoidale è positiva allora il diodo conduce, mentre nei ventri dell'onda non passerà corrente. Questa primo risultato è un segnale con le sole creste positive, dove il valore medio ora non è più nullo.
Il **condensatore consente di stabilizzare la tensione a un valore** che va calando con il tempo. (L'output effettivo è la linea tratteggiata).

### Diodi Zener

Il diodo zener funziona in modo opposto rispetto a un diodo normale.
Esso è progettato per lavorare in zona di **breakdown** ovvero quella regione in cui la corrente scorre nella direzione opposta a quella consentita normalmente.
L'inizio di questa zona corrisponde al semipiano sinistro al punto di tensione $V_{Z}$ (tensione di Zener, breakdown).

![[Pasted image 20230524120950.png]]

Modellizzaiamo la pendenza del diodo con **una resistenza equivalente** pari a $R_{z}=\Delta V/\Delta I$.
La **Corrente minima** $I_{Z,min}$ è la minima corrente da garantire per stare nella zona della  caratteristica dove (circa) la tensione non cambia con la tensione.
La **Potenza dissipata** $P_{d,max}$ rappresenta un limite superiore per la corrente, che non deve superare una certa soglia.

Possiamo usare il diodo zener insieme a una resistenza (limite in corrente) per generare un riferimento di tensione.

### Transistore BJT

Il **Bipolar Junction Transistor** è formato dalla giunzione di due semiconduttori drogati in modo differente:

![[Pasted image 20230524121941.png]]

I tre terminali sono: **collettore**, **base**, **emettitore**
La **Corrente di collettore** e quella **di base** sono indipendenti, mentre la terza rispondea alla legge di Kirchoff.
La relazione base dei transistori bipolari è la seguente:
$$
I_{c}=\beta I_{b}
$$
Dove $\beta$ è il **guadagno in corrente**.
Altri parametri importanti sono la **tensione collettore emettitore di breakdown**

Il transistore ha due principali usi: amplificatore e interruttore.

![[Pasted image 20230524122418.png]]

Il parametro $\beta$ diminuisce per correnti elevate!

### Safe Operating Area (SOA)

Ogni dispositivo ha limiti di corrente, tensione e potenza.
In particolare si richiede che:
$$
\begin{cases}
V < V_{BRK} \\
I < I_{MAX} \\
P = IV < P_{d,MAX}
\end{cases}
$$
La prima condizione evita condizioni di breakdown delle giunzioni.
La seconda evita l'innalzamento delle temperature che potrebbero danneggiare il circuito.
La terza evita ancora il surriscaldamento che altrimenti sfocerebbe in deriva termica (breakdown secondario).

Per i transistori BJT la SOA assume la seguente forma:

![[Pasted image 20230524125521.png]]

Il fenomeno della dissipazione termica è modellizzabile tramite il seguente circuito:

![[Pasted image 20230524134016.png]]

La massima temperatura che vogliamo raggiungere è circa:
$$
T_{j,max}=T_{amb}+ P_{D,max} \theta_{JA}
$$
Quindi la massima potenza può essere ricavata da questa relazione, tenendo conto che **per dispositivi in silicio** la temperatura è circa di 150°



# Uso dei transistor come interruttori
---
