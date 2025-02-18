# Classificazione
---
Le classificazioni dei sistemi si fanno in base all'uscita ovvero in base alla relazione:
$$
y(n)=L[x(n)]
$$
E in particolari i sistemi si differenziano in base alla funzione matematica $L$.
- Per **lineari** intendiamo i sistemi che rispettano la proprietà di linearità.
- I **Tempo invarianti** o stazionari sono tali se in partica un sistema non cambia nel tempo o non dipende dall'istante in cui applichiamo il segnale, ovvero se vale la proprietà:
$$
y(n)=L[x(n)] \implies L[x(n-n_{0})]=y(n-n_{0})
$$

- Per **sistemi causali** intendiamo quei sistemi dove l'uscita $y(n)$ non dipende dai valori futuri dell'ingresso cioè da termini del tipo $x(n-n_{0})$.
- I **sistemi con o senza memoria** si distinguono se l'uscita $y(n)$ dipende da valori precedenti o solo dall'ingresso corrente.


# Equazione alla differenze
---
Per desscrivere ogni sistema LTI possiamo rifarci a una combinazione lineare a coefficienti costanti del tipo: $$

y(n)=- \sum_{k=1}^M a_{k} y(n-k) + \sum_{k=0}^N b_{k} x(n-k)
$$
Quest’equazione esprime l’uscita di un sistema LTI come combinazione lineare dell’ingresso attuale $x(n)$, degli $N$ valori precedenti assunti dall’ingresso e degli $M$ valori precedenti assunti dall’uscita stessa.

L'equazione si dice **ricorsiva** se almeno un coefficiente $a_{i}$ è diverso da zero.
Se tutti gli $a_{i}$ sono nulli allora si dice **non ricorsiva**.

Questa equazione è esprimibile sotto forma di diagramma a blocchi.

![[Pasted image 20221130121140.png]]

Per risolvere questa equazione alle differenze occorre scomporla in due:
$$
y(n)=y_{so}(n)+y_{io}(n)
$$

### Risposta allo stato nullo 

La parte $y_{so}(n)$ si dice risposta allo stato nullo e Rappresenta l’evoluzione del sistema con stato iniziale nullo, ma ingressi non nulli.
In sostanza corrisponde alla risposta di ingressi NON nulli e stato iniziale nullo.
$$
y(n)=
$$

### Risposta all'ingresso nullo

Rappresenta l'evoluzione del sistema con ingresso nullo tenendo conto delle condizioni iniziali del sistema.
$$
y_{io}(n)+ \sum_{k=1}^M a_{k} y_{io}(n-k)=0 \quad y_{io}(k)=y(k)
$$
Per i LTI stabili questa è anche detta "risposta al transitorio" dal momento che decresce fino ad annullarsi man mano che il tempo avanza.
Ci concentreremo su sistemi a regime (stato iniziale scarico, nullo)


# Risposta all'impulso di LTI
---
Supponendo di avere sistemi scarichi (condizioni iniziali nulle) definiamo la risposta all'impulso come la risposta allo stato nullo con in ingresso una delta $\delta(n)$
$$
h(n)=L[\delta(n)]
$$
Inoltre come nel caso continuo posso caratterizzare il sistema come:
$$
y(n)=x(n)*h(n)
$$
Per un sistema l'uscita è composta da due parti: una dei campioni già entrati nel LTI (parte causale) e una di quello che entreranno (anticausale).
In particolare per un sistema causale  la **risposta all'impulso è nulla per $n<0$**.