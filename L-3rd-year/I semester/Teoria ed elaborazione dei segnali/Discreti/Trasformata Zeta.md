# Trasformata Zeta
---
Supponiamo di avere in ingresso a un LTI un segnale $x(n)=z_{0}^n$, dunque se ne facciamo la convoluzione con la risposta all'impulso otteniamo all'uscita $z_{0}^nH(z_{0})$ ovvero un numero complesso (che è la funzione di trasferimento ) che moltiplica l'ingresso stesso.

Il numero complesso qui sopra è la **funzione di trasferimento** del LTI ed è pari a $$
H(z)=\sum_{k=-\infty}^\infty h(k)z^{-k}
$$In generale, data un sequenza generica $x(n)$ definiamo la funzione complessa di variabile complessa:
$$
X(z)=Z[x(n)]=\sum_{k=-\infty}^\infty x(k)z^{-k}
$$
Essa dice **trasformata zeta** di $x(n)$
$z$ è una generica variabile complessa che può essere scritta in forma esponenziale ($z=\rho \cdot e^{j\omega}$) .

Dal punto di vista del significato è un modo alternativo di vedere il segnale:
- il coefficiente $z^{-n}$ è il valore del segnale al tempo $n$
- l'esponenze di $z$ contiene l'informazione temporale per identificare i campioni di cui abbiamo bisogno.


# ROC
---
Essendo una serie è soggetta a convergenza in diversi punto del piano: il luogo dei punti dove la serie converge è detta **regione di convergenza (ROC)**, all'interno della quale è analitica, derivabile e continua.
Per trovare ROC occorre scrivere la trasformata in forma esponenziale e notare che **la serie converge in modo uniforme se l'argomento è sommabile in modulo**:
$$
\sum |x(k)\rho^{-k}| < \infty \implies \sum|x(k)|\rho^{-k} <\infty
$$
Dunque la ROC dipende solo **dalle pulsazioni complesse, non dalla fase**, perciò nel piano complesso le ROC saranno delle circonferenze!

```ad-note
Calcolo della trasformata zeta della delta —>
Z[delta]=1
```

Per capire come sono fatte le ROC occorre spezzare la trasformata nei termini causali e anticausali:
Per il termine **anticausale** abbiamo $$
\sum_{i=1}^\infty |x(-i)|\rho^i
$$
dunque è ragionevole pensare che esista un valore di $\rho$ sufficientemente piccolo tale per cui la sommatoria risulti finita, dunque la roc sarà un circonferenza di raggio $\rho_{1}$.

Il termine **causale** è pari a $$
\sum_{k=0}^\infty \frac{|x(k)|}{\rho^k}
$$
Dunque supponiamo che esisterà un $\rho$ abbastanza grande tale per cui il valore della sommatoria converge.
La ROC sarà composta dai punti all'esterno di una circonferenza di raggio $\rho_{2}$

Per le **sequenze bilatera** avremo come ROC l'intersezione della parte causale e anticausale!
E' possibile che le due circonferenze non abbiamo intersezione.

Per le sequenze a supporto finito abbiamo che la trasformata xeta è pari a:
$$
X(z)=\sum_{n=n_{1}}^{n_{2}}x(n)z^{-n}
$$
In questo caso converge in qualunque punto del piano complesso eccetto $z=0$ se esistono termini tipo $z^{-k}$ con $k>0$ e in certi casi non converge per $|z|= \infty$.

### Forma razionale della trasformata

Nella maggior parte dei casi abbiamo trasformate zeta in una forma razionale
$$
X(z)=\frac{N(z)}{D(z)}= \frac{\sum_{i}b_{i}z^{-1}}{\sum_{i}a_{i}z^{-i}}
$$
Anche esprimibile in termini di potenze positive di $z$.
Per analizzare le ROC di trasformate in questa forma occorre fattorizzare e trovare **zeri e poli**
$$
X(z)=\frac{b_{0}}{a_{0}}\frac{\prod(1-c_{i}z^{-i}) }{\prod(1-d_{i}z^{-i})}
$$
Per sequenze **causali** i poli si trovano **all'interno della circonferenza** che delimita la ROC.

Per sequenze **anticausali** otteniamo ceh i poli si trovano tutti fuori o al massimo sul bordo della circonferenza.

Il poli più vicino all'origine delimita la ROC in entrambi i casi.
Nel caso di sequenza bilatera occorre combinare il polo di modulo massimo e il polo di modulo minimo per determinare la ROC.

DI seguito una tabella con trasformate e ROC notevoli

![[Pasted image 20221206161024.png]]


# Proprietà
---
![[Pasted image 20221206162047.png]]
Inoltre è valido il teorema del **valore iniziale**, per una sequenza $x(n)$ abbiamo:
$$
x(0)=\lim_{ z \to \infty } X(z)=\lim_{ z \to \infty } [x(0)+x(1)z^{-1}+x(2)z^{-2}+\dots]
$$


