# Comparatori integrati
---

![[Pasted image 20230314162152.png]]

Quando è presente isteresi in un comparatore di parla di **trigger di schmitt**.


# Generatori di onde quadre
---
Supponiamo di avere a dispozione una certe onda quadra che passa per un filtro RC passa basso: in uscita avremo dei rami di esponenziale.

![[Pasted image 20230314162551.png]]

Prendendo i rami di esponenziali e collegandoli a un comparatore con isteresi è possibile far commutare a 0 o 1 logico l'uscita del comparatore. Con una retroazione il sistema diventa autosostenibile e continuerà ad oscillare tra livello alto e basso.

![[Pasted image 20230314163051.png]]

Per calcolare il **periodo** dell'onda quadra procediamo coi seguenti passaggi.
Considerando il condensatore che segue la legge:
$$
v_{C}(t)=V_{UL}- (V_{UL}- V_{S_{2}})e^{- t/{RC}}
$$
Ci chiediamo il valore del tempo $T'$, ovvero il tempo di "scarica parziale", e sostituendo e manipolando $v_{C}(T')$ abbiamo:
$$
T'=RC \ln\left[ \frac{V_{UL}-V_{S_{2}}}{V_{UL}-V_{S_{1}}} \right]
$$
Per $T''$ ovvero il tempo di "carica" ripetiamo il procedimento e a questo punto il periodo è pari alla somma dei due semiperiodi:
$$
T=T'+T''
$$
![[Pasted image 20230314163600.png]]

I parametri in gioco sono la **resistenza** e la **capacità**:
1. La resistenza dev'essere superiormente limitata dal fatto che la corrente che circola deve andare tutta nel condensatore. Se scelgo una resistenza troppo grande è possibile che la corrente nel condensatore sia troppo piccola
2. La resistenza non può essere troppo bassa in quanto fa da carico in uscita, ma ciò produrrebbe una riduzione della dinamica di uscita e quindi difficoltà a pilotare altre porte.
3. La capacità deve essere abbastanza grande da superare la capacità parassita in ingresso


# Oscillatori
---

### Oscillatore ad anello

E' un anello formato da un numero **dispari** di inverter ($n$).
E' di tipo **astabile** fra due stati.

![[Pasted image 20230314164904.png]]

Il periodo di questa onda quadra generata non è preciso in quanto i ritardi variano con l'alimentazione, temperatura, difetti di fabbricazione.
Il periodo è dato da $n$ volte il periodo basso alto ed $n$ volte quello da alto a basso
$$
T=n(T_{pdLH}+T_{pdHL})
$$

### Oscillatore al quarzo

Esso si basa sulla retroazione negativa, e in particolare immaginiamo di avere una situazione:

![[Pasted image 20230314165439.png]]

Caratterizzando il circuito siamo in grado di ricavare l'espressione dell'uscita:
$$
U=\frac{A(f)}{1+A(f)\beta(f)}I
$$
Se la quantità a denominatore è pari a 0 allora il sistema è instabile e potrebbe oscillare senza un ingresso:
La condizione $A(f)\beta(f)=-1$ è detta **criterio di Barkhausen**.
Vogliamo che si verifichi $A(f)\beta(f)<-1$ in modo da ottenere oscillazioni.
Normalmente il guadagno è costante e l'unica cosa che dipende dalla frequenza è la rete di reazione.
La rete di reazione è spesso costruito con un **cristallo piezoelettrico (quarzo)**.

Il **quarzo** genera tensione se sottoposto a sollecitazioni meccaniche, inoltre di deforma in presenza di un campo elettrico.

L'oscillatore di **Pierce** segue il seguente schema circuitale:

![[Pasted image 20230314170609.png]]

Il guadagno dell'amplificatore è negativo in quanto costituito da una porta NOT.
Questa porta **forza l'inverter a lavorare intorno a** $v_{in}=v_{out}$
Grazie al quarzo abbiamo un guadagno di retroazione $\beta(f)>0$ in modo da avere un denominatore minore di uno, in grado di innestare le oscillazioni.
La resistenza $R_{2}$ serve a proteggere il cristallo di quarzo, inoltre previene frequenze armoniche.

### PLL Phase Locked Loop

![[Pasted image 20230314171348.png]]
