	# Sistema in Traslazione 
---
### Elementi base

Si trattano solo traslazione lungo piani ideali e orizzontali, i corpi saranno considerati **puntiformi**.
![[Pasted image 20230303090939.png]]
Dato un corpo di massa $M$ trascinato da una forza orizzontale $F$, esso rispetta la seconda legge di Newton:$$
M\frac{ \partial^2 p(t) }{ \partial t^2 }=\sum_{i}F_{i}(t) 
$$Normalmente più corpi puntiformi sono collegati fra loro tramite **molle**:
![[Pasted image 20230303091004.png]]
Una **molla ideale** è caratterizzata da una **costante elastica** e segue l'equazione:$$
F(t)=K[p_{+}(t)-p_{-}(t)]
$$
Un discorso simile si può fare per gli **smorzatori ideali**, la cui variabile di interesse è l'attrito viscoso:
![[Pasted image 20230303091018.png]]
Questo elemento segue la legge:$$
F(t)=\beta[v_{+}(t)-v_{-}(t)]=\beta[\dot{p}_{+}(t)-\dot{p}_{-}(t)]
$$ed è dunque proporzionale alla velocità relatica dei due elementi che compongono lo smorzatore stesso.

### Equazioni del moto

Per scrivere le equazioni del moto si seguono i seguenti passi:
1. Si introducono gli assi di riferimento **concordi fra loro**
2. Per ogni massa $M_{i}$, con posizione $p_{i}$ e velocità $v_{i}=\dot{p}_{i}$  si scrive la seconda legge di Newton espressa come: $$
M_{i}\ddot{p}_{i}(t)=\sum_{k}F_{k}^{est}(t)-\sum_{j}^{j \neq i}F_{ij}^{int}(t)
$$Le forze interne contengono informazioni riguardo **l'interazione dei diversi corpi $M$** e dunque riguardano molle, smorzatori:$$
\begin{cases}
\textrm{molle} \quad F_{ij}^{int}(t)=K_{ij}[p_{i}(t)-p_{j}(t)] \\
\textrm{smorzatore} \quad F_{ij}^{int}(t)=\beta_{ij}[\dot{p_{i}}(t)-\dot{p_{j}}(t)]    
\end{cases}
$$Le forze esterne **trasmettono direttamente il moto** ai corpi $M$.

### Rappresentazione di stato

Per ottenere questa rappresentazione:
1. Si scrivono le **equazione del moto** per ogni corpo puntiforme di massa $M$(eventualmente nulla), che sono dotati di velocità e posizione.
2. Si introducono le **variabili di stato** scegliendo in particolare la velocità e la posizione dei corpi.

```ad-note
Per ottenere solo equazioni differenziali del primo ordine si scelgono queste due grandezze per avere una coppia di equazioni differenziali.
```

3. Si associa una variabile **d'ingresso** a ogni forza esterna applicata al sistema meccanico.
4. Si scrivono le equazioni **di stato** del tipo $$
\dot{x}_{i}(t)=\frac{ \partial x_{i}(t) }{ \partial t } =f_{i}(t,x(t),u(t))
$$e le equazioni di uscita nella forma $$
y_{k}(t)=g(t,x(t),u(t))
$$
### Esempio

![[Pasted image 20230303090903.png]]
Scrivo le equazioni del moto per i due corpi $M$:

$M_{1}\ddot{p}_{1}=0-[K_{1}(p_{1}-0)+\beta(\dot{p_{1}}-0)+K_{12}(p_{1}-p_{2})+\beta_{12}(\dot{p_{2}}-\dot{p_{1}})]$
$M_{2}\ddot{p_{2}}=F-[K_{2}(p_{2}-0)+\beta_{2}(\dot{p_{2}}-0)+K_{12}(p_{2}-p_{1})+\beta_{12}(\dot{p_{2}}-\dot{p_{1}})]$

Individuo le varibili di stato:$$
x(t)=\begin{bmatrix}
p_{1}(t) & p_{2}(t) & \dot{p_{1}}(t) & \dot{p_{2}(t)}
\end{bmatrix}=\begin{bmatrix}
x_{1}(t) & x_{2}(t) & x_{3}(t) & x_{4}(t)
\end{bmatrix}
$$ e gli ingressi: $$
u(t)=\begin{bmatrix}
F(t)
\end{bmatrix}
$$
Le equazioni di stato sono 4 e tutti del tipo:
$$
\dot{x_{1}}=\frac{ \partial p_{1} }{ \partial t } =\dot{p_{1}}
$$
Otteniamo sostituendo:
![[Pasted image 20230303092450.png]]

