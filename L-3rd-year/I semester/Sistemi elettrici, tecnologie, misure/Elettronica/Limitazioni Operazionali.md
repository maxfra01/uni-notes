# Amplificazione finita
---
Finora abbiamo considerato amplificatore ideali data l'amplificazione $A_{d} \to \infty$ , ma nel caso reale l'amplificazione $A_{d}$ è finita, dunque non posso considerarlo ideale.

# Limitazione di dinamica
---
Considerando un amplificatore di tensione non invertente, con guadagno pari a $A_{v}=1+\frac{R_{2}}{R_{1}}$ mi chiedo qual è la dinamica del segnale che posso inviare all'ingresso dell'operazionale.
Esiste una **limitazione** sul segnale in uscita.
Nel caso di segnale in uscita che non rispetti la **dinamica d'uscita** il segnale in usicita verrà troncato:
$$
V_{out,min}<v_{out}(t)< V_{out,max}
$$
I valori corrispondenti alla dinamica d'uscita si trovano nei datasheets.

Oltre alla dinamica sulla tensione d'uscita è presente anche una **dinamica sulla corrente** in uscita dall'operazionale:
$$
I_{out,min}< i_{out}(t) <I_{out,max}
$$
Ovviamente va confrontato il valore in uscita (se compatibile con la dinamica) con il valore effettivo della tensione su un carico $R_{L}$, dipendente dalla corrente in uscita.

E' possibile che l'amplificazione di modo comune sia rilevante nel calcolo dell'uscita, perciò anche lui è soggetto a limitazioni sulla dinamica.
Chiamiamo qust'ultima **dinamica di modo comune**, anche detta **CMR, Common Mode Input Range**.
$$
V_{CM,min}<v_{cm}<V_{CM,max}
$$
Dato che $v^+ \approx v^-$ allora posso andare a chiedermi se il valore di $v^+$ rientra nel CMR.

# Slew Rate
---




# Errori Statici
---
## Offset 

A causa di difetti di fabbricazione il punto di funzionamento a riposo non è mai nullo e ciò si traduce in una traslazione della transcaratteristica dell'opamp.
Per questo motivo se applico $v_{d}=0$ otterrò qualcosa del tipo $v_{out} \neq 0$.

![[Pasted image 20221213092100.png]]

Per ottenere una tensione del tipo $v_{out}=0$ è necessario applicare in continua una tensione supplementare in ingresso, e ciò corrisponderà a una traslazione della caratteristica a sinistra.

Concenttalmente un OP AMP affetto da offset si può considerare come un OP AMP non affetto fa offset con all'interno un generatore in continua che ne provoca l'offset stesso: questa tensione è detta **Input Offset Voltage**

![[Pasted image 20221213092537.png]]

```ad-note
Per studiare l'offset occorre:
1. spegnere i generatori indipendenti e considerare il modello di OP AMP con offset 
2. studiare l'uscita dipendente dall'offset
3. considerare il caso peggiore e studiarne la statistica
```

## Correnti di polarizzazione

Nella realtà i terminali degli OP AMP erogano o assorbono una piccola quantità di corrente, detta $I_{BIAS}$, che definisce il **Input Bias Input**:
$$
\begin{cases}
\textrm{input bias current} \quad   I_{BIAS} \\
\textrm{input offset current} \quad  I_{OFF}=I^+-I^-
\end{cases}
$$
Questo avviene per lo più in OP AMP realizzati con BJT.
Se usiamo i MOS le correnti sono dell'ordine dei pico-Ampere dunque pratiacamente trascurabili.

Come per l'offset possiamo considerare il tutto ideale andando ad inserire i generatori che determinano gli effetti degli errori statici.

![[Pasted image 20221213094019.png]]

Queste correnti sono particolarmente dannosi per circuiti di tipo capacitivo, in quanto possono andare a caricare il condensatore portandolo fuori dinamica.
Per ovviare a ciò devo inserire un **percorso ohmico**, ovvero una resistenza che si avvicina a un circuito aperto (quindi R molto grande).

