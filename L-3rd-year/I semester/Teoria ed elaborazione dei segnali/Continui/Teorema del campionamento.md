# Sistemi ADC e DAC
---
Consideriamo la necessità di avere un sistema del genere per poter manipolare informazioni sul segnale.

![[Pasted image 20221118101800.png]]

E' fondamentale che l'utente finale del segnale si trovi davanti un segnale che corrisponde (idealmente uguale) a quello di ingresso, ovvero
$$
v_{out}(t) \simeq v_{in}(t)
$$
In questo capitolo l'**obiettivo sarà proprio questo qui sopra**.

### Campionamento

Si selezionano i valori del segnale a determinati istanti temporali equispaziati sull'asse dei tempi.
Si considerano gli istanti multipli di $T_{c}$ dunque si campioneranno i valori 
$$
x_{i}=x(iT_{c})
$$
Si considereranno solo campionamenti uniformi nel tempo, ovvero non cambia mai e rimane costante per tutta la cattura.

Ovviamente **più il tempo di campionamento è piccolo maggiore è la qualità del campionamento**.
Ci chiediamo qual è il numero ottimale di campioni per costruire un segnale discreto a partire dall'analogico **senza perdere informazione**.

### Quantizzazione

E' un'operazione non lineare che consiste nell'associare a ciascun intervallo di ampiezza in ingresso un singolo valore discreto (tipicamente un gruppo di bit).

Ovviamente codificando su $n_{bit}$ si hanno $2^{n_{bit}}$ codici di uscita diversi.
Alla quantizzazione è sempre associato un **errore di quantizzazione**.

Definiamo il **bit rate** di un sistema ADC come:
$$
R_{b}=n_{bit} \cdot f_{c}
$$


# Teorema del campionamento
---

Un segnale continuo $x(t)$ può essere campionato e poi perfettamente ricostruito dai supi campioni solo se:
$$
f_{c}> 2B_{x} \implies T_{c}< \frac{1}{2B_{x}}
$$
```ad-note
- La quantità B_x è da intendere come la frequenza massima del segnale (banda unilatera)
- La frequenza alla quale si ha l'uguaglianza è detta **frequenza di Nyquist**
```

Il **filtro di ricostruzione $K(f)$** deve essere:
- piatto nella banda del segnale
- 