# Inversione
---
Per riuscire ad antitrasformare dobbiamo ricorrere al teorema di Cauchy, è dunque possibile dimostrare che l'antitrasformata è pari a
$$
x(n)=\frac{1}{2j\pi} \int_{\gamma} X(z)z^{n-1} \, dz =Z^{-1}[X(z)]
$$
Abbstanza complessa dato che dobbiamo fare un integrale di linea. Possiamo allora ricorrere al teorema dei residui ma anche così è troppo complesso.

Per invertire in modo semplice dobbiamo ricorre a trucchi algebrici o ai **fratti semplici**.

# Sistemi LTI e Zeta-Trasnform
---
Vediamo ora come possima usare la tarsformata zeta per studiare i sistemi [[Sistemi LTI -ens]].
Ricordiamo che oltre alla convoluzione fra ingresso e risposta all'impulso possiamo andare nel dominio della trasformat zeta e ottenere:
$$
Y(z) = Z[x(n)*h(n)]= X(z) \cdot H(z)
$$
Dove la ROC sarà l'intersezione tra la ROC di  $X(z)$ e la ROC di $H(z)$.

### Filtri digitali

Sono i più comuni LTI utilizzati e sono descritti dall'equazione alla differenze, dalla quale possiamo ricavare l'espressione della funzione di trasferimento:
$$
H(z)=\frac{Y(z)}{X(z)}=\frac{b_{0}+b_{1}z^{-1}+ \dots + b_{N}z^{-N}}{1+a_{1}z^{-1}+\dots+a_{M}z^{-M}}=H_{N}(z)H_{R}(z)
$$
Dove $H_{N}(z)$ rappresenta il numeratore (o 'non ricorsivo') mentre il $H_{R}(z)$ il termine ricorsivo.

### FIR e IIR

Possiamo distinguere i filtri in:
- **FIR Finite impulse response** dove l'uscita dipende solo dal segnale in ingresso ( la sommatoria con gli $a_{k}$ + pari a $0$ ); di conseguenza la risposta all'impulso è finita e la funzione di trasferimento è pari a $$
H(z)=b_{0}+b_{1}z^{-1}+\dots+b_{N}z^{-N}
$$
- **IIR Infinite Impulse Response**, dove l'uscita dipende sia dagli ingressi che campioni in uscita del segnale (passati); compaiono entrambi i termine delle sommatorie.

### Stabilità di LTI

Per determinare la stabilità si usa il criterio **BIBO (bounded input bounded output)** già visto per i sistemi nel dominio continuo.
Ciò significa che per ogni sequenza in ingresso $x(n)$ limitata in ampiezza ottengo in uscita ancora una sequenza limitata in ampiezza:
$$
|x(n)|\leq X_{0}< \infty \quad\implies \quad |y(n)|\leq Y_{0}< \infty
$$
Per verificare ciò c'è un teorema che afferma che il criterio BIBO è verificato solo se la risposta all'impulso è **assolutamente sommabile**:
$$
\sum_{n=-\infty}^\infty |h(n)|<\infty\implies\textrm{BIBO} \quad  
$$
Nel caso di un sistema **causale** si può dimostrare che la relazione precedente si soddisfa se **tutti i poli di $H(z)$ cadono all'interno del cerchio di raggio $1$**.

Per **sistemi anticausali** per la stabilità si richiede che tutti i poli siano all'esterno della circonferenza di raggio $1$.

Per **i sistemi bilateri** si richiede che la circonferenza di raggio $1$ sia contenuta nella corona circolare, ovvero la ROC del sistema.

### Riepiloghino

![[Pasted image 20221209111506.png]]

| Causale                                                                                           | Anticausale                                                                          | Bilatera                                                                          |
| ------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------- |
| La ROC è l'**esterno** di un cerchio di raggio $R_{h}$                                            | La ROC è l'**interno** di un cerchio di raggio $R_{h}$                               | La ROC è una corona circolare con $R_{h,1}<z <R_{h,2}$                            |
| Affinchè siano stabili occorre che tutti i poli siano all'interno della circonferenza di raggio 1 | Per essere stabile i poli devono giacere all'esterno della circonferenza di raggio 1 | Per essere stabile la circonferenza di raggio 1 dev'essere contenuto nella corona |

### Realizzabilità fisica di LTI

Un sistema è fisicamente realizzabile se è causale e se la risposta all'impulso ha coefficienti reali!
- i coefficienti sono tutti reali se e solo se per ogni polo o zero è presente il rispettivo complesso coniugato. (condizioni rispettata da polinomi con coefficienti reali).


# Sistemi inversi
---
Un sistema inverso è progettato per invertire l'uscita di qualche sistema precedente; esso è caratterizzato da:
$$
H_{I}(z)=H^{-1}(z)= \frac{1}{H(z)}
$$
Inoltre se $H_{I}(z)$ è **causale** la convergenza sarà del tipo $|z|>\max(c_{i})$ (in quanto tutti i poli diventano zeri e viceversa).

Un sistema **a fase minima** ha tutti i poli e gli zeri all'interno del cerchio di raggio unitario.
Essi garantiscono la minor distorsione di fase possibile.