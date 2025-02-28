#  Nozione di sistemi
---
Per **sistema** si intende un ente (fisico o astratto) dato dalla connessione di diverse parti elementari. per cui vale il principio di azione e reazione.
Chiamiamo **ingresso (azione, causa)** il simbolo $u(\cdot)$, e l'**uscita (reazione, effetto)** $y(\cdot)$.

```ad-note
Con $y(\cdot)$ intendiamo solo le uscite del sistema che ci interessa conoscere
```

Il sistema $S$ è rappresentato con una scatola, inteso sia come il sistema in sè o la sua rappresentazione matematica.

I problemi tipici su un sistema sono:
- **Previsione**, trovare le uscite $y(\cdot)$ conoscendo $S,u(\cdot)$
- **Controllo**, trovare l'ingresso $u(\cdot)$, noto $S, y(\cdot)_{des}$ ovvero le uscite desiderate.
- **Identificazione**, trovare $S$, noti $u(\cdot),y(\cdot)$

### Sistema statico

Si intende un sistema in cui il legame ingresso-uscita è istantaneo:
$$
y(t)=g(u(t)), \space \forall t
$$
Dunque il valore dell'uscita al tempo $t$ è dipendente solo dall'ingresso al tempo $t$. Un esempio è il **resistore ideale**

### Sistema dinamico

Si intende un sistema in cui il legame ingresso-uscita è del tipo:$$
y(t)=g(u]-\infty,t]), \space \forall t
$$
Un esempio è dato dal **condensatore ideale**.
Per studiare la "storia" di un sistema dinamico abbiamo introdotto una **variabile di stato** $x(\tau)$, che racchiude la storia passata del sistema:$$
y(t)=g(x(\tau), u[\tau,t]), \space \forall t \geq \tau
$$
Per risolvere sistemi dinamici occorre sostanzialmente risolvere un'equazione differenziale, dunque necessitiamo di condizioni iniziali che corrisponde nella pratica al fissare una variabile di stato.


### Definizione assiomatica di un sistema dinamico

Il sistema dinamico è un ente definito dai seguenti elementi:$$
S(T,U,\Omega,X,Y,\Gamma,\phi, \eta)
$$
Dove
- $T$ rappresenta l'**insieme ordinato dei tempi**
- $U$ indica l'insieme dei valori assunti dall'ingresso
- $\Omega$ è l'insieme delle funzioni possibili in ingresso
- $X$ è l'insieme dei possibili valori assunti dallo stato
- $Y$ è l'insieme delle possibili valori d'uscita
- $\Gamma$ è l'insieme delle possibili funzioni d'uscita
Per questi insieme se sono definite le funzioni $\phi, \eta$, allora otteniamo una **rappresentazione di stato**.

La funzione $\phi$ è detta **transizione dello stato** ed è esprimibile come:$$
x(t)=\phi(t,\tau,x(\tau),u(\cdot))
$$
Questa funzione gode della proprietà di **causalità**.
La **funzione di uscita $\eta$** è descritta da:$$
y(t)=\eta(t,x(t),u(t))
$$E' da notare che la relazione è istantanea, ed è detto **non fisicamente realizzabile, o improprio**, perchè l'ingresso si riverbera subito sull'uscita e ciò fisicamente non è vero.
Un sistema **proprio, fisicamente realizzabile** è descritto da $$
y(t)=\eta(t,x(t))
$$dove non compare la relazione con l'ingresso istantaneo.

# Classificazione dei sistemi
---
Una prima distinzione è fatta sulla base dell'evoluzione temporale:
- Sistema dinamico a **tempo continuo**.
- Sistema dinamico a **tempo discreto**.

A seconda dei valori di ingresso e uscita $U,Y$ definiamo:
- Sistemi dinamici **a ingressi e uscite quantizzati**
- Sistemi dinamici **a ingressi e uscite reali**
Possiamo parlare di sistemi **SISO, MIMO**

