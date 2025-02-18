# Classificazione segnali a tempo discreto
---
In generale noi consideriamo un segnale a tempo discreto definito secondo una variabile indipendente $n$ che assume solo valori interi.
Abbiamo dunque espressione del tipo $x_{n}, x[n], x(n)$, e chiamiam quest'ultimi "numerici" oppure "digitali" solo se il segnale assume dei valori discreti.

Una **sequenza** si può classificare secondo la sua durata:
- **durata finita** se è identicamente nullo al di fuori di un intervallo finito di tempo $[n_{1},n_{2}]$
- **durata infinita** se perdura in un intervallo infinito monolatero o bilatero

Definiamo il **supporto temporale** di una sequenza a durata infinita in $[n_{1},n_{2}]$ come:
$$
N=n_{2}-n_{1}-1
$$
### Causalità

Una sequenza è detta **causale** se è identicamente nulla per valori di $n<0$ e **anticausale** nel caso opposto.

![[Pasted image 20221122174014.png]]

# Delta di Kronecker
---
Nel mondo discreto è possibile definire il corrispettivo della delta di dirac nel continuo come:
$$
\delta(n)=\begin{cases}
0 & se \space n \neq 0 \\
1 & se \space n =0
\end{cases}
$$
Una proprietà fondamentale dei segnali a tempo discreto è che è possibile scrivere ogni segnale $x(n)$ come somma di impulsi secondo la relazione:
$$
x(n)=\sum_{i=-\infty}^{\infty} x(i)\delta(n-i)
$$
Dove $\delta(n-i)$ rappresenta la delta di Kronecker.

# Classificazione delle sequenze
---
### Sync
Definita come: 
$$
sync\left( \frac{n}{N} \right)=\frac{\sin\left( \frac{\pi n}{N} \right)}{\frac{\pi n}{N}}
$$
Notiamo che è la stesa definizione per il caso continuo, e notiamo anche che nel caso di $sync(n)$ corrisponde alla delta di Kronencker.

![[Pasted image 20221124101619.png]]

### Triangolo
Definitito come nel caso reale:


### Esponenziale
Definita come:
$$
x(n)=a^n u(n)
$$
Dove $a$ in generale è un numero complesso, se esso è minore di 0 allora si ottiene una sequenza a segni alterni, invece è a segno costante se è poisitivo.

![[Pasted image 20221124101428.png]]

Nel caso di $a$ complesso possiamo scriverlo come $a=Ae^{j\theta}$ 


### Sinosoide ed esponenziali a tempo discreto
Definiti come nel caso continuo:
$$
x(n)=A\cos(2\pi f_{0}n+\theta)=A\cos(\omega_{0}n+\theta), \space \forall n
$$
$$
x(n)=Ae^{j(2\pi f_{0}n+\theta)}
$$
Dove $A,\theta$ sono costanti reali, $f_{0}$ è la frequenza numerica, $\omega_{0}$ è la pulsazione numerica.
Vale ovviamente la **relazione di eulero**.

Una prima **proprietà** che possiamo notare con i grafici è che sostituendo $f_{0}$ con $f_{0}+k$ per qualunque $k$ intero otteniamo sempre la stessa sinusoide. (valido anche per esponenziali)
Ciò implica che:
- Nel dominio del tempo le sinusoidi che differiscono per un numero intero di angoli giro sono indistinguibili
- Quando si analizza nel dominio della frequenza ci limitiamo a frequenze di supporto unitario come $\left[ -\frac{1}{2}, \frac{1}{2} \right]$, oppure $(0,1]$ che corrispondono a intervalli di pulsazione $(-\pi,\pi]$ e $[0,2\pi)$

La **seconda proprietà** dice che più si aumenta la frequenza e più le sinusoidi analogiche oscillano frequentemente, nel caso discreto ciò è valido fino a $f_{0}=0.5$ dopodichè va a diminuire fino ad annullarsi a $f_{0}=1$

La **terza proprietà** ci dice che le sinusoidi discrete **non sono necessariamente periodiche**, questo perchè ci rifacciamo alla definizione di periodicità nel caso discreto.
In particolare sono periodiche solamente se:
- $f_{0}$ è un numero razionale, per cui esiste un $n$ che soddisfi al relazione di periodicità


