# Possesso

In Rust, ogni valore introdotto nel programma è posseduto da una ed una sola
variabile: un particolare blocco logico contenuto nel compilatore, detto **borrow** **checker**, verifica formalmente questo fatto, per ogni punto dell’esecuzione del programma
Ogni violazione porta ad un fallimento della compilazione!

Possedere un valore significa essere responsabili del suo rilascio: se il valore contiene una risorsa (puntatore ad memoria dinamica, handle di file o socket, …), questa deve essere liberata.
Dopodiché occorre restituire al sistema operativo la memoria in cui il valore è memorizzato.

Il rilascio avviene quando la variabile che lo possiede esce dal proprio scope
sintattico o quando le viene assegnato un nuovo valore: Rust offre un meccanismo (drop) mediante il quale è possibile associare azioni arbitrarie da eseguire prima che sia liberata la memoria

Il rilascio può essere rimandato a dopo, se il contenuto della variabile viene trasferito (**mosso**) in un’altra variabile: in questo caso la nuova variabile diventa responsabile del suo rilascio.

### Movimento

Quando si dichiara una variabile, quest'ultima inizia a possedere il valore: nel caso si decidesse di copiare la variabile in una nuova allora il valore precedentemente posseduto viene rilasciato e la nuova variabile diventa proprietaria del nuovo valore.

```rust
let mut v1 = 19;
let v2 = v; //v1 è sollevato dai doveri di rilascio 
```

Se una variabile viene assegnata ad un'altra variabile oppure passata come argomento ad una funzione, il contenuto viene **mosso** nella destinazione:
- La variabile originale non può essere acceduta in **lettura** daranno errori di compilazioni (è come se la variabile originale venisse resa non inizializzata)
- Un tentativo di **scrittura** nella variabile originale riabilitano la lettura di quest'ultima, che inizia a possedere il nuovo valore.
Dal punto di vista pratica il movimento non è altro che una copia bit a bit del valore.

### Copia

Alcuni tipi di Rust, come i tipi numerici **implementano il tratto COPY**: nel caso di assegnazione ad altra variabile (o chiamata a funzione) non perdo i diritti di lettura dopo l'assegnazione.
Ciò significa che posso continuare ad usare una variabile dopo aver mosso il valore in un'altra variabile.
Il tratto copy è mutualmente esclusivo con il tratto drop.

![[Pasted image 20240318121005.png]]

Anche i puntatori non mutabili implementano il tratto copy.

### Clonazione

Se i tipi implementano il tratto clone, allora è possibile chiamare il metodo `clone()` che esegue una copia in **profondità** (e quindi può avere un costo elevato).
Affinchè un tipo possa implementare il tratto **copy** è necessario che implementi anche il tratto clone.
L'invocazione di clone è modificabile dal programmatore, basata sulla funzione `memcpy()`.

### Riferimenti

L'implementazione dei puntatori in rust è differente a seconda del tipo di dato copiato:
- **Simple pointer**: usato se il compilatore conosce già la dimensione del dato
- **Fat pointer**: usato se la dimensione del dato è noto solamente in fase di esecuzione
- **Double pointer**: usato se il tipo di dato puntato è noto solamente per l'insieme di tratti che implementa

![[Pasted image 20240318124556.png]]
![[Pasted image 20240318124607.png]]

Il double pointer si utilizza ad esempio nell'apertura su un file: ci si limita a considerare il file come una zona scrivibile con una serie di tratti.

I **tempi di vita dei riferimenti** sono controllati dal borrow checker e quest'ultimo garantisce che gli accessi ad un riferimento avvengano solamente nell'intervallo temporale in cui il dato esiste.

Un tempo di vita può essere espressa nella firma dei tipo tramite:

```rust
&'a NomeTipo;
&'static NomeTipo;
```

### Riassunto

- Ciascun valore ha un **unico possessore** (variabile o campo di una struct): il valore è rilasciato (drop) quando si esce dallo scope della variabile o quando al possessore è assegnato un nuovo valore.
- Può esistere **al più un solo puntatore mutabile** ad un dato valore
- Possono esistere **molti riferimenti immutabili** ad un valore
- Tutti i riferimento devono avere un tempo di vita **minore** del valore a cui fanno riferimento.

### Slice

Le slice sono viste su sequenze contigue di elementi: internamente sono rappresentate come un puntatore (che punta all'inizio della sequenza) e da un intero che rappresenta il numero di elementi da considerare. Il tipo di una slice è `[T]`.

Le slice **non posseggono i dati**: questi appartengono sempre alla variabile da cui la slice è derivata: tutti i valori sono garantiti essere inizializzati, gli accessi sono verificati in fase di esecuzione.

### Vantaggi del possesso
- Non esiste il concetto di puntatore nullo, quindi non è possibile dereferenziarlo.
- Gli errori di segmentazione o accesso illegale sono evitati dal compilatore
- I dangling pointer non esistono
- Il compilatore evita buffer overflow
- Gli iteratori non eccedono mai i loro limiti

![[Pasted image 20240318140231.png]]

Next: [[Tipi Composti]]