A seconda della cardinalità dell'insieme $X$:
- Se è discreto, abbiamo **sistemi a stati finiti**
- Nel caso di $X \subseteq \mathcal{R}^n$ , con $n$ finito, abbiamo **sistemi a dimensione finita (parametri concentrati)**, descritto da funzioni differenziali alle derivate ordinarie
- Se $n$ è infinito abbiamo **sistemi a dimensione infinita, o a parametri distribuiti**, descritte da eq. differenziali alle derivate parziali.

### Linearità

Un sistema è **lineare** se
- $U,\Omega,X,Y$ sono spazi vettoriali
- $\phi$ è lineare in $x,u$, ovvero $\phi$ è scomponibile in due termini, uno che dipende da x e uno da u
- $\eta$ è lineare in $x,u$, ovvero similmente a sopra
In caso una di queste non sia rispettata abbiamo un sistema **non lineare**.

### Stazionarietà

Un sistema è detto **stazionario** se $\phi,\eta$ non dipendono esplicitamente dal tempo $$
\phi(t,\tau,x^*,u(\cdot))=\phi(t+\Delta \tau, \tau+\Delta \tau,x^*, u^{\Delta \tau}(\cdot))
$$ $$
y(t)=\eta(x(t),u(t))
$$Se non rispetto queste due condizioni il sistema è **tempo-variante, o non stazionario**.

# Casi particolari
---
Un primo tipo è il **sistema dinamico a tempo continuo, a dimensione finita**, in cui la variabile $x(t)$ è soluzione di $$
\dot{x}(t)=\frac{ \partial x(t) }{ \partial t } =f(t,x(t),u(t))
$$Mentre l'equazione di uscita è sempre del tipo $$
y(t)=g(t,x(t),u(t))
$$ 
### T continuo, lineare

Se aggiungiamo l'ipotesi di **linearità** al caso precedente abbiamo:$$
\dot{x}(t)=A(t)x(t)+B(t)u(t)
$$ dove $A(t),B(t)$ sono rispettivamente la **matrice di stato e degli ingressi**, rispettivamente di dimensioni $n$x$n$ e $n$x$p$ 
L'uscita è data da:$$
y(t)=C(t)x(t)+D(t)u(t)
$$dove $C(t),D(t)$ sono le matrici delle uscite, e del legame diretto ingresso-uscita.

### LTI

In questo caso le matrici non dipendono dal tempo, risulta dunque:
$$
\dot{x}(t)=Ax(t)+Bu(t)
$$$$
y(t)=Cx(t)+Du(t)
$$
### Dinamico, t discreto

Ricorro alle equazioni alle differenze:$$
x(k+1)=f(k,x(k),u(k))
$$dove posso vedere il valore all'istante discreto successivo.
L'uscita è esprimibile come:$$
y(k)=g(k,x(k),u(k))
$$quindi a parte la lettera $k$ la differenza fra tempo continuo e discreto sono quasi nulle.

### T discreto, lineare

$$
\begin{cases}
x(k+1)=A(k)x(k)+B(k)u(k) \\
y(k)=C(k)x(k)+D(k)+u(k)
\end{cases}
$$

### LTI a T discreto

Come sopra ma senza le matrici che dipendono dal tempo.

# Esempio
---
Per classificare se è **dinamico** basta notare la presenza di una varibile di stato.

Per verificare se è a t **continuo** basta vedere che le derivate sono rispetto al tempo, e non equazioni alle differenze.

Per la **dimensione** del sistema conto le variabili di stato.

Per la **linearità** guardo il secondo membro dell'eq. di stato e dell'uscita e vedere se sono combinazioni lineari di $x,u$.

Per vedere se è **LTI** occorre vedere se la variabile temporale $t$ compare esplicitamente nelle variabili di stato e nell'uscita: se non compare allora è LTI.

Per vedere se **fisicamente realizzabile** devo vedere se $y$ dipende ESPLICITAMENTE da $u(t)$.

