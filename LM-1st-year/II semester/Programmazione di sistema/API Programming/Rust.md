# Rust

Rust è focalizzato sulla **correttezza**, **velocità** e **programmazione concorrente**.
E' fortemente e staticamente tipizzato, quindi le variabili sono associate a un tipo per tutta la loro vita.

Tramite i puntatori di Rust **non sono possibili** una serie di problematiche legate a C/C++:
- Dangling pointer
- Doppio rilascio
- Corse critiche
- Buffer Overflow
- Iteratori invalidi
- Overflow aritmetici

Il linguaggio introduce il concetto di **possesso di un valore**: in ogni istante un valore è posseduto da una e una sola variabile.
E' possibile passare i parametri tramite il **movimento**, inoltre è possibile fornire temporaneamente l'accesso a un valore a patto che non venga modificato.

### Variabili

Per introdurre le variabili si usa la keyword `let`: di base ciò che dichiaro con let è **immutabile** e non va modificata, mentre se uso anche la keyword `mut` la rendo mutabile.
Il concetto di immutabilità riguarda anche le strutture dati, che se dichiarate solamente con let non potranno essere popolate.

`mut` significa **mutualmente esclusivo**, ciò significa che solamente uno per volta può modificare il valore, e gli altri non ci possono leggere.

A differenza del C/C++ i costrutti come gli `if` ritornano il valore del ramo vero o del ramo falso. L'unica eccezione è l'assegnazione che non ritorna nulla (non si possono fare le triple assegnazioni).

### Tipi e tratti

Rust offre diversi tipi predefiniti come i tipi **elementari** (i32, i64, f32, f64..., bool), tuple, stringhe, array, slice, never e diversi puntatori. I **caratteri** sono Unicode su 32 bit, mentre le stringhe fanno uso della utf-8
E' possibile definire nuovi tipo con struct, enum...
I tipi **non sono organizzati in gerarchie**.
Un tipo particolare è il **unit** che si indica con `()`, corrisponde di fatto al void di C/C++

Ad ogni tipo sono associati zero o più **tratti**: sono simili alle interfacce, quindi insieme di metodi non implementati o con implementazione di default che un oggetto che implementa quel tipo può eseguire. Si dice quindi che un certo oggetto **gode** di un certo tratto.

### Tuple

Sono collezioni ordinate di valori eterogenei: possono avere fino a $n$ campi a cui è possibile accedere con `.<numero_campo>` quindi ad esempio:

```rust
let t:(i32, bool) = (123, false);
let i = t.1
let j = t.0
```

### Riferimenti

Il primo modo per rappresentare degli indirizzi di memoria è tramite l'uso dei **riferimenti**: tramite l'operatore `&` posso creare riferimenti a qualsiasi valore.

```rust
let r1 = &v; //v è sia un valore che una espressione (es. &75)
```

La variabile r1 prende **in prestito** il valore v e potrà accedervi **in sola lettura**.
Si parla anche di puntatori senza possesso.
Di puntatori in sola lettura se ne possono concedere quanti ne si vuole, ma mentre un riferimento è in vita il valore **non è modificabile**. Il compilatore ha il compito di tracciare il tempo di vita dei riferimenti e permettere la modifica dei valori se non esistono riferimenti validi.
Il compilatore deve inoltre garantire che il valore originale viva più a lungo dei riferimenti.

E' possibile dichiarare **riferimenti mutabili**:

```rust
let r2 = &mut v;
```

Il riferimento mutabile r2 prende il **possesso esclusivo**, quindi il possessore è in grado di leggere e scrivere. Mentre un riferimento mutabile è in vita, il valore è inaccessibile in lettura e scrittura, e non si possono creare altri riferimenti (in altre parle il riferimento mutabile è unico).
Notiamo che chi riceve un riferimento non ha il compito di rilasciare la memoria, dato che sarà compito del proprietario.

Di fatto i riferimenti implementano la logica **multiple readers & single writer**.

### Box\<T\>

Normalmente le variabili locali risiedono nello stack: in alcuni casi non è nota la dimensione del dato, oppure in altri casi bisogna prolungare la vita di una variabile.
Per fare queste operazioni si usa una **box**: di fatto è un puntatore che **possiede** la sua variabile, la quale viene allocata nello **heap**.
E' possibile allocare un box con il seguente codice:

```rust
let b = Box::new(v);
```

Si accede `v` dereferenziando con l'operatore `*b`.

```ad-note
title: Notazione .
Quando accediamo a un box o a un puntatore non è necessario usare l'asterisco se poi usiamo una dot notation sul puntatore.
Infatti questa notazione guarda sempre a sinistra e dereferenzia fino a quando non trova un valore.
```

Quando la box cessa la sua esistenza, il compilatore rilascia la memoria.

![[Pasted image 20240308084625.png]]

Per **stampare in rust** esistono due tratti: debug e display. Il tratto display stampa in modo utile ad un utente finale (equivale al toString in java) , mentre il tratto debug stampa i dati così come sono, in modo da essere comprensibili dal programmatore.

