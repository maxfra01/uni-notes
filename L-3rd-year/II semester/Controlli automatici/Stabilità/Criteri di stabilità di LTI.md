I criteri visti fino ad ora richiedono la conoscenza degli autovalori dalla matrice di stato A, e quindi del calcolo degli zeri del polinomio caratteristico.
I criteri di seguito non richiedono questo calcolo.

# Regola dei segni di Cartesio
---
Fornisce solamente una condizione necessaria per far sì che tutte le radici di $p(\lambda)$ siano a parte reale strettamente negativa.
Si consideri un polinomio di grado $n$:
$$
p(\lambda)=a_{n}\lambda^n+a_{n-1}\lambda^{n-1}+\dots+a_{1}\lambda +a_{0}
$$
Il **numero di radici reali positive** è pari al numero $\nu$ di variazioni di segno fra coefficienti consecutivi non nulli, oppure è inferiore a $\nu$ per un multiplo intero di 2.

Ad esempio per $p(\lambda)=\lambda^3 +\lambda^2-\lambda-1$ c'è solo un cambio di segno di coefficienti consecutivi perciò è presente una sola radici reale positiva.

Un **corollario** di questa regola afferma che, dato il polinomio a coefficienti reali allora il **numero di radici reali negatice** è pari al numero $\omega$ di variazioni di segno fra coefficienti consecutivi non nulli del polinomio $p(-\lambda)$

Una condizione **necessaria ma non sufficiente** per far sì che $p(\lambda)$ abbia radici con parte reale strettamente negativa,  è che non ci siano variazioni di segno fra coefficienti consecutivi non nulli.


# Criterio di Routh-Hurwitz
---
Condizione necessaria affinchè tutte le radici del polinomio caratteristico siano a parte reale strettamente minore di 0, è che tutti gli $n+1$ coefficienti siano di segni concorde.

Il criterio fa uso di una tabella:
- Essa è costituita da $n+1$ righe
- Gli elementi delle prime due righe sono costituiti dai coefficienti di $p(\lambda)$
- Nell'ultima riga compare il termine noto $a_{0}$

Le prime due righe hanno la seguente struttura:

![[Pasted image 20230329092038.png]]

Dalla $n-2$ esima righe procediamo a far partire il primo coefficiente appunto dal $n-2$ esimo e poi si decrementa l'indice di 2 quindi $b-2,b-4\dots$

Gli elementi successivi si costruiscono come:

![[Pasted image 20230329092528.png]]

Dove il termine che divide il determinante è sempre il termine in posizione 2,1 della matrice.
Per la matrice la prima colonna resta invariata, è quella sopra l'elemento in considerazione, mentre la seconda colonna è quella che vede in alto in diagonale l'elemento considerato.

Il criterio dice che **considerando solamente la prima colonna, se tutti gli elementi hanno segno concorde allora tutte le radici del polinomio hanno parte reale strettamente negativa**.
Inoltre **se la tabella può essere completata** allora:
- nessuna radice ha parte reale nulla
- il numero di radici a parte reali strettamente maggiori di 0 è dato **dal numero di variazioni di segno nella prima colonna**


### Esempio
$$
p(\lambda)=\lambda^4+6\lambda^3+8\lambda^2+\lambda+1
$$
Costruiamo le prime due righe della tabella di routh:
$$
\begin{bmatrix}
1 & 8 & 1 & 0 & \dots \\
6 & 1 & 0 & 0 & \dots
\end{bmatrix}
$$
Ora costruiamo la nuova riga:
$$
b_{2}=-\begin{bmatrix}
1 & 8  \\
6 & 1
\end{bmatrix}/6 \quad b_{0}=-\begin{bmatrix}
1 & 1 \\
6 & 0
\end{bmatrix} / 6 \quad b_{-2}=-\begin{bmatrix}
1 & 0 \\
6 & 0
\end{bmatrix} / 6
$$
E così via... ottenendo alla fine:
$$
\begin{bmatrix}
1 & 8 & 1 & 0 \\
6 & 1 & 0 & 0 \\
\frac{47}{6} & 1 & 0 & 0 \\
\frac{11}{47} & 0 & 0 & 0 \\
a_{0}=1 & 0 & 0 & 0
\end{bmatrix}
$$
Dunque abbiamo completato la tabella dunque valgono le considerazioni:
- Le radici con parte reale maggiore strattamente di 0 non ci sono
- Non ci sono radici con parte reale nulla
- Concludiamo che tutte le radici hanno parte reale strettamente negativa


# Criterio di Jury
---
Il seguente criterio afferma che: 
Una condizione **necessaria e sufficiente** affinchè tutte le $n$ radici del polinomio $p(\lambda)$ **siano strettamente minori di 1** è che:
- Nel caso $n=2$ siano soddisfatte le seguenti disuguaglianze: $$
\begin{cases}
p(\lambda=1)>0  \\
(-1)^np(\lambda=-1)> 0 \\
|a_{n}|>|a_{0}|
\end{cases}
$$
- Nel caso di $n>2$ oltre alle 3 disuguaglianze sopra indicate bisogna ricorrere alla **tabella di Jury** costruita nel seguente modo:

![[Pasted image 20230331135155.png]]

Si parte dalle prime due righe con i coefficienti in ordini inversi.
Dopodichè si va a costruire una nuova riga costituita dai determinanti della matrici formata dai due elementi sopra il nuovo (tenuti fissi) e gli speculari al fondo:

![[Pasted image 20230331135334.png]]

Infine dopo aver costruito la nuova riga se ne inserisce un'altra invertendo gli elementi:

![[Pasted image 20230331135434.png]]

Terminando, ovvero **arrivando alla riga con massimo grado 2** si arriva alla forma:

![[Pasted image 20230331135556.png]]

E si devono rispettare le seguenti condizioni: 
$$
\begin{cases}
|b_{0}|>|b_{n-1}| \\
|c_{0}|>|c_{n-2}| \\
\dots \\
|z_{0}|>|z_{2}|
\end{cases}
$$
