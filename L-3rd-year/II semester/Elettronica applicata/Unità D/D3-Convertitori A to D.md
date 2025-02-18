# Convertitore Delta
---
![[Pasted image 20230516161122.png]]

Una grandezza analogica $A$ è in ingresso a un comparatore, e inizialmente nello stato iniziale l'uscita del comparatore sarà alta. In maniera periodica ci saranno degli impulsi in uscita sul ramo $L(t)$ (per l'interrutore ceh viene chiuso e aperto): l'integratore finale integra i segnali e li ricostruisce fino a quando non si ottiene un $A_{R}$ uguale a quello in ingresso.

![[Pasted image 20230516161512.png]]

$L$ sono gli impulsi che sono generati a causa dello switch che si apre e si chiude.
Sotto troviamo il segnale in ingresso $A$ e quello ricostruito $R$.

Le dinamiche di questo convertitori dipendono dall'altezza del gradino $\gamma$ e il periodo di clock:
$$
\textrm{slew rate massimo}: \frac{\gamma}{T_{CK}} \quad  
$$
Mentre il minimo segnale analogico rilevato è pari alla metà del gradino:
$$
\textrm{V minimo}: V_{i,min}=\frac{\gamma}{2} \quad  
$$
Uno slew rate troppo alto porta a situazioni di overload.


# Convertitore SigmaDelta
---
![[Pasted image 20230516161849.png]]

Inserendo un derivatore all'uscita e un integratore all'ingresso riusciamo a ridurre l'ampiezza al crescere dalla frequenza, quindi limitiamo il problema dello slew rate.

Per elimanare la complessità del sistema possiamo raggruppare i due integratori ottenendo l'architettura del tipo:

![[Pasted image 20230516162021.png]]

Nello schema non sono indicati come sempre i circuiti di condizionamento, come i filtri anti aliasing, e quelli passa basso in uscita.