```rust
println!("{}", 34) //Stampa con tratto display

println!("{:?}", *b) //Stampa con tratto debug
```

Da notare che in rust, l'assegnazione corrisponde a uno **spostamento di possesso tra variabili**: nell'esempio sotto, la responsabilità del rilascio passa da r a b.

![[Pasted image 20240308090129.png]]

Il tratto **copy** permette di copiare un riferimento e trasferirne diritti e doveri.
Il tratto **clone** permette di duplicare effettivamente un oggetto: nel caso di un box andrò ad allocare una nuova area con un nuovo puntatore (oneroso).

### Puntatori nativi

Sono equivalenti ai puntatori di C/C++ e ne condividono tutti i problemi.
Per deferenziarli e accedervi sia in lettura e/o scrittura occorre trovarsi in un blocco **unsafe**.

```
const* T
mut* T
```

### Array

In rust gli array sono sequenze di oggetti **omogenei**, disposti consecutivamente nello stack.
Ogni array ha un tipo nel formato `:[T, length]` e la dimensione è nota a priori.
Per dichiararli abbiamo diverse scelte:

```rust
let a: [i32, 5] = [1,3,6,2,5];

let b = [0;5] // [0,0,0,0,0]

let l = b.len() // 5
let e = a[3] // 2
```

A differenza del C dove il nome del puntatore corrisponde al primo elemento, in rust gli array sono veri e propri tipi, quindi è sempre nota la lunghezza. Attenzione: la lunghezza di un array è **immutabile**.

### Slice

E' possibile fare riferimento a porzioni di valori consecutivi in memoria:  questa struttura è detta **slice** ed è un tipo composto da:
- Il riferimento al primo elemento dello slice
- Il numero di elementi da tenere in considerazione
Per la sua natura è anche detto **fat pointer**.

```rust
let a = [1,2,3,4];

let s1: &[i32] = &a //s1 conterrà tutti i valori di a
let s2 = &a[0..2] //s2 conterrà 1, 2
let s3 = &a[2..] //s3 conterrà 3, 4
```

Di base una slice è **immutabile**, ma si ottiene la possibilità di modificare il contenuto con `let ms = &mut a[..]`

Nel caso di accessi **out of bounds**, il compilatore potrebbe lasciare compilare, ma a runtime il programma va in **panic**.

### Vec\<T\>

Il Vec\<T\> rappresenta una **sequenza ridimensionabile** di elementi di tipo T, allocati sullo **heap**. In pratica è una tupla con 3 valori: il primo è il **puntatore a una zona dello heap**, poi il secondo indica la **dimensione complessiva del buffer**, mentre il terzo valore è la **dimensione del buffer occupato**.
Quando memorizzo (push) un nuovo valore in vec, allora inserisco il valore nella prima posizione libera, e successivamente incremento la dimensione utilizzata del buffer.

```rust
let mut v: Vec<i32> = Vec::new();
```

![[Pasted image 20240308095140.png]]

Alla chiusura dello scope, viene invocato il distruttore del vec, andando a liberare lo heap.

### String

Esistono due rappresentazioni:
- **Tipo primitivo `str`**: array di caratteri immutabili memorizzati in Unicode.
- Oggetto **allocato dinamicamente**, usando il tipo `String`.
Per indicare costanti di tipo stringa uso i doppi apici `""`.
Si accede ai tipi primitivi stringa solamente tramite slice `&str`: possiamo inoltre usare lo slice per manipolare i buffer dinamici del tipo `String`, perciò esiste completa interoperabilità fra i due formati.

Dall'immagine notiamo la differenza fra le due opzioni:

![[Pasted image 20240308102628.png]]

Per dichiarare oggetti di tipo **String** usiamo il seguente codice:

```rust
let s0 = String::new();
let s1 = String::from("Hello World");
let s2 = "Some text".to_string();

//Per convertire da String a str
let s: &str = s1.as_str();
```

Sono disponibili numerosi metodi per entrami i modi di rappresentazione.

### Uso del `match`

![[Pasted image 20240308110459.png]]

### Riga di comando

Possiamo accedere agli argomenti passati da riga di comando:

```rust
use std::env::args
fn main(){
	let args: Vec<String> = args().skip(1).collect();
	if (args.len() > 0){
		//do stuff with args
	}
}
```

Nella realtà questo metodo si usa raramente, si usa invece una libreria della community chiama **clap**.

### I/O da console

Il **crate** (package) `std::io` contiene la definizione delle strutture dati per accedere ai flussi standard di ingresso o uscita: le operazioni restituiscono delle **monadi** che sono dei `Result<T,Error>` che incapsula il valore attesa in caso di successo o il tipo di errore per il fallimento.

Per verificare la correttezza di una monade usiamo il metodo `is_ok()` e poi posso accedervi con `unwrap()`.

![[Pasted image 20240308111831.png]]

### Convenzione sui nomi

- Per strutture legate ai **tipi** (struct, enum, tratti...) si usa **UpperCamelCase**.
- Per strutture legate ai valori (variabili, funzioni, metodi) si usa lo **lower_snake_case**.

Ora si analizza in dettaglio il [[Possesso]] di Rust.