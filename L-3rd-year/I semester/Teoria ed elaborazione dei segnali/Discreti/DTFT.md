# Discrete Time Fourier Transform
---
Consideriamo di porre all'ingresso di un [[Sistemi LTI -ens]] un segnale del tipo:
$$
x(n)=e^{2j\pi f_{0}n}
$$
Se consideriamo il nostro ingresso come $z_{0}$ allora otteniamo un'uscita del tipo:
$$
y(n)=e^{2\pi jf_{0}n }H(e^{2j\pi f_{0}})
$$
Questa funzione $H$ prende il nome di **risposta in frequenza** e corrisponde alla $H(z)$ calcolata in $z=e^{2j\pi f}$ e ciò corrisponde (al variare di $f$) la circonferenza di raggio unitario sul piano complesso.

Definiamo questa operazione come **DTFT** ed è definita come:
$$
X(e^{2j\pi f})=\sum_{n=-\infty}^\infty x(n) e^{-2j\pi fn}
$$
Si tratta dunque di **una funzione complessa di variabile reale $f$**.

```ad-note
La trasformata di Fourier è una particolarizzazione di quella di Laplace, in particolare Laplace è definita su tutto il piano complesso, mentre Fourier solo sull'asse immaginario.

Una cosa analoga avviene per la ZETA e la DTFT
La ZETA è definita su tutto il piano complesso mentre la DTFT solo sulla circonferenza di raggio unitario: |z|< 1
```

Al variare di $f$ oppure di $\omega=2\pi f$ andiamo a percorrere tutti i punti della circonferenza: dunque **la DTFT è periodica di periodo unitario in $f$**.
In $\omega$ è periodica di $2\pi$.

```ad-note
La DTFT della delta è pari a 1.
La DTFT della funzione costante 1 è pari alla delta(f) con f compreso fra -1/2 e 1/2
```

### Inversione della DTFT

L'espressione della DTFT può essere interpretata come lo sviluppo di Fourier della funzione periodica $X(e^{j\omega})$ dove i coefficienti della serie sono proiezioni del segnale lungo i vettori della base di funzioni esponenziali:
$$
x(k)=\frac{1}{2\pi}\int _{-\pi}^\pi X(e^{j\omega})e^{j\omega k}\, d\omega 
$$
E' possibile anche ottenere la versione dipendente dalla frequenza ricordandosi che $\omega=2\pi f \implies f=\frac{\omega}{2\pi}$

### Proprietà della DTFT

![[Pasted image 20221209160200.png]]

Molte sono derivate dalla trasformata ZETA.
# DTFT e LTI
---
### Funzione di trasferimento

La funzione $H(e^{j\omega})$ è detta **risposta in frequenza** del sistema LTI ed è pari alla trasformata DTFT della risposta all'impulso.
$$
H(e^{j\omega})=\sum_{n=-\infty}^\infty h(n)e^{-j\omega n}=DTFT[h(n)]
$$
Quest'ultima è una funzione complessa in $\omega$ e rispetta le normali proprietà di una funzione di trasferimento.
- Se all'ingresso di un LTI abbiamo un esponenziale allora in uscita avremo ancora un esponenziale moltiplicato per la DTFT dell'esponenziale stesso.

Ricordiamo che $H(e^{j\omega})$ si può scrivere come modulo elevato a fase.

### Spettro di energia

Ricordiamo al definizione di spettro di energia per un  segnale discreto:
$$
S_{x}(f)=|X(e^{2j\pi f})|^2
$$
Esso rappresenta la distribuzione dell'energia di $x(n)$ sulle componenti armoniche che lo costituiscono.
- $S_{x}(f)\geq 0$
- Se $x(n)$ è reale, allora lo spettro è pari e reale

### Relazione di Parseval
$$
E_{x}=\sum_{k=-\infty}^\infty |x(k)|^2=\int _{-\frac{1}{2}}^{\frac{1}{2}} |X(e^{2j\pi f})|^2 \, df 
$$
CI dice che l'energia di una sequenza corrisponde a quella della sua DTFT calcolata sul suo periodo.

### Banda Assoluta

La **banda assoluta** della sequence $x(n)$ si definisce come la frequenza $B_{x}$ tale per cui lo spettro è identicamente nullo al di fuori dell'intervallo $[-B_{x},B_{x}]$

### Banda equivalente

E' la frequenza $B_{eq}$ tale per cui il rettangolo con base  $[-B_{eq};B_{eq}]$ e altezza pari al massimo dello spettro di energia possiede la medesima energia del segnale:
$$
2B_{eq}|X_{M}|^2 =\int _{T} S_{x}(f) \, df=\sum_{n=-\infty}^\infty |x(n)|^2 
$$
# Convoluzione Circolare
---
Operatore compatibile con due sequenze di tempo discrete, SOLO se hanno la stessa lunghezza $N$, in caso contrario occorre inserire degli zeri per pareggiare.
Il supporto della convoluzione circolare è proprio N e cambia a seconda di quest'ultimo.
Il risultato coincide con quello della convoluzione lineare solo se $N$ è pari al supporto della convoluzione lineare stessa.

Dal punto di vista operativo la convoluzione circolari si può calcolare come il prodotto fra matrici di cui una circolante, ovvero ogni colonna shifta di una posizione in giù.