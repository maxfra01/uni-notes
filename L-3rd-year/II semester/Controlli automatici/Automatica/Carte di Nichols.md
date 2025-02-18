Obiettivo: garantire una buona resistenza della stabilità con dei margini definiti, in modo da non superare picchi di risonanza pari a un valore dato.

# Diagramma di Nichols
---
Il diagramma di Nichols (DdNic) vuole rappresentare modulo e fase di una funzione ad anello del tipo:
$$
G(s)|_{s=j\omega}=G(j\omega)=M(\omega)e^{j\phi(\omega)} \quad \omega \in (0, +\infty)
$$
Vogliamo rappresentare modulo e fase su **un piano cartesiano**.
Usiamo come intervallo di riferimenti le fasi compresi nell'intervallo $(-360^{\circ},0)$

Possiamo leggere il **margine di guadagno** osservando che il punto A dei diagrammi di Nyquist corrisponde all'intersezione della curva di Nichols con l'asse verticale:

![[Pasted image 20230517090543.png]]

Per quanto riguarda il **margine di fase** sulla carta di Nichols, il punto C è l'intersezione della curva con l'asse orizzontale:

![[Pasted image 20230517090638.png]]

```ad-note
In Matlab è possibile ottenere il diagramma di Nichols, a partire dalla funzione ad anello, con il comando `nichols`
```


# Carta di Nichols
---
E' la rappresentazione grafica dei punti a modulo $M$ costante e fase $N$ costante.

![[Pasted image 20230517091120.png]]

```ad-note
In matlab questa mappa si può generare con il comando `ngrid('new');`
```

Di particolare interesse sono i luoghi a **modulo costante**, e per ricavarli dobbiamo sovrapporre carta e diagramma di Nichols:
Vogliamo ottenere un limite superiore al picco ri risonanza pari a $M_{r,lim}$, andiamo perciò a cercare la curva con moduli pari ad essa.
Dato che man mano che ci avviciniamo al centro il modulo aumenta vogliamo che **la curva sia esterna (e non tocchi) alla curva limite**:
$$
M<M_{r,lim} \implies \textrm{DdNic esterno alla curva } M_{r,lim}  
$$
![[Pasted image 20230517091532.png]]

Posso anche leggere il **limite inferiore del margine di fase**, ovvero l'intersezione del cerchio limite con l'asse orizzontale.

![[Pasted image 20230517092125.png]]

Devo perciò anche garantire che:
$$
M<M_{r,lim} \implies m_{\phi}> m_{\phi,lim}
$$
Perciò riassumendo, la condizione che la curva di $G_{a}$ sia esterna alla curva di $M_{r,lim}$ è una condizione **necessaria e suffieciente** per garantire che il modulo sia contenuto.
La condizione sul margine di fase è **necessaria ma non sufficiente**.

Per il margine di guadagno si procede vedendo quando la curva interseca l'asse verticale, ma dal punto di vista pratico è poco utile.

E' inoltre possibile ricavare le seguenti relazioni per i margini di fase e guadagno:

![[Pasted image 20230517093600.png]]

