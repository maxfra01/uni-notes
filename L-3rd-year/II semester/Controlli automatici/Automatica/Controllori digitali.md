# Sistemi di controllo digitali
---
Finora il controllore da noi progettato è una funzione di trasferimento matematica. Nella realtà occorre **discretizzarlo**: lo schema generale per fare ciò è il seguente.

![[Pasted image 20230531083947.png]]

Il sistema è detto **ibrido** in quanto alcuni blocchi evolvono in tempo continuo e alcuni in tempo discreto.
Il funzionamento è il seguente:
- L'errore di inseguimento è discretizzato
- Il controllore digitale elabora questo segnale discreto
- Il comando generato nel dominio discreto viene convertito in analogico e dato in input al sistema.

### Modello matematico

Dato una funzione analogica $f(t)$ posso andare a scegliere un passo di campionamento, e successivamente trovare la **sequenza di campioni** come:
$$
\{f_{k}\} \implies f^*(t)=\sum_{k=0}^\infty f_{k} \cdot \delta(t-kT)
$$
A questo punto la sequenza di campioni può essere trasformata nella sua trasformata zeta o laplace:
$$
\mathcal{Z}(f_{k})=F^*(z)=\sum_{k=0}^\infty f_{k} \cdot z^{-k} \quad 
\mathcal{L}(f_{k}) = F^*(s)= \sum_{k=0}^\infty f_{k} \cdot e^{-ts}
$$
Notiamo che per passare dalla zeta a laplace è sufficiente considerare $F^*(s)=F^*(z)|_{z=e^{-st}}$

### Scelta del passo di campionamento

Dobbiamo scegliere un passo sufficientemente piccolo in modo che la pulsazione di campionamento $\omega_{s}$ sia almeno il doppio della componente più elevata $\omega_{M}$ del segnale. 
Nella pratica abbiamo che dovrà essere verificata  
$$
\omega_{s}\gg \omega_{M} \quad \quad con \,\, \omega_{M}=\omega_{B}
$$
Dobbiamo anche tenere conto del ricostruttore che introduce una perdita di fase in $\omega_{c}$
Non possiamo prendere $T$ troppo piccolo, altrimenti si potrebbero verificare problemi di quantizzazione e anche costi troppo elevati.

### ZOH

Il ricostruttore più usato è il **zero order hold**, che nell'intervallo $T$ mantiene $u(t)$ pari all'ultimo valore acquisito.
E' caratterizzato dalle seguenti risposte all'impulso:
$$
h_{0}(t)=\epsilon(t)-\epsilon(t-T) \quad \quad H_{0}(t)=\frac{1-e^{-sT}}{s}
$$
La fdt deve essere razionale fratta, perciò usiamo un **approssimazione di Padè del I ordine**:
$$
H_{0}(s) \approx \frac{T}{1+s \frac{T}{2}}
$$
Notiamo che c'è un polo in $s=-\frac{2}{T}$ che cresce con la frequenza, vogliamo che sia di alta frequenza per non farci perdere fase in corrispondenza di $\omega_{c}$

```ad-note
Se so già di dover discretizzare il controllore, allora è opportuno progettare $C(s)$ garantendo un margine di fase superiore a quanto strettamente richiesto.
```


### Metodi di discretizzazione

Il metodo delle **differenze all'indietro** consiste nell'approssimare l'area sotto una certa funzione $f(t)$ con un operazione di sommatoria.
Si prende il valore della funzione in una certa ascissa, si considera allora il rettangolo di altezza $f(t^*)$ nell'intervallo $(T_{-1},T)$.
Da **forti distorsioni**, quindi non va bene

Il metodo delle **differenze in avanti** è la stessa cosa ma l'intnervallo considerato è quell'istante preso in considerazione e l'istante di campionamento **successivo**.
Potrei ottenere un controllore instabile, non va bene

Il metodo di **Tustin** approssima le aree in maniera più accurata, usando non dei rettangoli ma dei trapezi, formati fra due campioni consecutivi.

Il metodo della **precompensazione in frequenza** è un miglioramento di Tustin con un termine aggiuntivo 

Il metodo dell'**invarianza della risposta al gradino** 

# Progetto per discretizzare
---
Come prima scelta si considera un passo di campionamento con periodo:
$$
T= \frac{2\pi}{\alpha \omega_{B}}
$$
Dove $\alpha$ è un valore numero che tipicamente **scegliamo intorno a 20**.
Il passo di campionamento $T$ che otteniamo va arrotondato per ottenere un valore accettabile.

Una volta fissato il $T$, deve verificare che una volta inserito il filtro ZOH, il nuovo margine di fase non sia sceso di troppo: quindi la nuova $G_{a}$ sarà pari a:
$$
G_{a,new} = \frac{G_{a,old}}{1+s \frac{T}{2}}
$$
Su questa $G_{a,new}$ dovremmo verificare il margine di fase che otteniamo.
Se il margine di fase $m_{\phi}$ risulta sufficiente devo abbassare $T$ il più possibile.
Nel caso non sia possibile ottenere un margine di fase sufficiente, devo rivedere $C(s)$.

Una volta fatto ciò uso il comando:

```MATLAB
Cz = c2d(C, T, "method")
```

Per ottenere un controllore discreto.

Successivamente posso discretizzare il sistema rimasto, da controllare e ottenere $F(z)$ e in catena chiusa $W(z)$.
Per valutare poi il sistema nel mondo discreto sono obbligato ad usare Simulink.
In Simulink devo solo sostituire $C(s)$ con $C(z)$, mentre gli altri blocchi possono stare lì, tanto l'informazione è contenuta nel controllore.