Se ora supponiamo la **linearità** di $K,\beta, M\dots$ allora il sistema **lineare, LTI, dinamico, a dimensione finita (4), tempo continuo, e proprio**.

Nel caso volessi modellare l'**attrito sul piano** è possibile farlo tramite una sorta di **smorzatore equivalente**:
![[Pasted image 20230303094055.png]]
Perciò se nel caso sopra volevamo tener conto anche dell'attrito, possiamo aggiungere uno smorzatore equivalente:

```ad-note
Nel caso di smorzatori ideali in parallelo, è possibile considerarne uno solo con il $\beta$ che è la somma dei due.
```

```ad-note
Se nelle equazioni di stato non compare una certa variabile scelta in precedenza A SECONDO MEMBRO, allora essa può essere scartata, rifare i conti e ridurre la dimensione del sistema.
La variabile rimossa PUO' pur sempre apparire derivata!
```


# Sistemi in Rotazione
---
### Elementi base

Consideriamo un **corpo puntiforme** in rotazione di **inerzia $J$**.
In ogni istante il corpo possiede una **posizione angolare $\theta$** e una **velocità angolare $\omega$**.
Se sottoponiamo il corpo a una **coppia (T)** esterna allora il corpo entra in rotazione.

![[Pasted image 20230308094758.png]]

Il moto è descritto da:
$$
J\ddot{\theta}(t)=J \frac{ \partial^2 \theta(t) }{ \partial t^2 } =T(t)=\sum_{i}T_{i}
$$
La **sommatoria delle coppie** applicate è pari alla **coppia d'inerzia** $J \ddot{\theta}(t)$, ovvero il prodotto fra il **momento d'inerzia** $J$ e la derivata seconda della posizione angolare $\theta$.
E come sistema di riferimenti scegliamo il positivo nel verso **antiorario**.
Dimensionalmente la coppia ha u.d.m. di un momento: $[T]=Nm$
Mentre per l'inerzia $[J]=kg\,m^2$

I corpi in rotazione si possono collegare tramite **molle ideali**:

![[Pasted image 20230310084310.png]]

Essa esercita due coppie uguali e opposte proporzionale alla torsione della molla da entrambe le estremità:
$$
T(t)=K[\theta_{+}(t)-\theta_{-}(t)]
$$
Dove $K$ è detta **coefficiente di elasticità torsionale** $[K]=\frac{Nm}{rad}$.
Mentre i $\theta$ sono le posizioni angolari delle due estremità rispetto alle posizioni di riposo.

Un altro modo per collegare due corpi in rotazione è lo **smorzatore ideale**:

![[Pasted image 20230310084643.png]]

E' costituito da due dischi di materiali diversi che sfregano e generano attrito fra loro. La caratteristica è descritta da:
$$
T(t)=\beta[\omega_{+}(t)-\omega_{-}(t)]=\beta[\dot{\theta}_{+}(t)-\dot{\theta}_{-}(t)]
$$
Dove $\omega_{+}$ è la velocità angolare dell'estremo "positivo" e così via.
$\beta$ è lo **smorzamento**.

### Equazioni del moto

