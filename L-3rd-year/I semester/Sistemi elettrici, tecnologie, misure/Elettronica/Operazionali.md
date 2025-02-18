# Retroazione negativa
---
Fino ad ora considerando amplificatori a stadi comune abbiamo due principali limitazioni:
- **Effetti di carico** in ingresso e uscita, è complesso ottenere impedenze d'ingresso e d'uscita molto alte o molto basse
- **L'accuratezza dei parametri** in particolar modo di quelli attivi è oltre $\pm 50\%$

Per migliorare questi due aspetti si introduce il principio della **retroazione attiva**.
Fino ad ora abbiamo considerato amplificatori dove l'ingresso è direttamente collegato all'ingresso esterno, ciò significata che ogni perturbazione in ingresso viene amplificata.

Definiamo la retroazione come la correzione degli errori che ottengo in uscita: **negative feedback**

![[Pasted image 20221128085634.png]]

Quindi ottengo il segnale d'errore $\epsilon$ che è quello che effettivamente entra nell'amplificatore.
$$
\begin{cases}
y=A\epsilon \\
\epsilon=x-\beta y  \\ 
\end{cases}
\implies y \to \frac{1}{\beta}x \quad \epsilon \to 0
$$
Notiamo che abbiamo ottenuto uno stadio la cui amplificazione dipende non più dall'amplificazione ad anello aperto $A$ ma bensì dal blocco di reazione che ha coefficiente $\frac{1}{\beta}$
E' importante notare anche che $\beta$ è compreso fra $0$ e $1$ quindi c'è comunque amplificazione!
Questo ovviamente è valido **solamente se l'amplificatore è abbastanza grande in modulo**

Per studiare le azione di carico, errori, tolleranze su questo circuito usiamo un termine additivo $n$ 
Allora otteniamo un sistema di non idealità, dove svolgendo i calcoli
$$
\begin{cases}
y=A\epsilon +n \\
\epsilon= x-\beta y
\end{cases}
\implies y= \frac{1}{\beta}x
$$
Ci accorgiamo che l'uscita dipendente da $\beta$ ancora una volta non dipende da $n$, dunque **non è per nulla influenzato da qualsiasi difetto dovuto alla non idealità**.

Dovendo quindi ottenere un amplificatore che agisce sulla differenza e con altissimo guadagno, si è progettato appositamente l'amplificatore operazionale.


# Operazionale
---

![[Pasted image 20221128084519.png]]

Quest'oggetto è composto da numerosi (tipicamente 100) stadi comuni collegati in cascata necessari per arrivare a un elevato guadagno.
L'ingresso è dell'amplificatore operazione è di tipo differenziale ed è caratterizzato da un'elevata tensione in uscita (tipicamente $10^6$) 

All'interno dell'amplificatore **concettualmente** è presente un generatore di tensione pilotato in tensione (quella differenziale).

## Analisi di circuiti con operazionali

Consideriamo per questa parte anche le eventuali resistenze di ingresso e di uscita.

![[Pasted image 20221128094553.png]]

Dobbiamo individuare $A_{v}= \frac{v_{out}}{v_{in}}\,, R_{in}\,,R_{out}$ in due casi differenti, il primo per un generatore con $A_{d}$ finita e uno con $A_{d} \to \infty$ 

```ad-note
Una resistenza collegata direttamente ai morsetti di un operazionale non provoca cadute di tensione ed è trascurabile
```

Nel caso non ideale si mette a sistema l'espressione del generatore pilotato con l'espressione del pilota
$$
\begin{cases}
\hat{e}=A_{d}v_{d} \\
v_{d}=R'v_{in}-\beta' \hat{e}
\end{cases}
$$
E si risolve normalmente con calcoli laboriosi ma semplici.
Nel caso di **amplificatore ideale** non è necessario usare il metodo del pilota ma si semplificano diverse cose.
$$
v_{d}=0 \implies v^+=v^-
$$
Inoltre ricaviamo anche che la corrente nei morsetti è nulla:
$$
i^+=i^-= \frac{v_{d}}{R_{in,OP}}=0
$$
In questo esempio otteniamo il seguente risultato dimostrando che l'operazionale guadagna ancora $\frac{1}{\beta}$ nonostante gli effetti di carico in ingresso e uscita:
$$
A_{v}=\frac{v_{out}}{v_{in}}=1+ \frac{R_{2}}{R_{1}}
$$

