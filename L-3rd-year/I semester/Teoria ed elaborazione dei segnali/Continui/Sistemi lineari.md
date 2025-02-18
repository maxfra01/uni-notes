# Classificazione dei sistemi
Per questo capitolo occorre ricordare le proprietà della trasformata di fourier [[Serie e trasformata di Fourier]].

Un **Sistema** è un elemento che dato un segnale in ingresso lo trasforma in un segnale differente tramite una **trasformazione**.
$$
\mathcal{T}:x_{1}(t) \implies y_{1}(t)
$$
Segue la classificazione dei sistemi.

### Sistemi Lineari
Sistemi per cui vale la proprietà di **sovrapposizione degli effetti**
$$
\mathcal{T}[ax_{1} +bx_{2}]=a \mathcal{T}[x_{1}]+b\mathcal{T}[x_{2}]
$$
![[Pasted image 20221013120810.png]]

### Sistemi Tempo Invarianti
Sistema in cui un ritardo sugli ingressi si traduce in un ritardo sulle uscite.
$$
\mathcal{T}[x(t)]=y(t) \leftrightarrow \mathcal{T}[x(t-\theta)]=y(t-\theta)
$$
![[Pasted image 20221013121025.png]]

Nella maggior parte del corso tratteremo **LTI (Linear and Time Invariant)**.

### Sistemi senza memoria
Sistemi in cui l'uscita dipende solo dal valore in ingresso in quel momento.
(parallelismo con sistemi combinatori (vedi Calcolatori elettronici)).

Se un sistema è senza memoria e tempo invariante allora si può descrivere il sistema con una **relazione ingresso-uscita**.



# Sistemi LTI
---
### Risposta all'impulso
In questi sistemi assume particolare importanza la **risposta all'impulso**: ovvero quando osservo l'uscita del sistema quando applico una **delta di Dirac**.
$$
h(t)=\mathcal{L}(\delta(t))
$$
![[Pasted image 20221013121508.png]]

Si dimostra che grazie alla risposta all'impulso (indicata con $h(t)$) è possibile calcolare la risposta di ogni ingresso per un sistema LTI.
$$
y(t)=x(t)*h(t)=\int x(u)h(t-u) \, du 
$$
Tramite un integrale di convoluzione fra l'ingresso e la risposta all'impulso.
Vale anche traslare l'ingresso e non la risposta all'impulso: $\int h(u)x(t-u) \, du$
Useremo Fourier per non calcolare mai integrali di convoluzione.

### Funzione di trasferimento
Si dice **Funzione di Trasferimento** la trasformata di Fourier della risposta all'impulso:
$$
H(f)=\mathcal{F}[\mathcal{L}(\delta(t))]
$$
Spesso è anche detta **risposta in frequenza** di un sistema lineare.
Da ciò si possono dimostrare le seguenti relazioni:
$$
Y(f)=H(f)X(f) \quad \quad \quad H(f)=\frac{Y(f)}{X(f)}
$$
Dunque possiamo caratterizzare un LTI tramite **la risposta all'impulso** oppure **la funzione di trasferimento**.

### Ingressi in LTI di tipo esponenziale complesso
Se poniamo all'ingresso di un sistema LTI una sinusoide complessa, otteniamo:
$$
x(t)=e^{2j\pi f_{0}t} \implies X(f)=\delta(f-f_{0})
$$
Se usiamo la funzione di trasferimento per ottenere l'uscita abbiamo:
$$
Y(f)=H(f) \cdot \delta(f-f_{0}) \implies y(t)=x(t) \cdot H(f_{0})
$$
Dunque per questo particolare tipo di ingresso la risposta **dipende solamente dalla funzione di trasferimento calcolata in $f_{0}$.**

