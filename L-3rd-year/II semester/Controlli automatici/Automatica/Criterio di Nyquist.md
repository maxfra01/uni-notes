# Criterio di Nyquist
---

### Legami fra $D_{a}$ e $D_{w}$

Consideriamo le seguenti osservazioni: $G_{a}(s)=\frac{N_{a}}{D_{a}}$;
$$
W_{y}(s)=\frac{G_{a}(s)}{1+G_{a}(s)}= \frac{N_{a}(s)}{D_{a}(s)+N_{a}(s)}=\frac{N_{w}(s)}{D_{w}(s)}
$$
Dunque possiamo osservare, e questo sarà importante, che:
$$
D_{w}(s)=D_{a}(s)(1+G_{a}(s)) \implies \frac{D_{w}(s)}{D_{a}(s)}=1+G_{a}(s)
$$
### Il criterio

Siano $n_{i,a}$ il numero di poli instabili della fdt ad anello $G_{a}(s)$, e $n_{i,c}$ il numero di poli instabili della fdt in catena chiusa $W(s)$.
Sia inoltre $N$ il numero di giri compiuti in **senso orario** da $G_{a}(j\omega)$ intorno al punto $(-1,0)$ al variare di $\omega$.

Allora si può dimostrare che: $N=n_{i,c}-n_{i,a}$, notando che i due parametri non assumono mai valori negativi.
Notiamo che se $N$ è negativo è da intendersi come giri in senso antiorario.
Possiamo a questo punto **enunciare il criterio di Nyquist**:

Condizione necessaria e sufficiente per l’asintotica stabilità del sistema in catena chiusa è:
$$
n_{i,c}=0 \implies N=-n_{i,a}
$$
$N$ inoltre, è ben definito se il diagramma di NYquist non passa per il **punto critico di Nyquist $(-1,0)$**.

### Analisi della stabilità 

1. Si valuta il numero di poli instabili $n_{i,a}$ di $G_{a}(j\omega)$
2. Si traccia il diagramma di Nyquist di $G_{a}(j\omega)$, determinando la posizione del punto critico
3. Si calcola graficamente $N$, vedendo quanti giri ci sono attorno al punto critico.
4. Si calcola $n_{i,c}=N+n_{i,a}$ e distinguo due casi: 
	 - se $n_{i,c}=0 \implies stabile$
	- se $n_{i,c}>0 \implies instabile$


### Passaggio per il punto critico

Se si trova una pulsazione $\omega^*$ tale per cui:
$$
|G(j\omega^*)|=1 \quad \arg(G(j\omega^*))=-180°
$$
Allora il diagramma di Nyquist **passa per il punto critico $(-1,0)$** e perciò $N$ **non risulta ben definito**: in questi casi **il criterio di Nyquist non è applicabile**.

Sappiamo però che in questi casi il diagramma ha **almeno un polo sull'asse immaginario**, perciò il **sistema risulta non essere asintoticamente stabile**.



# Estensione del criterio di Nyquist
---
Consideriamo il caso in cui la funzione di trasferimento ad anello sia definita a meno di un **fattore di guadagno $K_{c}$**:
$$
G_{a}(s)=K_{c}G_{a,f}(s)
$$
In tal caso la relazione fra la fdt ad anello e quella in catena chiusa è pari a:
$$
\frac{D_{W}(s)}{D_{a}(s)}=1+K_{c}G_{a,f}(s)
$$
Possiamo allora dire che la differenza fra i poli instabili della fdt in catena chiusa e il numero di poli instabili della fdt ad anello, è pari al numero di giri che il diagramma di Nyquist compie intorno al punto critico $( -\frac{1}{K_{c}}, 0)$ 
Facendo questa considerazione sul **punto critico variabile**, si può procedere nel solito modo, andando a ricavare $N$ per ogni valore di $K_{c}$ per cui appunto $N$ è ben definito.

### Applicazione su sistemi con retroazione positiva

Nel caso di retroazione positiva si ha:
$$
\frac{D_{W}(s)}{D_{a}(s)}=1-G_{a}(s)
$$
Allora la differenza fra il numero di poli instabili della fdt in catena chiuso e il numero di poli instabili della fdt ad anello è pari al numero di giri che il vettore $-G(j\omega)$ compie attorno a $(1-,0)$, **equivalente al numero di giri che $G(j\omega)$** fa intorno a $(1,0)$.

Nel caso di guadagno variabile allora si avrà il numero di giri intorno a $\left( \frac{1}{K_{c}},0 \right)$
Procediamo poi nel solito modo, notando che **cambiare il segno di $K_{c}$ equivale a cambiare il segno della retroazione.**

