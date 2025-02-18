# Introduzione
---
La variabile temporale assumerà solo valori interi e discreti, a istanti campionati.
Useremo la variabile temporale $k$, e inoltre possiamo caratterizzare le equazioni di stato e uscita come equazioni alle differenze:
$$
x_{i}(k+1)=f_{i}(k,x(k),u(k)) \quad \quad y_{l}(k)=g_{l}(k,x(k),u(k))
$$
Esistono due grandi insieme di sistemi a tempo discreto: sistemi a classe d'età e sistemi dinamici a dati campionati.


# Sistemi a classe d'età
---
Sono caratterizzati da diversi stati ed ognuno di essi rappresenta la numerosità di una particolare classe di popolazione.
Il termine $x_{i}(k)$ rappresenta la numerosità della popolazione della classe $i$ nel periodo considerato $k$.
Per tenere conto dell'evoluzione della popolazione facciamo riferimento al **tasso di sopravvivenza e mortalità**.

### Esempio 1, il parco macchine

$x_{i}(k)$ rappresenta il numero di macchine nel parco, durante l'anno $k$, ed aventi età $i$:
$$
(i-1)\leq età(x_{i}(k)) <i
$$
$u(k)$ rappresenta il numero di macchine nuove che vengono acquistate nell'anno $k$.
L'ingresso è uno solo, in quanto non compro auto usate.
Inoltre non vogliamo conservare auto più vecchie di 3 anni.

Dobbiamo allora introdurre 3 classi: una per ogni anno di età delle macchine:
le macchine che l'anno prossimo avranno un anno di età sono descritte dall'equazione:
$$
x_{1}(k+1)=u(k)
$$
Per quelle aventi due anni:
$$
x_{2}(k+1)=x_{1}(k)- \gamma_{1}x_{1}(k)
$$
Ovvero tutte le macchine che hanno un anno ad oggi e un contributo negativo che tiene conto delle auto che si rompono ($\gamma_{1}$ è una sorta di "tasso di mortalità")
Per il terzo anno:
$$
x_{3}(k+1)=x_{2}(k)-\gamma_{2}x_{2}(k)
$$

Volendo conoscere numero totale di auto e costi di asscurazione abbiamo:
$$
y_{1}(k)=x_{1}(k)+x_{2}(k)+x_{3}(k) \quad \quad \frac{y_{2}}{k}=c_{1}x_{1}(k)+c_{2}x_{2}(k)+c_{3}x_{3}(k)
$$
Possiamo dunque ricavarne l'equazione matriciale.


### Esempio 2, la Popolazione universitaria

Si consideri la descrizione della popolazione studentesca di un ateneo per una laurea di primo livello.
Le classi di età sono gli studenti che frequentano i corsi di un certo anno di studi. (come per le macchine).
Si suppone che uno studente non possa seguire corsi di anni diversi.
Si suppone che l'unico ingresso sia al primo anno e la percentuale $\gamma_{i}$ di bocciati sia costante.

Per il primo anno ho gli studenti che si immatricolano per la prima volta più i bocciati del primo anno stesso:
$$
x_{1}(k+1)=\gamma_{1}x_{1}(k)+ u(k)
$$
Per il secondo anno avremo:
$$
x_{2}(k+1)=(1-\gamma_{1})x_{1}(k)+\gamma_{2}x_{2}(k)
$$
Per il terzo anno:
$$
x_{3}(k+1)=(1-\gamma_{2})x_{2}(k)+\gamma_{3}x_{3}(k)
$$
Per l'uscita siamo interessati a sapere quanti studenti superano il terzo anno:
$$
y(k)=(1-\gamma_{3})x_{3}(k)
$$
Quindi posso ricavare anche in questo caso la rappresentazione matriciale:
$$
A=\begin{bmatrix}
\gamma_{1} & 0 & 0 \\
1-\gamma_{1} & \gamma_{2} & 0 \\
0 & 1-\gamma_{2} & \gamma_{3}
\end{bmatrix} \quad B=\begin{bmatrix}
1 \\
0 \\
0
\end{bmatrix}
\quad
C=\begin{bmatrix}
0 & 0 & 1-\gamma_{3}
\end{bmatrix}
\quad
D=\begin{bmatrix}
0
\end{bmatrix}
$$



# Sistemi dinamici a dati campionati
---
Possiamo modellare questo tipo di sistemi come:

![[Pasted image 20230317085039.png]]

Il primo convertitore D/A è detto **filtro di tenuta**, mentre il secondo viene detto **campionatore**.

### Discretizzazione del segnale

Si parla quindi di conversione da tempo continuo a tempo discreto.
Supponiamo inizialmente che il passo di campionamento sia una quantità $T_{s}$
Dalla rappresentazione matriciale continua dobbiamo trovare le corrispettive matrici "discrete" $A_{d},B_{d},C_{d},D_{d}$.

Facendo uso della formula di Lagrange (vista in [[Movimento di LTI TC]]) e chiamando $t_{0}$ la quantità $i \cdot T_{s}=k$
Il tempo $t=(i+1)T_{s}=k+1$ sarà l'istente di tempo "finale", e mettendo tutto insieme ottengo:
$$
x(k+1)=e^{AT_{s}}x(k)+\left( \int _{0}^{T_{s}}e^{A(T_{s}-\tau)} \, d\tau  \right)Bu(k)
$$
Otteniamo l'espressione delle matrici discrete:
$$
A_{d}=e^{AT_{s}}, \quad B_{d}= \int _{0}^{T_{s}} e^{A(T_{s}-\tau)}\, d\tau \cdot B, \quad C_{d}=C, \quad D_{d}=D
$$

