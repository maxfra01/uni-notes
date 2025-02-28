
# Schema a blocchi
---
I componenti base degli schemi a blocchi sono: rami, blocchi, sommatori e punti di derivazione.

A partire da equazioni note possiamo costruire lo schema a blocchi:

![[Pasted image 20230502143559.png]]

Da $V_{a}(s)$ devo andare a togliere un contributo $K_{m}\Omega(s)$ quindi la prima parte dello schema a blocchi è la seguente:

![[Pasted image 20230502143714.png]]

Il ramo aperto deve essere completato usando la seconda equazione, e perciò vado a completare lo schema nel seguente modo:

![[Pasted image 20230502143843.png]]



# Algebra a blocchi
---
Possiamo servirci di queste regole per ridurre lo schema in uno più semplice, facendo uso delle seguente regole:
- Blocchi in serie moltiplicano la funzione di trasferimento
- Blocchi in parallelo sommano la funzione di trasferiemento
- Spostamente rispetto a un sommatore 

![[Pasted image 20230502144319.png]]
![[Pasted image 20230502144339.png]]

E' possibile spostare un blocco rispetto a un punto di derivazione:

![[Pasted image 20230502144536.png]]



# Calcolo della funzione di trasferimento
---
A partire da uno schema a blocchi calcoliamo la funzione di trasferimento ad anello chiuso $W_{y}(s)$ con le tecniche già viste a TES.

![[Pasted image 20230502145408.png]]

Il contributo $G(s)$ è detta **fdt del ramo diretto** oerchè collega direttamente ingresso e uscita.
$H(s)$ è la fdt **del ramo di retroazione**.
$G(s)H(s)$ è la fdt **dell'anello chiuso**
A denominatore è presente un segno **opposto** a quello del sommatore.

In generale, in uno schema ad anello chiuso la **forma della funzione di trasferimento** è sempre la seguente:
$$
W_{y}(s)=\frac{y(s)}{y_{des}}=\frac{G_{ramo\,diretto}(s)}{1 \mp G_{anello}(s)}
$$

Considerando un ipotetico disturbo **ad ingresso $y_{des}$ nullo** allora scrivo l'espressione della fdt:

![[Pasted image 20230502151600.png]]

Tra ingresso considerato (ovvero il disturbo) e l'uscita non ho blocchi quindi $1$.
A denominatore ci sarà $1$ meno (in quanto segno opposto alla retroazione) e la fdt dell'anello:
$$
W_{d}(s)=\frac{1}{1 + G(s)H(s)}
$$