Tramite gli operazionali è possibile realizzare tutti i tipi di [[Amplificatori]] che abbiamo visto nei capitoli precedenti.

![[Pasted image 20221129093434.png]]


# Circuiti Analogici
---
Sono analizzati i seguenti circuiti:
- Inseguitore di tensione 
- Amplificatore di tensione invertente
- Amplficatore esponenziale
- Amplificatore logaritmico
- Integratore
- Derivatore
- Sommatore
- Amploficatore differenziale
- Filtri attivi

### Inseguitore di tensione

![[Pasted image 20221129094802.png]]

Questo tipo di circuito è caratterizzato dalle seguenti equazioni:
$$
v_{out}=v_{in} \quad R_{in} \to \infty \quad R_{out}=0
$$
Utile per far arrivare una certa tensione ai capi di un bipolo minimizzando gli effetti di carico.

### Amplificatore di tensione invertente

![[Pasted image 20221129095656.png]]

Questo amplificatore non è ideale in quanto la resistenza in ingresso non è prossima all'idealità ma è finita. Il tutto ha le seguenti caratteristiche:
$$
A_{v}=\frac{v_{out}}{v_{in}}=- \frac{R_{2}}{R_{1}}\quad R_{in}=R_{1} \quad R_{out}=0
$$
### Integratore

![[Pasted image 20221205090841.png]]