### Ingressi in LTI di tipo sinusoidale
Poniamo all'ingresso $x(t)=\cos(2\pi f_{0}t)$ con TdF $X(f)=\frac{1}{2}[\delta(f-f_{0}) +\delta(f+f_{0})]$
Otteniamo, moltiplicando per la funzione di trasferimento:
$$
Y(f)=\frac{1}{2}[H(f_{0})\delta(f-f_{0})+H(-f_{0})\delta(f+f_{0})]
$$
Se scriviamo i moduli e fasi nel dominio del tempo allora otteniamo un importante risultato: (calcoli su slide)
**Un sistema LTI reale trasforma un segnale sinusoidale in ingresso in un segnale sinusoidale in uscita alla stessa frequenza.**

![[Pasted image 20221021103246.png]]

Invece modulo e fase sono modificabili da un sistema LTI, inoltre dipendono solamente dal valore di $H(f_{0})$



# Sistemi LTI reali
---

### Principio di causalità dei sistemi LTI
Consideriamo la convoluzione di segnali:
$$
y(t)=\int_{-\infty}^{\infty} x(t-\theta)h(\theta) \, d\theta 
$$
Lo spezziamo in due parti:
$$
y(t)=\int_{-\infty}^{0} x(t-\theta)h(\theta) \, d\theta + \int_{0}^{\infty} x(t-\theta)h(\theta) \, d\theta   
$$
Dato che abbiamo operato la traslazione la prima parte della somma dipende dai valori di $t$ in istanti di tempo successivi (non intuitivo, l'ingresso dipenderebbe da istanti che non posso ancora conoscere), mentre la seconda parte dipende dagli istanti di tempo precedenti a $t$.

![[Pasted image 20221021104413.png]]

Non è realistico in un sistema fisico reale pensare questa dipendenza dagli "istanti fututi", e per evitare ciò dobbiamo considereare **risposte all'impuslo che valgono $0$ per tempi $t<0$**
$$
h(t)=0 \quad \forall t<0
$$

### Condizione di parità della funzione di trasferimento
- $h(t)$ dev'essere reale
- $H(f)$ deve avere parte reale pari
- $H(f)$ ha parte immaginaria dispari
- $H(f)$ ha modulo pari
- $H(f)$ fase dispari

### Fisica realizzabilità
Per un sistema reale ad un ingresso reale corrisponde uscita reale quindi le due condizioni per la realizzabilità sono:
- risposta all'impulso reale
- risposta all'impuslo nulla per tempi negativi

### BIBO
Ad un ingresso limitato in ampiezza deve corrispondere un'uscita limitata
$$
\forall x(t) : |x(t)| <\infty \, , \forall t \implies |y(t)|<\infty ,\forall t
$$
![[Pasted image 20221021105221.png]]

Si dimostra che affinchè un sistema sia stabile:
$$
\textrm{stabile} \quad  \leftrightarrow \int |h(t)| \, dt <\infty \implies|H(f)|<\infty \space \forall f 
$$

# Filtri realizzabili
---
Un filtro è un sistema lineare LTI , progettato per ottenere specifici comportamenti di risposta in frequenza.
Tipologie di filtri:
- **LPF, Filtro Passa Basso**, ha un $H(f)$ concentrata intorno alle basse frequenze
- **Filtro Passa Banda**, che seleziona un certo intervallo di frequenze (non include l'origine)
- **Filtro Passa Alto**, che selezionano solo le alte frequenze
- **Elimina Banda**, complementare del passa banda

![[Pasted image 20221021114209.png]]

Nella realtà un filtro IDEALE passa basso dovrebbe avere un supporto strettamente limitato in frequenza:

![[Pasted image 20221021114726.png]]

Ma dato il supporto limitato nel dominio della frequenza, nel dominio del tempo avremo un supporto illimitato, inoltre la risposta all'impulso non è causale, dunque nella realtà possiamo solo costruire filtri approssimati.

La risposta all'impulso di un filtro passa basso ideale è una $sync$, per approssimarla la tronchiamo nel dominio del tempo.

![[Pasted image 20221021115124.png]]

Con questo troncamento otterremo una funzione di trasferimento del tipo:

![[Pasted image 20221021115202.png]]


