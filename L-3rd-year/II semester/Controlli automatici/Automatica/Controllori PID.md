# Legge di controllo
---
Il comando $u$ è generato in base al contributo di **tre termini** che si sommano fra loro:
- Un primo termine $P$ proporzionale all'errore di inseguimento
- Un termine $I$ proporzionale all'integrale dell'errore
- Un termine $D$ proporzionale alla derivata dell'errore
$$
u(t)=K_{P} \cdot e(t)+K_{I} \int_{t_{0}}^t e(\tau) \, d\tau + K_{D} \frac{ \partial e(t) }{ \partial t } 
$$
Le tre constanti assumono valori positivi o nulli, supponendo che il gaudagno del sistema sia positvo.

La **fdt dei controllori PID** è nella forma ( raccogliendo $K_{P}$ ):
$$
R_{PID}(s)=K_{P} (1+ \frac{1}{T_{I}s} + T_{D} s)
$$
Con $T_{I}=K_{P}/L_{I}$ e $T_{D}=K_{D}/K_{P}$
Così facendo abbiamo una fdt impropria, e per ovviare a questo inseriamo un blocco derivativo, introducendo il parametro $N$.

Dobbiamo assicurarci che il **polo di chiusura sia esterno alla banda passante**:
$$
p_{c}=-\frac{N}{T_{D}}
$$
Facciamo però attenzione al fatto che l'attività sul comando aumenta all'aumentare di $N$.

![[Pasted image 20230530181116.png]]


# Taratura in anello chiuso
---
Questi metodi sono applicabile solamente dove il margine di guadagno risulti finito!

### Ziegler Nichols

Questo metodo in anello chiuso fornisce i valori dei parametri PID in base a misure effettuate sul sistema chiuso in retroazione unitaria, inserendo un controllore statico.

Quindi scegliamo **il giadagno $\overline{K_{P}}$ pari al margine di guadagno del sistema**:
$$
\overline{K}_{P}=m_{G}
$$
La **pulsazione alla quale leggiamo il valore del margine di guadagno è detta $\omega_{\pi}$**: il parametro sul periodo dell'oscillazione si può calcolare come
$$
\overline{T}=\frac{2\pi}{\omega_{\pi}}
$$
Una volta determinati questi primi parametri approssimati si ha:

![[Pasted image 20230530181647.png]]

Questo metodo fornisce un **margine di fase inferiore a 40 gradi** quindi la risposta al gradino presenterà oscillazioni poco smorzate.

Se risulta accettabile che la pulsazione di crossover sia pari a $\omega_{\pi}$ allora si ottengono le seguenti relazioni:

![[Pasted image 20230530182022.png]]


# Taratura in anello aperto
---
Sono basato su un modello approssimato del sistema da controllare, in particolare per Ziegler Nichols in anello aperto si ha:
$$
F(s)=\frac{K_{F}}{1+\tau_{F}s} \cdot e^{-\theta_{F}s}
$$
Questi metodi sono applicabile solo per sistemi **che in catena aperta risultino asintoticamenti stabili**, e tradotto significa che la risposta al gradino deve risultare monotona.

### Metodo della tangente

Grazie a questo metodo determiniamo i parametri tipici:

![[Pasted image 20230530182419.png]]

Notiamo che il punto di ordinata corrispondente all'ascissa desiderata è:
$$
y(\tau_{F}+\theta_{F})=0.63 \cdot y_{\infty}
$$
Posso poi calcolare i parametri tipici del controllore PID:
Vale sempre l'approssimazione $T_{I}=4T_{D}$

![[Pasted image 20230530182608.png]]

### Metodo Cohen Coon

![[Pasted image 20230530182730.png]]

### Metodo IMC

![[Pasted image 20230530182941.png]]

# Esempio di taratura
---
![[Pasted image 20230530151615.png]]

La funzione di trasferimento nella realtà non è nota.
$$
G(s)=\frac{8(1+0.5s)}{(1+2s^2)^2(1+0.125s)^2}
$$
### Taratura in anello chiuso

Per applicare questi metodi devo avere **margine di guadagno finito**.
Devo verificare quindi il margine di guadagno.

```Matlab
G= (8*(1+0.5*s))/...;
[Gm,Pm,Wcg,Wcp] = margin(G)
```

Dal valore di Gm ricaviamo che esso vale:
$$
\overline{K_{p}}=G_{m}=11.8763
$$
Inoltre ricavo $\overline{T},T_{D}$ che corrisponde a:
$$
\overline{T}= \frac{2\pi}{\omega_{cg}} = 0.9003
$$
$$
T_{D}=0.125\cdot  \overline{T}
$$
Inoltre ho $N=10$ quindi possiamo scrivere la funzione di trasferimento del regolatore:
$$
R_{PID}=K_{P}\left( 1+ \frac{1}{T_{I}s}+ \frac{T_{D}s}{1+ \frac{T_{D}}{N}s} \right)
$$
