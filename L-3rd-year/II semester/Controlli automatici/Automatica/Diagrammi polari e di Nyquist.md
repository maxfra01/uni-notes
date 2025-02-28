# Diagramma polare
---
A differenza dei diagrammi di Bode, qui nei polari è possibile rappresentare modulo e fase di una certa fdt su un **unico piano complesso**.
Considerando la variabile $s=j\omega$ allora si ha:
$$
G(s)=|G(j\omega)| \cdot e^{j\arg(G(s))}
$$
Quando caratterizziamo la funzione per un certo $\omega=\omega^*$ allora si ottiene:
$$
G(j\omega^*)=M^* \cdot e^{j \phi^*}
$$
Questi due parametrei $M^*$ e $\phi^*$ sono usate come coordinate polari sul piano complesso:

![[Pasted image 20230509152800.png]]

Ripetendo questo criterio con tanti valori di $\omega$ differenti si ottiene un **diagramma polare**.

### Proprietà

1. Considerando un sistema **senza poli nell'origine** allora il diagramma polare ha il suo punto di partenza sull'asse reale.
2. Per sistemi con $i$ poli nell'origine il diagramma polare parte da un punto infinitamente lontano dall'origine, con fase $$
\phi|_{\omega^+ \to 0} = i \cdot -\frac{\pi}{2}+ \arg(K)
$$
3. Se sono presenti dei poli nell'origine allora è possibile trovare l'asintoto da cui parte il diagramma secondo le seguenti regole: ![[Pasted image 20230509153527.png]]

4. Per sistemi strettamente propri (Numero di poli maggiore del numero di zeri) il diagramma polare terminerà nell'origine, in quanto il modulo del guadagno andrà a zero prima o poi ($-\infty$ nel caso di Bode ). Inoltre il diagramma terminerà con fase multipla di $\pm 90°$


### Tracciamento qualitativo

A partire dai diagrammi di bode della funzione è possibile effettuare il tracciamento qualitativo:
1. Determino il comportamento iniziale del diagramma con $\omega \to 0^+$, tenendo conto del numero di poli nell'origine
2. Se sono presenti dei poli nell'origine allora determino anche la fase iniziale e il quadrante di partenza (senza calcolare esplicitamente l'asintoto)
3. Determinare il comportamento finale del diagramma polare, per $\omega \to \infty$ anche se sappiamo già che finiremo dell'origine. Inoltre calcoliamo la fase finale: $$
\phi(\infty) = \phi(0) -90° (n_{n}+m_{p})+ 90°(n_{p}+m_{n})
$$dove $n_{n},n_{p}$ rappresentano rispettivamente il numero di zeri a parte reale negativa e positiva (stessa con per i poli $m$).
4. Si completa l'andamento del diagramma sulla base dei DdB (Diagrammi di Bode).


### Esempio

Consideriamo la funzione:
$$
F_{I}(s)=\frac{1}{s(s+2)(s+4)}
$$
Notiamo che ha un polo nell'origine, dunque è del primo tipo. Non possiede zeri, dunque ho $K_{f}=s \cdot F_{I}(0)=\frac{1}{8}$
La fase iniziale, dato che $i=1$ è $-\frac{\pi}{2}$, la fase finale è data dalla formula sopra ed è pari a $\phi(\infty)=-270°$


# Diagramma di Nyquist
---
Il diagramma di Nyquist di una fdt è una **rappresentazione grafica sul piano complesso** di:
$$
G(s)|_{s=j\omega}=G(j\omega) \quad \textrm{per} \quad \omega \in (-\infty,\infty)  
$$
Perciò la variabile indipendente che consideriamo descrive un ipotetico raggio di valore infinito:

![[Pasted image 20230510090709.png]]

Nel caso di **poli sull'asse immaginario** allora non possiamo procedere come sopra, ma la soluzione è la seguente: considerare come percorso di omega anche dei piccoli cerchi per evitare i poli:

![[Pasted image 20230510090722.png]]

Come conseguenza di ciò, abbiamo che la variabile $s$ varia in un percorso **chiuso**, dunque il **diagramma di Nyquist descrive un percorso chiuso**.
Quando la variabile $s$ percorre il cerchio di raggio infinitesimo allora il modulo va a infinito.
Se la molteplicità del polo è pari a $i$ allora avremo $i$ semicirconferenze percorse in senso antiorario.

### Tracciamento

1. Tracciare il diagramma polare di $G(j\omega)$
2. Tracciare il diagramma simmetrico (del polare) rispetto all'asse reale.
3. Nel caso siano presenti poli sull'asse immaginario, congiungere i rami all'infinito con il numero corretto di circonferenze.
4. Specificare il verso di percorrenza
5. Verificare sia un percorso chiuso

