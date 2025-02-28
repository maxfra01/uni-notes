**Massimo Francios, s328914, Homework #2**

# Decision tree (1)

Generando un decision tree con minimal gain pari a 0.01 si ottiene inizialmente un albero con **altezza 6** (numero di archi). In questo caso l'attributo più selettivo è la radice dell'albero, ovvero **node-caps**.

Esempio di **partizionamento puro** (una foglia contiene solo elementi di una singola classe):

![[Pasted image 20231201125900.png]]

# Impatto del minimal gain e tree depth (2,3)

Di seguito sono riportate le diverse configurazioni al variare del minimal gain (**mg**) e maximal depth (**md**):

### mg=0.05 md=5
![[Pasted image 20231201143419.png]]
![[Pasted image 20231201143249.png]]

### mg=0.05 md=3
![[Pasted image 20231201143610.png]]
![[Pasted image 20231201143621.png]]

### mg=0.03 md=7
![[Pasted image 20231201143722.png]]
![[Pasted image 20231201143737.png]]

### mg=0.05 md=4
![[Pasted image 20231201143948.png]]
![[Pasted image 20231201143958.png]]

### mg=0.001 md=10
![[Pasted image 20231201144037.png]]
![[Pasted image 20231201144048.png]]

Dalle configurazioni riportate notare il comportamento degli alberi quando si cambiano minimal gain e maximal depth: in particolare possiamo notare che andando a **diminuire la profondità dell'albero** generiamo degli alberi molto più piccoli e semplici, che per questo specifico dataset danno **valori di accuratezza più alta**.
**Aumentando la profondità massima** l'albero risulterà più complesso e incline all'overfitting: per questo dataset l'accuratezza risulterà minore (vedere caso mg=0.03 md=7).

Il **minimal gain** indica la soglia per effettuare uno split: **diminuendolo** si ottengono alberi più precisi e complessi, ma inclini all'**overfitting** (come nell'esempio con mg=0.001) e quindi con **accuratezza minore**.
Viceversa **aumentando** il minimal gain possiamo andare a diminuire la complessità dell'albero.
La miglior combinazione fra quelle sopra è caratterizzata da un mg=0.005 e da md=3, con un accuratezza pari al 74,82%.

# K-NN e Bayes (4)

Usano il classificatore **K-NN** dobbiamo scegliere il parametro K adeguato per il dataset in esame. Di seguito sono riportati gli screenshot con diverse configurazioni:

### K=5 (default)
![[Pasted image 20231201111658.png]]

### K=3
![[Pasted image 20231201111837.png]]

### K=2
![[Pasted image 20231201111857.png]]

### K=9
![[Pasted image 20231201111931.png]]

### K=23
![[Pasted image 20231201112116.png]]

Possiamo notare come il parametro K influenzi l'accuratezza media delle predizioni. in particolare se consideriamo un valore di K **troppo piccolo**, allora il modello sarà influenzato da errori e dagli outliers del dataset, ottenendo una scarsa accuratezza.
Al contrario nel caso di K **troppo grande**, il modello ignora caratteristiche locali perchè va ad includere dati "lontani": si ottiene una bassa accuratezza anche in questo caso.

Il primo comportamento lo si può notare da valori di K pari a 2 e 3, dove l'accuratezza media risulta più bassa rispetto (ad esempio) a K=5.
Il secondo comportamento lo si nota nel caso di K=23.
In questo problema specifico il valore ottimale di **K è intorno al 9**, dove si registra la migliore accuratezza del modello.
### Naive Bayes

Usando il classificatore **Naive Bayes** si ottiene la seguente matrice di confusione: possiamo notare che mediamente questo classificatore ha prestazioni **inferiori** rispetto al classificatore K-NN.

![[Pasted image 20231129215311.png]]

# Matrice di correlazione (5)

Screenshot della matrice:

![[Pasted image 20231130205428.png]]

Per analizzare quale coppia è maggiormente correlata è sufficiente cercare valori vicini a 1 (o -1 per correlazione negativa): gli attributi maggiormente correlati sono **inv-nodes** e **node-caps**.
Possiamo affermare che l'**ipotesi di indipendenza di Bayes non è del tutto soddisfatta**, ma dato che la maggior parte degli attributi sono praticamente indipendenti, il risultato è comunque valido.