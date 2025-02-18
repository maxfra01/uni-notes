# Box

Struttura che incapsula un puntatore ad un blocco allocato dinamicamente sullo heap all’atto della sua costruzione (tramite il metodo `Box::new(t)` ).
Il valore in questione è **posseduto dalla box**, quindi all'uscita dello scope sintattico viene direttamente rilasciato.

Se la grandezza del tipo `T` non è nota allora la box diventa un **fat pointer** dove un `usize` informa il programmatore dello spazio usato 

# RC

Nelle situazioni in cui occorre disporre di più possessori di uno stesso dato immutabile, è
possibile usare questo smart pointer.
Internamente mantiene una copia del dato e due contatori: il primo indica quante copie del
puntatore esistono, il secondo quanti riferimenti deboli sono presenti.
Ogni volta che questo puntatore viene clonato, il primo contatore viene incrementato.
Quando il puntatore esce dal proprio scope, il contatore viene decrementato: se il risultato è
0, il blocco viene rilasciato. `Rc<T>` si presta a realizzare alberi e grafi aciclici.
**Non può essere usato da più di un thread**.

![[Pasted image 20240507131142.png]]

Tramite la notazione `ref = &*rc` posso ottenere un puntatore normale a partire da un RC, a condizione che il tipo T implementi `Sized`.
Il campo `strong` viene incrementato se viene clonato il riferimento, mentre quando si fa un drop di un puntatore viene decrementato.

Tutte le funzioni implementate da RC sono state scritte con `this` al posto che `self` per evitare problemi di omonimia: non posso quindi chiamare i metodi di RC con la notazione puntata, ma devo necessariamente usare quella estesa.

# Weak

Per creare un puntatore weak si deve eseguire un **downgrade** di un valore di tipo RC tramite:

```rust
Rc::downgrade(&rc);
```

L'oggetto weak **non implementa il tratto deref**, perciò se avessimo bisogna di accedere al dato puntato dobbiamo fare **upgrade** a RC. Questo è possibile solamente se `strong_count` > 0, ovvero il valore originale è ancora in vita.
Il metodo Upgrade restituisce un Option, in quanto il dato potrebbe non essere in vita.

# Cell

Questo tipo di **struct** offre dei metodi per modificare il dato contenuto all'interno senza richiedere la **mutabilità del contenitore**. In altre parole, la cell implementa la **interior mutability**.

```rust
use std::cell::Cell

struct my_struct {
	a:i32,
	b: Cell<i32>
}

let s = my_struct {
	a: 0,
	b: Cell::new(1)
} 

s.a = 3; //ERRORE, struct non mutabile

s.b.set(4); //OKAY, è un cell
```

Altri metodi offerti da Cell:

![[Pasted image 20240507134850.png]]

# RefCell

La struct `std::cell::RefCell<T>` rappresenta un blocco di memoria a cui è possibile accedere attraverso particolari smart pointer che simulano il comportamento di riferimenti condivisi e mutabili.
Oltre al dato, un refcell contiene anche un flag che indica se è già stato creato un riferimento mutabile o non mutabile.
Posso creare un riferimento mutabile solo se non ne esiste un altro in vita.
Posso creare un riferimento non mutabile solo se non ne esiste un altro in vita.

# Cow

Smart pointer che implementa il meccanismo **clone on write**. 
Se ci cerca di modificare il dato contenuto, e questo è condiviso, il dato viene clonato: si prende possesso della copia e si effettua la modifica, lasciando l’originale invariato.

Se il dato che si vuole modificare era già posseduto, non avviene nessuna clonazione e si opera la modifica direttamente.