Se al posto di una resistenza $R_{2}$ dell'amplificatore di tensione invertente mettiamo un condensatore, otteniamo un **integratore**.
Dato che i due morsetti sono a un potenziale di riferimento ($0$ volt) la tensione di uscita è uguale e opposta alla tensione sul condensatore.
$$
i_{c}=\frac{v_{in}}{R} \quad \quad \quad v_{out}=-v_{c}=-\frac{1}{C} \int_{0}^t i_{c}(t')  \, dt'= - \frac{1}{RC}\int_{0}^t v_{in}(t') \, dt'  
$$
Dunque otteniamo uno stadio con le seguenti caratteristiche:
$$
v_{out}=- \frac{1}{RC} \int_{0}^t v_{in}(t') \, dt' \quad R_{in}=R \quad R_{out}=0 
$$

### Derivatore

![[Pasted image 20221205092223.png]]

Scambiando resistenza e condensatore visti nel caso precedente otteniamo un **derivatore**.
Dato che entrambi i morsetti sono al riferimento la tensione in uscita è la tensione ai capi di $R$, ciò ci porta a scrivere che:
$$
v_{out}=-v_{R}=-R \cdot i_{R}=-RC\frac{ \partial v_{in} }{ \partial t } 
$$
Dunque otteniamo lo stadio caratterizzato da:
$$
v_{out}=-RC \frac{ \partial v_{in} }{ \partial t } \quad \quad R_{in} \to \infty_{DC}, \frac{1}{j\omega C}_{AC} \quad R_{out}=0
$$

### Sommatore

![[Pasted image 20221205093426.png]]

Il precedente circuito presenta un'uscita che corrisponde di fatto a una somma pesata dei vari ingressi in tensione. Di fatto ciò che realizza è:
$$
v_{out}=\sum_{i} k_{i}v_{i}(t)
$$
Andiamo di fatto a pesare i vari segnali in ingresso in base se sono attaccati al morsetto non invertente o invertente ($m$ indica il meno e $p$ indica il più)
$$
v_{out}=\frac{\sum_{i=0}^M G_{im}+G_{F}}{\sum_{i=0}^N G_{ip}}\sum_{i=1}^N \frac{G_{ip}}{G_{F}}v_{ip}- \sum_{i=1}^M \frac{G_{im}}{G_{F}}v_{im}
$$
Volendo scrivere l'uscita in una forma del tipo: $v_{out}=\sum_{i=1}^N k_{ip}v_{ip}- \sum_{i=1}^M k_{im}v_{im}$ allora dobbiamo assicurarci che la frazione iniziale sia pari a 1 e quindi verificare che 
$$
\sum_{i=0}^N G_{ip}=\sum_{i=0}^N G_{im} + G_{F}
$$
Se quest'ultima non è verificata è ancora possibile cercare di bilanciare l'equazione con le resistenze collegate a riferimento $G_{0m}$ e $G_{0p}$ 
Come bilanciare correttamente l'equazione?
Definiamo $G_{p}'$ come la somma delle conduttanze del morsetto non invertente e $G'_{m}$ come la somma delle conduttanze del morsetto invertente (escluse la $G_{0}$)
A questo punto impeniamo le uguaglianze:
$$
\begin{cases}
\textrm{if} \space G'_{p}>G_{m}' \quad G_{0m}=G'_{p}-G'_{m} \quad G_{0p}=0 \\
\textrm{else}     \quad G_{0m}=G'_{m}-G'_{p} \quad G_{0m}=0 \\
\end{cases}
$$

# Amplificatore Differenziale
---
![[Pasted image 20221206090035.png]]

Questo amplificatore fornisce una tensione d'uscita **proporzionale alla differnza tra le due tensioni dei generatori riferite a massa**.
Per il teorema di sovrapposizione degli effetti abbiamo un primo contributo dovuto a $v^-$ (amplificatore invertente) pari a $- \frac{R_{a}}{R_{b}}v^-$
Il secondo effetto dovuto a $v^+$ è pari a $\frac{R_{a}}{R_{a}+R_{b}}\left( 1+ \frac{R_{a}}{R_{b}}v^+ \right)$
La somma delle due ci da le caratteristiche dell'amplificatore differnziale:
$$
v_{out}=\frac{R_{a}}{R_{b}}(v^+-v^-) \quad R_{in(v^+)}=R_{a}+R_{b} \quad R_{in(v^-)}=R_{b} \quad R_{out}=0
$$
### Modo comune e modo differenziale

Quando parliamo di circuiti differenziali possiamo sempre scomporre il segnale nella **componente differenziale** e **componente di modo comune**:
$$
\begin{cases}
v_{d}=v^+-v^- \\
v_{cm}=\frac{v^++v^-}{2}
\end{cases}
\implies
\begin{cases}
v^+=v_{cm}+ \frac{v_{d}}{2} \\
v^-
=v_{cm}-\frac{v_{d}}{2} \\
\end{cases}
$$
Nel nostro caso sopra l'amplificazione di modo comune è nulla dunque l'amplificatore si comporta in modo ideale.

Nel caso di resistenze non perfettamente identiche (affette da una tolleranza $\delta$) ho un errore nel caso peggiore pari a $4\delta=\epsilon$ Devo dunque  sviluppare i calcoli e ottengo che la tensione di uscita **dipende anche dal modo comune** e dall'errore.
Possono definire il **CMRR Common Mode Rejection Rate** come il rapporto:
$$
CMRR=|\frac{A_{d}}{A_{cm}}|
$$
### Disturbi elettromagnetici

![[Pasted image 20221206093301.png]]

Nel caso di collegamento tra un sensore e un'amplificatore è possibile che per una lunghezza elevata del cavo ci sia la presenza di **disturbi elettromagnetici** (rappresentato da un generatore di tensione di disturbo $v_{dl}$) che vengono portati all'ingresso.
Inoltre la sorgente originale del segnale è riferito a una massa mentre l'amplificatore sarà riferito a un'altra massa che non è necessariamente uguale.
Dunque all'ingresso dell'amplificatore ho:
$$
v_{in}=v_{s}+v_{dl}+v_{g}
$$
Allora per ovviare al problema posso ricorrere al seguente schema:

![[Pasted image 20221206093813.png]]

Ogni sorgente ha una quantità $\frac{v_{s}}{2}$ e utilizzo un' amplificatore **differenziale** per componsare fra loro le interferenze:
$$
\begin{cases}
v^+=\frac{v_{s_{1}}}{2} +v_{dl}+v_{g} \\
v^-=-\frac{v_{s_{2}}}{2} +v_{dl}+v_{g}
\end{cases}
\implies v_{d}= v_{s}
$$
Questo è valido solo nel caso di differnziale ideale, altrimenti se ho del modo comune amplificherò anche i disturbi.


# Filtri con operazionali
---

	![[Pasted image 20221206094539.png]]

Andando nel dominio delle frequenze possiamo notare che il guadagno di questo amplificatore è pari al rapporto fra le impedenze $\frac{Z_{2}}{Z_{1}}$ che sarà una funzione di $\omega$
