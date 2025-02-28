# Indicatori di robustezza della stabilità
---
Finora il criterio di Nyquist permette di conoscere l'asintotica stabilità del sistema in catena chiusa, a partire dalla sua funzione ad anello.
L'analisi si è basata sul presupposto che $G_{a}(s)$ sia esatta, ma nella realtà l'approssimazione matematica non è perfetta e presenta delle discrepanze.

Abbiamo pertanto delle **variazione della funzione ad anello**.
Vogliamo mantenere l'asintotica stabilità anche in presenza di variazioni, e ciò viene detta **stabilità robusta**.

### Margine di stabilità vettoriale

Un primo indicatore è rappresentato dalla **distanza fra il diagramma di Nyquist e il suo punto critico**.
Per garantire una buona robustezza devo assicurarmi che il diagramma sia **sufficientemente distante** dal suo punto critico:

![[Pasted image 20230512111407.png]]

Questo indicatore è un buon indice di robustezza, ma non è di facile lettura perciò cerchiamo delle alternative.


### Margini di guadagno e di fase

Consideriamo un certo $G_{a}(j\omega)= K_{c}G_{a,f}(s)$ priva di poli a parte reale positiva, tale che il **suo diagramma intersechi l'asse reale in un punto $A$ a destra del punto critico**:

![[Pasted image 20230512111653.png]]

Al variare di $K_{c}$ possiamo notare che la parte sinistra del diagramma si sposta verso sinistra, e lo potrà fare, fino a che il diagramma intersecherà il punto critico.
Definiamo allora il **margine di guadagno**, come il massimo fattore moltiplicativo che **non deve essere raggiunto** per preservare l'asintotica stabilità:
$$
m_{G}=\frac{1}{|x_{A}|}
$$
Come leggere il valore di $m_{G}$:
| Da Bode                                                                                                                                                                        | Da Nyquist                        |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------- |
| Nel caso dei diagrammi di bode possiamo leggere il margine come il modulo che corrisponde alla fase di -180°, cambiata di segno, ovvero $m_{G,dB}=-G_{a}(j\omega_{\pi}) _{dB}$ | Attraverso il calcolo visto sopra |
 
![[Pasted image 20230515130905.png]]


Per quanto riguarda il **margine di fase**: Consideriamo $G_{a}(j\omega)$ a guadagno positivo, priva di poli a parte reale positiva e tale che il suo diagramma polare attraversi una sola volta la circonferenza unitario in un punto $C$.
Richiediamo anche che la **fase del punto $C$** sia: $\phi(C)>-180°$ 
In queste condizione che abbiamo descritto il sistema è **asintoticamente stabile**.

![[Pasted image 20230515131142.png]]

Una perdita di fase **fa avvicinare il punto $C$ al punto critico di Nyquist** fino a quando non si sovrappongo.
Ecco quindi che questa perdita di fase rappresenta il **margine di fase**:
$$
m_{\phi}=180°+ \angle G_{a}(j\omega)
$$
Definiamo la **pulsazione di cross-over** $\omega_{C}$ come la pulsazione nella quale il diagramma di Bode attraversa l'asse degli $0 \space dB$

![[Pasted image 20230515131728.png]]


# Stabilità regolare e marginale
---
Se consideriamo la funzione ad anello $G_{a}(j\omega)$ che gode delle seguenti proprietà:
- Guadagno positivo
- E' a **minima rotazione di fase**, ovvero non ha singolarità a parte reale positiva
- Esiste una sola pulsazione per cui il modulo risulta unitario
- Esiste una sola pulsazione di valore finito per cui la sua fase vale $180^{\circ}$
Allora se **tutte le condizioni sono soddisfatte** posso leggere i margini di fase e guadagno direttamente dai diagrammi di Bode e **se essi risultano positivi** allora il sistema è **asintoticamente stabile**

Un sistema che risulti asintoticamente stabile, per qualunque valore positivo del guadagno NON superiore al margine di guadagno $m_{G}$, viene detto **a stabilità regolare**

Se le condizioni non sono rispettate occorre estendere i concetti di margine:

### Attraversamenti multipli dell'asse reale

Devo applicare il criterio di Nyquist.
I sistemi **a stabilità marginale** sono quelli per i quali si può avere instabilità per valori troppo piccoli o troppo elevati di guadagno.
Esempio su slide.
Troveremo due margini di guadagno, uno superiore e uno inferiore

### Molteplici punti a modulo unitario

Per un sistema il cui diagramma polare interseca più volte la circonferenza unitaria in punti a fase  maggiore di -180°, il margine di fase (secondo la definizione precedentemente data di massima perdita di fase tollerabile senza   andare in instabilità) deve essere letto in corrispondenza del punto di intersezione più vicino all’asse reale

![[Pasted image 20230515135929.png]]

![[Pasted image 20230515140003.png]]

Nell'esempio sopra scelto il punto C in quanto il più vicino all'asse reale.


### Sistema non a minima rotazione di fase

In questi casi può capitare di tutto, quindi devo applicare per forza Nyquist.

```ad-note
In MatLab tramite il comando `margin` ci vengono resistuiti i valori dei margini di modulo e fase.
Sono corretti solo nel caso la funzione ad anello rispetti tutti quei requisiti.
```

