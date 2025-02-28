# Struct in Rust

In Rust è possibile definire delle struct, ovvero zone di memoria in cui i vari campi hanno un tipo deciso dal programmatore: spesso si vogliono mantenere insieme delle informazioni eterogenee.

Per dichiarare una struct in Rust usiamo la keyword `struct`:

```rust
struct Player{
	name: String,
	health: i32,
	level: u8
}
```

Per convenzione le struct si nominano in **CamelCase**, mentre per i campi si segue lo **snake_case**.
Posso creare un'istanza di una struct andando a specificare i valori del campi:

```rust
let p1 = Player{name:"Mario".to_string(), health: 25, level:1};
let mut p2 = Player{name, health, level};

//Posso anche dichiarare una struct derivata da un'altra, andando a sovrascrivere ai campi che mi interessano

let p3 = Player{name:"Luca".to_string(), ..p1}
```

Per **accedere ai campi** di una struct usiamo la notazione puntata.
Per modificare i campi occorre che la variabile sia mutabile.

Un secondo modo per definire delle struct è omettere i nomi dei campi, andando a definire le struct con delle tuple:
E' possibile anche dichiarare delle struct "vuote".

```rust
struct Playground(String, i32, i32);
struct Empty;

let mut f = Playground("test".to_string(), 23, 43);
let e = Empty;
```

### Rappresentazione in memoria

In Rust l'ordine di dichiarazione di una struct e la conseguente disposizione in memoria non coincidono: infatti è il compilatore a decidere come inserire i dati in memoria.

![[Pasted image 20240319131932.png]]

In realtà l'ordine è scelto in modo da garantire i **vincoli di allineamento** che cambiano in base al tipo di dato. Per conoscere l'allineamento di un tipo di dato posso usare `std::mem::align_of_val()`.

Per modificare la disposizione dei campi in memoria posso anteporre alla dichiarazione della struct la riga `#[repr(C)]` per obbligare il compilatore a fare come in C, ovvero disporre in memoria nello stesso ordine della dichiarazione.

### Visibilità

Sia la struct che i suoi campi possono essere preceduti dalla parola chiave `pub` che ne modifica la visibilità.
Di default Rust considera i campi di una struct come privati (diverso da Java), ovvero appartenenti al **modulo** in cui sono dichiarati.
Questo permette di implementare il meccanismo di incapsulamento (**information** **hiding**).

```ad-note
title: pub struct
Mettere una struct pubblica non implica la possibilità di vederne i campi: infatti posso rendere pubblica la struttura, mantenendo i campi privati, poi posso definire i meotodi per manipolare i dati.
```

### Metodi

Rust non possiede il concetto di classe, perciò per definire dei comportamenti associati a dei dati personalizzati, devo farlo in un blocco a parte, usando la parola chiave `impl`.

```rust
struct Point {
	x: i32,
	y: i32,
}

impl Point {
	fn mirror(self) -> Self {
		Self{ x: self.y, y: self.x }
	}
	
	fn length(&self) -> i32 {
		sqrt(self.x*self.x + self.y*self.y)
	}
	
	fn scale(&mut self, s: i32) {
		self.x *= s;
		self.y *= s;
	}
}
```

Per chiamare un metodo possiamo usare la sintassi `<Tipo>::<metodo>`.

Da notare che il tipo di `self` passato è differente a seconda di cosa fa il metodo:
- Se passo `elf` allora significa che il metodo **consumerà** la struttura che gli è stata passata, quindi quella struttura originale non sarà più accessibile
- Passando `&self` abbiamo un riferimento condiviso, quindi posso accedere in lettura ai campi della struct, ma non modificarla
- Passando `&mut self` abbiamo un riferimento esclusivo, dunque il metodo potrà anche modificare la struct

E' possibile che un metodo non abbia come argomento self, ciò significa che è un metodo statico e quindi potrebbe svolgere la funzione di **costruttore**.
Queste funzioni prendono il nome di **funzioni associative**.
### Costruttori, Distruttori

In Rust non esiste il concetto di costruttore: ogni zona del codice con visibilità di una struct può creare un'istanza. Siamo però liberi di implementare un metodo che ritorna `Self`.
Normalmente lo chiamiamo `new`, e se dobbiamo inizializzare più "costruttori" dobbiamo dare nomi diversi (non esiste overloading) che seguano un pattern del tipo `with_<details>`

```rust
pub fn new () -> Self {
	//do stuff
}

pub fn with_details() -> Self{
	//do stuff
}
```

```ad-summary
title: RAII
In C++ esiste un paradigma detto **Resource Acquisition Is Initialization**: indica che un oggetto che possiede qualcosa ha la responsabilità di rilasciarlo.
Grazie a questo approccio sono nate (in C++) delle classi formate solo da un costruttore e un distruttore, ma senza altre informazioni.
La funzionalità sta nel fatto che, anche non usando la classe, posso far fare delle cose al costruttore e al distruttore, avendo la garanzia sulla loro esecuzione, dato che ho definito l'oggetto.
```

Il concetto di distruttore non esiste in Rust, ma posso emularlo andando a personalizzare il **tratto DROP**: 

```rust
pub struct Shape {
	pub position: (f64,f64),
	pub size: (f64,f64),
	pub type: String
}

impl Drop for Shape {
	fn drop(&mut self){
		//do something
	}
}
```

Il metodo drop è l'ultima azione eseguita da una struct nel suo ciclo di vita (se implementato).
**Attenzione**: il tratto DROP è **mutualmente esclusivo** con il tratto COPY, questo perchè si vuole garantire il legame tra una singola creazione e una singola distruzione (approccio RAII).
### Enum

In Rust, è possibile definire tipi enumerativi composti da un semplice valori scalare.
E' possibile però **associare a una valore delle strutture** per memorizzare informazioni aggiuntive.

```rust
enum HttpResponse {
	Ok=200,
	NotFound=404,
	InternalError=500
}

println!("200 == {}", HttpResponse.Ok);


//Associazione di strutture a una enum
enum HttpResponse {
	Ok,
	NotFound(String),
	InternalError {
		desc: String,
		data: Vec<u8> },
}
```

Per rappresentarle in memoria, le enum hanno un byte per il tag (il numero), poi il campo di dimensione maggiore (se presente):

![[Pasted image 20240319142319.png]]

L'uso degli enum si combina con l'uso del match, come nell'esempio seguente:

```rust
enum Shape{
	Circle {r:f64},
	Square {s: f64},
	Rectangle{h:f64, w:f64}
}

fn compute_shape(shape : Shape) -> f64{
	match shape {
		Square{s} => s*s,
		Circle{s} => r*r*3.14,
		Rectangle{h,w} => h*w
	}
}
```

### Destrutturazione

E' possibile l'uso del seguente costrutto per verificare se un oggetto corrisponda a un certo pattern indicato: 

```rust
enum Shape{
	Circle {r:f64},
	Square {s: f64},
	Rectangle{h:f64, w:f64}
}

fn process(shape: Shape) {
	// stampa solo se shape è Square…
	if let Square { s } = shape {
		println!("Square side {}", s);
	}
}
```

### Enumerazioni generiche

Rust offre due importanti enumerazioni generiche, che sono alla base della sua
libreria standard
-  Option\<T\>: rappresenta un valore di tipo T opzionale (ovvero che potrebbe non esserci)
- Result<T,E> - rappresenta alternativamente un valore di tipo T o un errore di tipo E

Option\<T\> contiene due possibili valori:
- Some(T): indica la presenza e contiene il valore
- None: indica che il valore è assente

Next: [[Tratti e Tipi generici]].