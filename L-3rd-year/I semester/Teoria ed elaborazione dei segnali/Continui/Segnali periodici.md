# Trasformata di Fourier di segnali periodici
---
Tramite calcoli (vedi slide) otteniamo che la trasformata di Fourier del segnale periodico si può scrivere come:
$$
X(f)=\sum_{n=-\infty}^\infty \mu_{n} \cdot \delta\left( f-\frac{n}{T} \right)
$$
Dunque data la presenza della delta lo spettro dei un segnale periodico è "a righe".

![[Pasted image 20221028105148.png]]

I segnali periodici sono un'astrazione matematica, nonostante ciò i segnali ciclici hanno uno spettro molto simile.

I coefficienti della serie di Fourier possono essere espressi utilizzando la TdF del segnale troncato:
$$
\mu_{n}=\int _{-\frac{T}{2}}^{T/2} \,x(t)e^{-j 2\pi nt/T} dt = \frac{1}{T}X_{T}\left( \frac{n}{T} \right) 
$$
Posso dunque esprimere la TdF del segnale periodico come:
$$
X(f)=\frac{1}{T} \sum_{n=-\infty}^\infty X_{T}\left( \frac{n}{T} \right) \cdot \delta\left( f- \frac{n}{T} \right)
$$
Con impulsi equispaziati di $\frac{1}{T}$.


# Treno di impulsi
---
Definiamo il segnale campionatore o treno di impulsi nel tempo:
$$
c_{T}(t)=\sum_{n=-\infty}^{\infty} \delta(t-nT)
$$
Il segnale qui sopra è alla base della teoria del campionamento.
Se provo a farne la TdF trovo (ricordando che $\mathcal{F}(\delta(t))=1$):
$$
C_{T}(f)=\sum_{n=-\infty}^{\infty} \exp(-2j\pi f \cdot nT)
$$
Alternativamente, applicado il risultato a segnali periodici otteniamo:
$$
C_{T}(f)=\frac{1}{T} \sum_{n=-\infty}^\infty \delta\left( f-\frac{n}{T} \right)
$$
Ricordiamo la presenza della dualità fra il dominio del tempo e quello della frequenza.

![[Pasted image 20221028110612.png]]

Come abbiamo già detto possiamo campionare il segnale in alcuni punti (scanditi dal treno di impulsi) moltiplicando il segnale per il treno di impulsi:
$$
x(t) \cdot c_{T}(t)=\sum_{n=-\infty}^\infty x(nT)\delta(t-nT)
$$
Ma è ugualmente possibile effettuarne la convoluzione, ottenendo un segnale periodico di periodo pari alla spaziatura degli impulsi:
$$
x(t)*c_{T}(t)=\sum_{n=-\infty}^\infty (t-nT)
$$
![[Pasted image 20221028111011.png]]


# Segnali periodicizzati
---
Consideriamo il segunte segnale:
$$
x(t)=\sum_{n=-\infty}^\infty z(t-nT)
$$
Non esiste un unico modo per esprimere un segnale periodico, e non c'è un solo modo per calcolare i coefficienti $\mu_{n}$ della serie di Fourier.

