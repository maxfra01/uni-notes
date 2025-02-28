# Introduzione
---
Fin'ora (da [[Grandezze di interesse sui segnali]] a [[Auto Correlazione]]) ci siamo concentrati su segnali **determinati**, ovvero che il loro andamento nel tempo è descritto da una precisa funzione matematica.
Ora vogliamo estendere i concetti per segnali con evoluzione aleatoria nel tempo e per farlo ci serviamo dei **Processi Casuali**.

L'idea è di raccogliere tante **realizzazioni** del segnale e descriverle con dei parametri basati sulla probabilità.

Un esempio di processo casuale è la tensione ai capi di un resistore non alimentato...

![[Pasted image 20221104164148.png]]

# Definizioni
---
Un **Processo casuale** è un modello probabilistico per un insieme di segnali.
Si associa per ogni realizzazione del segnale una probabilità.
Dunque un processo casuale si può pensare come un **insieme di funzioni caratterizzate statisticamente.**

Ad ogni istante di tempo $t_{0}$ ottengo una variabile casuale:
$$
x(t_{0},s_{1}) \quad x(t_{0},s_{2}) \quad x(t_{0},s_{3}) 
$$
corrispondente alle diverse realizzazioni del processo.

Possiamo dunque pensare a un processo casuale come **una sequenza di variabili aleatorie scandite dagli istanti temporali.**

La classificazione dei processi casuali aggiunge due particolari categorie alla già precedente classificazione per i segnali determinati:
- **Processo quasi determinato**, il segnale è esprimibile come un segnale determinato dipendente da una serie di variabili casuali 
- **Processo non quasi determinato**, quando il segnale non può essere espresso come qui sopra

Consideriamo un processo quasi determinato, allora lo definiamo come:
$$
x(t)=A \cos(2\pi f_{0}t +\phi)
$$
Dove $A$, $f_{0}$, e $\phi$ sono variabili casuali.
Al variare di queste 3 variabili si otteranno diverse realizzazioni del segnale.

Possiamo anche considerare dunque un segnale **determinato** come un un non determinato degenere con unica realizzazione con probabilità uguale a 1.


# Descrizione probabilistica
---
Avendo a disposizione un insieme $n$ di realizzazione possiamo definire le probablità congiunte di ogni possibile sottoinsieme.

E' possibile dunque caratterizzare completamente un processo casuale solo se possiamo definire ogni quantità statistica per ogni insieme di realizzazioni.

![[Pasted image 20221105112954.png]]

Per caratterizzare dunque ci si limita a una caratterizzazione statistica del primo ordine:
$$
f_{X}(x;t)
$$
In pratica definiamo la densità di probabilità del processo osservata su ogni possibile realizzazione.

### Media di un processo casuale
La possiamo definire come:
$$
m_{x}(t)=E\{X(t)\}=\int xf_{X}(x;t) \, dx 
$$
e rappresenta dunque un segnale determinato!

### Autocorrelazione di un processo casuale
Definita come:
$$
R_{x}(t_{1},t_{2})=E\{X(t_{1})X^*(t_{2})\}=\int \int x_{1}x^*_{2}f_{X_{1},X_{2}}(x_{1},x_{2};t_{1},t_{2}) \, dx_{1}  \, dx_{2} 
$$
Media e correlazione insieme definiscono la **caratterizzazione di insieme del processo**.


# Trasformazione di processi casuali
---

![[Pasted image 20221111114945.png]]

Un sistema ingresso-uscita deterministico che elabora un processo casuale da in uscita un altro processo casuale (analizzeremo i processi WSS visti in [[Processi Stazionari]]).

Normalmente per fare le cose complete dovremmo conoscere tutte le statische di ordine n del processo in uscita, ma ci limiteremo a **media e autocorrelazione**.
Inoltre ci limitiamo a consiederare sistemi lineari, dunque è possibile calcolare i momenti del primo e del secondo ordine.

Consideriamo il seguente esempio:

![[Pasted image 20221111115301.png]]

Quindi all'uscita cerchiamo di calcolare la media del processo in uscita:
$$
m_{Y}(t)=E[Y(t)]=E\left[ \int X(\tau) \, d\tau  \right]=\int E[X(\tau)] \, d\tau 
$$
Notare che abbiamo scambiato integrale e media per la linearità degli operatori.
L'autocorrelazione è invece:
$$
R_{Y}(t_{1},t_{2})=\int E[X(\tau_{1}) \cdot X^*(\tau_{2})] \, d\tau_{1} d\tau_{2} 
$$

## Trasformazioni LTI su WSS

In generale considerando un processo WSS e un sistema lineari LTi con una certa risposta all'impulso:
$$
m_{Y}(t)=E[Y(t)]=E[h(t)*X(t)]=E\left[ \int h(\tau)X(t-\tau) \, d\tau  \right]= \dots =m_{X}\int_{-\infty}^{\infty} h(\tau) \, d\tau 
$$
Ma qurll'ultimo integrale non è altro che la trasformata di Fourier della risposta all'impulso centrata alla frequenza nulla, dunque
$$
m_{Y}(t)=m_{X} \cdot H(0)
$$
E per l'autocorrelazione (calcoli su slide):
$$
R_{Y}(\tau)=R_{X}(\tau)*R_{h}(\tau)
$$

## Trasformazioni LTI su ingresso Gaussiano

La **densità di probabilità** è molto complicata da calcolare all'uscita, ma esistono casi particolari dove ciò è possibile (gaussiana):
In particolare sappiamo che **se l'ingresso è gaussiano allora anche l'uscita è ancora una gaussiana**.

La dimostrazione di ciò che segue è omessa e complessa, ma possimao considerare l'uscita del sistema come una sommatoria di tutte le possibili variabili causali GAUSSIANE $x(t_{i})$ pesate sulle relative risposte all'impulso.
$$
y(t)=\sum_{i}h(t-t_{i})x(t_{i})
$$

