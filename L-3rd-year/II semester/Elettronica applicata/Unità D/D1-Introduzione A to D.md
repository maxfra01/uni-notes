# Quantizzazione
---

![[Pasted image 20230509163627.png]]

Il segnale analogico corrisponde a una sequenza di tensioni variabili, in un intervallo che denotiamo con $S$.
Possiamo definire l'errore di quantitzzazione come la differenza fra la tensione effettiva e il bit di riferimento centrale.

![[Pasted image 20230509163826.png]]

Possiamo rappresentare la conversione anche sul piano xy:

![[Pasted image 20230509163920.png]]

In questo scenario abbiamo che l'errore di quantizzazione rappresentato in funzione del segnale analogico:

![[Pasted image 20230509164049.png]]

Ha una distribuzione uniforme nell'intervallo $[  -\frac{A_{d}}{2} , \frac{A_{d}}{2}]$
Quindi possiamo ricavarne la varianza:
$$
\sigma_{\epsilon q}^2=\int_{\frac{-A_{d}}{2}}^{A_{d}/2} \epsilon_{q}^2  \rho(\epsilon_{q})   \, d\epsilon_{q}=\frac{A_{d}^2}{12} 
$$
Dove noi sappiamo che $A_{d}=\frac{S}{S^N}$

Possiamo a questo punto definire il **SNR Signal Noise Ratio**:
$$
SNR_{q}=\frac{\sigma_{A}^2}{\sigma_{\epsilon_{q}}^2}
$$
Infatti in questo caso le varianze corrispondo alla potenza dei rispettivi segnali.
I valori del SNR sono sempre nella forma:
$$
SNR = (6N+K)dB
$$
e questo vale solo per segnali che arrivano al fondo scale ($S=A$).
Per segnali di ampiezza $A<S$ la potenza del segnale diminuisce e quindi diminuisce anche SNR (in particolare di -20db/Decade) .
Per segnali di ampiezza $A>S$ si ha che la conversione satura il fondo scala, si ha **overload (sovraccarico)** e in questa situazione SNR diminusice all'aumentare della potenza del segnale.

![[Pasted image 20230509165132.png]]


# Sistema di conversione completo
---
![[Pasted image 20230509165433.png]]

Ogni modulo introduce degli errori e del rumore, ecco perchè introuduciamo il parametro:
$$
SNR_{tot}=\frac{P_{s}}{P_{n,tot}}
$$
Che tiene conto della potenza del segnale $P_{s}$ e dei rumori che aggiunge ogni blocco (i vari contributi si sommano) $P_{n,tot}$

Possiamo dunque esprimere l'efficienza del nostro convertitore, tramite **ENOB Effective number of bit** che indica quanti bit siano effettivamente significativi.
E' definito come:
$$
ENOB=\frac{SNR_{tot}}{6}-0.3
$$

### Topic unità

[[D2-Convertitori D to A]]
[[D3-Convertitori A to D]]
[[D4-Multiplexer e Sample-Hold]]
[[D5-Condizionamento del segnale]]
