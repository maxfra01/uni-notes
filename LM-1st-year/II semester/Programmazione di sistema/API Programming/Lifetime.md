# Tempo di vita dei riferimenti

In Rust tutti i valori hanno un tempo di vita.
Normalmente il tempo di vita di un valore inizia alla sua creazione e termina all'uscita dello scope in cui risiede.
Nel caso di accesso con riferimento al valore, quest'ultimo **è valido solamente durante il tempo di vita del valore**.

Nel caso di una funzione che riceve uno o più riferimenti, allora la funzione deve garantire che le operazioni siano svolte nel tempo di vita di tutti i riferimenti.
Possiamo indicare in modo **esplicito** che due riferimenti siano soggette allo **stesso tempo di vita**: utilizziamo una notazione che assomiglia a quella dei generics.

```rust
fn aFunction<'a,'b>(p1: &'a i32, p2: &'b i32) {...}
```

Nel momento un cui una funzione ritorni un riferimento allora dev'essere necessario indicare il tempo di vita in modo esplicito

```rust 
fn f<'a, 'b>(p1: &'a Foo, p2:&'b Bar) -> &'b i32 { /*…*/ return &p2.y; }
```

Nell'esempio qui sopra stiamo evidenziando che il valore ritornato proviene dal riferimento con tempo di vita `'b`. Se non avessi indicato il tempo di vita, allora quel valore sarebbe stato disponibile solamente nel tempo di vita di entrambi i riferimenti.
Lo scopo dell'uso esplicito dei tempi di vita è duplice: il primo indica effettivamente da quale parametro va ereditato il tempo di vita; il secondo fa si che la funzione restituisca solo indirizzi a cui sia lecito accedere 
### Esempi

Consideriamo la seguente funzione che riceve due riferimenti:

```rust
fn f(s: &str, v: &mut Vec<&str>) {
	v.push(s);
}
```

Per il compilatore significa che ho una stringa che esiste con un tempo di vita `'a` e un vettore con tempo di vita `'b`: a questo punto il compilatore non può sapere che il tempo di vita della stringa sia sufficiente ad essere acceduta dal vettore. Il compilatore restituisce un errore di **lifetime mismatch**.

Per risolvere il problema possiamo iniziare ad indicare esplicitamente i tempi di vita dei dati passati come parametro (incluso il tipo di elementi del vettore):

```rust
fn f<'a>(s: &'a str, v: &'a mut Vec<&'a str>)
```

### Elisione dei tempi di vita

Quando scriviamo del codice Rust, il compilatore, in maniera implicita, assegna una tempo di vita diverso a ogni riferimento contenuto in strutture dati o passato come parametro a funzioni.
**Se una funzione ritorna un riferimento**:
- Se **un solo parametro** è dotato di tempo di vita, allora il compilatore intuisce che il tempo di vita del risultato sarà lo stesso
- Se **più di un parametro è dotato di tempo di vita**, allora sta al programmatore indicare esplicitamente il tempo di vita
Nel caso di metodi che usano riferimenti a `self`, il compilatore assume sempre che il riferimento da ritornare abbia tempo di vita uguale a `self`.

# Chiusure

Il paradigma del **functional programming** in Rust introduce il concetto di **funzioni di ordine superiore**, ovvero funzioni in cui un parametro sia a sua volta una funzione.
Questo richiede che, in qualche modo, le funzioni siano trattate come un qualsiasi tipo di dato: in Rust è possibile usando un puntatore a funzione, oppure assegnare un valore che **implementa un tratto** Fn, FnOnce, FnMut.

Nel caso di puntatore a funzione avrò una sintassi del genere:

```rust
fn f1(i: i32, d: f64) -> f64 {
	return i as f64 * d;
}

let ptr: fn(i32, f64) -> f64;

ptr = f1; //assegno il puntatore

ptr(2, 3.14); // chiamo la funzione
```

### Funzioni lambda

Per utilizzare le lambda function in Rust ci serviamo della seguente sintassi:

```rust
let f = |x| {x +1}
```

Ovvero racchiudiamo i parametri all'interno di `| |` e il corpo della funzione in parentesi graffe.
Una volta definita la funzione e assegnata a una variabile, è possibile utilizzare la lambda function come una normale funzione, ad esempio `f(5)`.
Possiamo inoltre passare la lambda come argomento di funzione, o come valore di ritonrno di una funzione:

```rust
fn doSomething() -> fn(i32) -> i32 {
	return |x| {x +1} ;
}
```

Le variabili all'interno delle lambda function sono dette variabili **libere**; la funzione lambda prende il nome di **chiusura**.
Di fatto il compilatore traduce una chiusura in una tupla, con tanti campi quanti sono le variabili libere, e che implementa uno o più tratti Fn, FnMut, FnOnce.

E' necessario distinguere fra lambda function che accedono in sola lettura o in modifica: nel secondo caso la funzione lambda andrà dichiarata come mutabile `mut`.
Possiamo inoltre dichiarare funzioni lambda che **assumono il possesso dei valori contenuti nelle variabili libere**: lo facciamo tramite la keyword `move`.

![[Pasted image 20240422125213.png]]

Inoltre Rust mette a disposizione 3 tratti che possono essere implementati **solamente dalle chiusure**:

![[Pasted image 20240422132019.png]]

FnOnce viene implementato se la chiusura consuma uno o più valori durante la sua esecuzione: può essere invocata solamente una volta.
FnMut viene implementato se la chiusura prende possesso esclusivo di valori, potrà essere chiamata più volte.
Fn viene implementata solamente se la chiusura accede ai valori in sola lettura, pertanto sarà possibile che venga chiamata più volte.

### Funzioni di ordine superiore

In Rust è possibile definire delle funzioni di ordine superiore, ovvero funzioni in cui un parametro è un'ulteriore funzione:

```rust
fn higher_order_function<F, T, U>(f: F) where F: Fn(T) -> U {
	// … codice che usa f(…)
}
```

Il tratto che F deve implementare dipenderà da che tipo di funzione vogliamo scrivere.

Next: [[Gestione degli errori]]