Per ottenere le equazioni del moto bisogna seguire i seguenti passi:
1. Fissare un sistema di riferimento per tutti i corpi, in modo concorde
2. Per ogni corpo **in rotazione** $J_{i}$ (anche se con momento d'inerzia nullo) si introduce la seconda legge di Newton: $$
J_{i}\ddot{\theta}(t)=\sum_{k}T_{k}^{est}(t)- \sum_{l}^{l \neq i}T_{il}^{int}(t)
$$Dove le **coppie esterne** sono quelle che tengono conto dell'azione del mondo esterno sull'elemento $J_{i}$-esimo. Le **coppie interne** tengono conto dell'interazione fra corpi in rotazione e coinvolgono molle o smorzatori.

### Rappresentazione di stato

1. Scrivo per ogni corpo in rotazione (anche con $J_{i}=0$) l'equazione del moto.
2. Si introducono le **variabili di stato** scegliendo solitamente la posizione e la velocità angolare $\omega, \theta$
3. Si associa per ogni coppie esterne una **variabile d'ingresso**.
4. Scrivo le **equazioni di stato** nella forma:$$
\dot{x}_{i}(t)=\frac{ \partial x_{i}(t) }{ \partial t } =f_{i}(r,x(t),u(t))
$$
5. Infine si ricavano le equazioni di uscita richieste.

### Esempio

Nell'esempio abbiamo ad esempio un albero motore, guidato da un cuscinetto a sfera ($\beta_{1}$) che tramite una **giunzione elastica smorzata** fa ruotare un carico generico (corpo 2, ad esempio un antenna satellitare).
La giunzione è necessaria per smorzare l'effetto elastico della molla, una volta che il moto verrà trasmesso al carico.

![[Pasted image 20230310091003.png]]

Per il corpo 1 abbiamo:

$J_{1}\ddot{\theta_{1}(t)}=T_{m}-[\beta_{1}(\dot{\theta_{1}}-0)+K_{12}(\theta_{1}-\theta_{2})+\beta_{12}(\dot{\theta_{1}}-\dot{\theta_{2}})]$

Per il 2:

$J_{2}\ddot{\theta_{2}}=-T_{r}-[K_{12}(\theta_{2}- \theta_{1})+\beta_{12}(\dot{\theta_{2}}-\dot{\theta_{1}})]$

Introduco le variabili di stato e gli ingressi:
$$
x(t)=\begin{bmatrix}
\theta_{1} \\
\theta_{2} \\
\dot{\theta}_{1} \\
\dot{\theta}_{2}
\end{bmatrix}=\begin{bmatrix}
x_{1}(t) \\
x_{2}(t) \\
x_{3}(t) \\
x_{4}(t)
\end{bmatrix} \quad u(t)=\begin{bmatrix}
T_{m}(t) \\
T_{r}(t)
\end{bmatrix}=\begin{bmatrix}
u_{1}(t) \\
u_{2}(t)
\end{bmatrix}
$$
E le equazioni di stato:

![[Pasted image 20230310092742.png]]

Tutti termini linerari, se suppongo i coefficienti costanti allora il sistema è LTI, dinamico a tempo continuo, proprio.

### Esempio 2, Pendolo inverso

![[Pasted image 20230310093936.png]]

Sotto l'ipotesi di asta rigida e di massa trascurabile $J=Ml^2$ calcolare le espressioni di $\theta$ e $\omega=\dot{\theta}$.
La forza che agisce sulla massa è scomposta nelle sue componenti orizzontali e verticali ed genera una coppia data dal prodotto esterno
Ogni componente della forza genera una coppia:
$T_{F_{o}}=l \wedge F_{o}=lF_{o}\sin\left( \frac{\pi}{2}-\theta \right)=lF_{o} \cos(\theta)$
$T_{F_{v}}=l \wedge F_{v}=lF_{v}\sin(\pi-\theta)=lF_{v}\sin(\theta)$
$T_{F_{p}}=l \wedge F_{p}=lF_{p}\sin(\pi-\theta)=lF_{p}\sin(\theta)$

Dunque l'equazione del moto è pari a:
$$
J \ddot{\theta}=Ml^2 \ddot{\theta}=lF_{o}\cos(\theta)+lF_{v}\sin(\theta)+lF_{p}\sin(\theta)-[K(\theta-0)+\beta(\dot{\theta}-0)]
$$
Ora introduco:
$$
x(t)=\begin{bmatrix}
\theta(t) \\
\dot{\theta}(t)
\end{bmatrix}=\begin{bmatrix}
x_{1}(t) \\
x_{2}(t)
\end{bmatrix}, \quad u(t)=\begin{bmatrix}
F_{o}(t) \\
F_{v}(t)
\end{bmatrix}=\begin{bmatrix}
u_{1}(t) \\
u_{2}(t)
\end{bmatrix}
$$
Scrivo stato e uscita ed eventualemente notazione matriciale abbreviata

![[Pasted image 20230310095924.png]]

Posso dire che il sistema è a tempo continuo, dinamico, dimensione finita (2), MIMO (p=2, q=2), non lineare a causa dei termini trigonometrici, inoltre è tempo invariante se M,l,g,K,$\beta$ sono costanti nel tempo.

Nonostante per angoli molto piccoli si possa approssimare $\sin\theta \sim \theta$ non è possibile elimanare il prodotto incrociato stato-ingresso $u_{2}(t) \cdot x_{1}(t)$, dunque in ogni caso è non lineare.