# Energia e potenza media
---
La definizione di **energia per segnali a tempo discreto** è 
$$
E_{x}=\sum_{n=-\infty}^{\infty} |x(n)|^2
$$
E come nel caso analogico se questa quantità è finita allora si parla di **segnale a energia finita**.
Da ciò derivano due proprietà per questo tipo di segnali:
- l'enegia è reale, finito e positivo
- l'energia non dipende da traslazioni temporali

La **potenzia media di segnale a tempo discreto** la si definisce come:
$$
P_{x}=\lim_{ N \to \infty } \frac{1}{2N+1} \sum_{n=-N}^{N} |x(n)|^2 
$$
Vale che per un segnale a energia finita si ha potenza media nulla, mentre per segnale a potenza media finita vale che essi hanno energia infinita.

La **potenza media per segnali periodici a tempo discreto** è definita come:
$$
P_{x}=\frac{1}{N}\sum_{n=0}^{N-1}|x(n)|^2
$$
### Segnali analogici campionati

Quando cerchiamo di calcolare energia e potenza si segnali a partire da un campionamento occorre tenere conto del tempo di campionamento $T_{c}$ quindi possiamo approssimare gli integrali nel caso continuo a sommatorie nel discreto inserendo il termine del campionamento.
$$
E_{x}=\int_{-\infty}^{\infty} |x(t)|^2 \, dt  \approx T_{c}\sum_{-\infty}^{\infty}|x(nT_{c})|^2
$$
$$
\textrm{Per segnali periodici} \quad  P_{x}=\frac{1}{T}\int _{0}^T |x(t)|^2\, dt  \approx \frac{1}{N} \sum_{n=0}^{N-1}x(nT_{c})|^2
$$
$$
\textrm{Per qualunque segnale} \quad P_{x}=\lim_{ T \to \infty } \frac{1}{2T}\int _{{-T}}^T |x(t)|^2\, dt \approx \lim_{ N \to \infty } \frac{1}{2N+1} \sum_{{n=-N}}^{N} |x(nT_{c})|^2  
$$

# Operazioni elementari
---
Somma, prodotto e differenza si effettuano normalmente tra coppie di campioni osservati nei medesimi istanti di tempo.

Il **ribaltamento** consiste nel mettere $x(n-N)$ e ciò rappresenta il segnale traslato di $N$ campioni.
Se N positivo è ritardo, se negativo è anticipo.

Il **ribaltamento** consiste nel $x(n)=x(-n)$ e corrisponde al ribaltamento rispetto all'asse dei tempi.

Lo **scalamento temporale** corrisponde a un sotto-sovra campionamento.
ad esempio $x(D \cdot n)$ corrisponde a prendere un campione ogni $D$, quindi è sottocampionamento.

Il **sovracampionamento** lo si definisce come:
$$
y(n)=
\begin{cases}
& x\left( \frac{n}{I} \right) \, \forall n=0,\pm I,\pm 2I\dots \\
& 0 ,\quad \textrm{otherwise} \quad  
\end{cases}
$$
E ciò equivale a inserire $I-1$ zeri fra i campioni del segnale originale $x(n)$.

![[Pasted image 20221124112028.png]]

### Convoluzione

La **convoluzione lineare** fra due sequenze discrete è definita come:
$$
q(n)=x(n)*y(n)=\sum_{k=-\infty}^{\infty}x(k)y(n-k)
$$
Se le due sequenze sono temporalmente disgiunte allora la convoluzione è pari a $0$.
Proprietà:
- Il supporto della convoluzione è pari alla somma dei singoli supporti meno $1$.
- Gode di proprietà commutativa
- distributiva
- associativa

### Correlazione

Per due sequenze a energia finita definiamo la **correlazione** fra $x(n)$ e $y(n)$ come:
$$
R_{x,y}(n)=\sum_{k=-\infty}^{\infty}x^*(k+n)y(k)
$$
Nel caso di processi reali si deduce che:
$$
R_{x,y}(n)=R_{y,x}(-n)
$$

La **funzione di autocorrelazione** è definita nel caso di $R_{x,x}(n)$ e vale la relazione:
$$
R_{x}(0)=E_{x}
$$

