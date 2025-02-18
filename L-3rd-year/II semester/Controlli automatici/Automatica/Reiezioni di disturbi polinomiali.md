# Disturbi
---
Un **Disturbo** è un segnale indesiderato che modifica il comportamento, e l'uscita del sistema. Cause del rumore sono ad esempio il rumore elettrico dei dispositivi usati, oppure eventi "fisici".

I disturbi possono influenzare il sistema in diversi punti dello schema a blocchi: abbiamo perciò disturbi agenti direttamente sull'uscita del sistema, sul riferimento, sul comando, sulla retroazione e in generale in ogni punto intermedio.

### Tipologie di disturbi

Abbiamo sempre la stessa classificazione fra disturbi **polinomiali** e **sinusoidali**, in cui distinguiamo come sempre:
- **Disturbo costante**, che è un polinomio di grado zero, che modellizza un errore di offset o errori di natura fisica
- **Disturbo a rampa**, ovvero polinomi di primo grado, che modellizza la deriva di un segnale
- **Disturbo sinusoidale**, modellizza il rumore di un dispositivo, in modo generico


# Effetti sull'uscita
---
L'effetto sull'uscita in regime permanente di un disturbo polinomiale dipende da:
- **Posizione** di dove si applica il disturbo, e quindi da **fdt disturbo-uscita**
- **Tipo** e **Guadagno stazionario** dei blocchi presenti sull'anello
- **Grado** del disturbo polinomiale

Per procedere con lo studio dei disturbi, usiamo sempre il principio di sovrapposizione, "spegnendo" il riferimento e studiano l'uscita.
Nel caso di più disturbi i contributi di ognuno si sommano.
Una volta ottenuto l'effetto $y_{d,\infty}$ sull'uscita, posso andare ad esprimere l'errore associato a quel disturbo come:
$$
e_{d,\infty}=-y_{d,\infty}
$$
Consideriamo ora un caso pratico, e per prima cosa spegnendo il riferimento, semplifichiamo lo schema a blocchi:

![[Pasted image 20230505103236.png]]

Possiamo calcolare la **fdt errore uscita** usando le regole dell'algebra a blocchi:
$$
W_{dy}(s)= \frac{y(s)}{d(s)} =\frac{1}{1+G_{a}(s)}
$$
Trovandoci in asintotica stabilità è possibile usare il teorema del valore finale per ottenere l'uscita a regime:
$$
y_{dy,\infty}=\lim_{ t \to \infty } y_{dy}(t)=\lim_{ s \to 0 } y(s)=\lim_{ s \to 0 } W_{dy} \cdot d(s)
$$
Da questi calcoli si capisce se sarà possibile reittare completamente il disturbo o cercare di attenuarlo.

### Studio del disturbo costante

Nel caso di $d_{y}(t)=D_{y}$ (disturbo costante) avremo in Laplace $d_{y}(s)=\frac{D_{y}}{s}$
Possiamo inizialmente ricavare il valore finale a regime dell'uscita con il teorema visto prima.

Nel caso di $G_{a}$ di tipo 0, l'effetto del disturbo sull'uscita in regime permanente è finito, non nullo, che vale:
$$
y_{dy, \infty}=\frac{D_{y}}{1+K_{Ga}}
$$

Nel caso di $G_{a}$ di tipo 1, allora l'effetto del disturbo sull'uscita sono **nulli**:
$$
y_{dy, \infty} = 0
$$
Cioè **Un sistema di almeno tipo 1 o superiore, risulta astatico per un disturbo collocato sull'uscita**.

### Studio del disturbo a rampa

Ci troviamo nel caso di $d_{y}(t)=\alpha_{dy}t$ e nel dominio di Laplace $d_{y}(s)=\frac{\alpha_{dy}}{s^2}$
Possiamo ancora calcolare il valore dell'uscita a regime.

Nel caso di $G_{a}(s)$ di **tipo 0** allora l'effetto sull'uscita è di carattere **divergente**:
$$
y_{d,\infty}=\infty
$$
Nel caso di $G_{a}(s)$ di tipo 1 l'effetto è finito e non nullo:
$$
y_{d,\infty}=\frac{\alpha_{dy}}{K_{Ga}}
$$
Se $G_{a}(s)$ è **almeno di tipo 2 o superiore** allora in questo caso l'effetto sull'uscita è nullo:
$$
y_{d,\infty}=0
$$


# Effetti sul ramo diretto
---
Considerando un sistema con disturbo sul ramo diretto:

![[Pasted image 20230505104747.png]]

Mettendo a zero il riferimento rimodelliamo lo schema, semplificandolo, ottenendo:

![[Pasted image 20230505104816.png]]

Con le regole dei blocchi abbiamo:
$$
W_{di}(s)= \frac{y(s)}{d_{i}}=\frac{G_{2}(s)}{1+G_{1}(s)G_{2}(s)}= \frac{G_{2}(s)}{1+G_{a}(s)}
$$

### Studio del disturbo costante

Nel caso di disturbo costante $d_{i}(t)=D_{i}$ allora nel caso di $G_{1}(s)$ e $G_{2}(s)$ di tipo 0 abbiamo un effetto sull'uscita pari a:
$$
y_{di,\infty}=\frac{K_{G_{2}}D_{i}}{1+K_{G_{1}}K_{G_{2}}}
$$
Se $G_{2}(s)$ è di tipo 1 e $G_{1}(s)$ è di tipo zero, l'effetto è ancora una volta finito e non nullo e vale:
$$
y_{di,\infty}=\frac{D_{i}}{K_{G_{1}}}
$$
Ovvero dipende solo dal guadagno del blocco senza poli nell'origine.

Se $G_{1}(s)$ è almeno di tipo 1, l'effetto del disturbo è nullo, **indipendentemente da $G_{2}(s)$**.

Possiamo affermare che **un sistema è astatico a un disturbo costante posto lungo un ramo diretto se e solo se nei blocchi a monte del disturbo c'è almeno un polo in $s=0$**

### Studio del disturbo a rampa

Considerando un disturbo del tipo $d_{i}(t)=\alpha t$, i blocchi a valle risultano ininfluenti, perciò guardiamo solo $G_{1}(s)$ e distinguiamo i seguenti casi:

![[Pasted image 20230505110048.png]]



# Implicazioni sul progetto del controllore
---

### Specifica di attenuazione totale

Per reittare completamente un disturbo polinomiale di **grado $h$** allora è necessario che la fdt di $G_{1}(s)$ (composto dalla cascata dei blocchi che precedono l'ingresso) abbia almeno $h+1$ poli.

### Specifica di attenuazione

L'effetto in regime permanente di un disturbo di grado $h$ è finito e non nullo se la $G_{1}(s)$ ha esettamente $h$ poli.

Nel caso di disturbo limitato si ha sempre un effetto limitato, e ciò significa che per attenuare questo non c'è mai bisogno di mettere un polo in 0 al controllore.
Per invece un disturbo a rampa, il suo effetto risulta limitao solamente se nella $G_{1}(s)$ risulta esserci **1 polo** in 0.

Se ho un disturbo di grado $h$ e la fdt di $G_{1}(s)$ ha esattamente $h$ poli allora si crea un vincolo:
$$
|y_{d,\infty}(K_{G_{1}})|\leq y_{d,max} \implies |K_{G_{1}}|\geq K_{G_{1},min} \implies |K_{C}|\geq K_{C,min}
$$
