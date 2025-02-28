# Polimorfismo

Il **Polimorfismo** è la capacità offerta dai linguaggi di associare comportamenti comuni ad un insieme di tipi differenti. Spesso viene implementato con il paradigma della programmazione generica, oppure tramite l'uso di interfacce o di ereditarietà.

# Tratti

In Rust, per implementare il polimorfismo, si utilizza il concetto di **tratto** (equivalente delle interfacce Java/C++): quest'ultimo esprime la capacità di un tipo di eseguire una certa funzionalità. Ad esempio se un tipo implementa `std::io::Write` allora può scrivere dei byte, mentre se implementa `std::iter:Iterator` può generare sequenze di valori.
A differenza di C++, la chiamata a funzione relativa a un tratto **non ha costi aggiuntivi** (si hanno costi nel caso della creazione di un puntatore dinamico).

Per **definire un tratto** si usa la seguente sintassi:

```rust
trait T1 {
	fn returns_num() -> i32;
	fn returns_self() -> Self;
}
```

Posso poi far implementare un tratto a un certo tipo con la seguente sintassi:

```rust
impl T1 for SomeType {
	fn returns_num() -> i32 { 1 }
	fn returns_self() -> Self {SomeType}
}
```

Per chiamare un metodo relativo a un tratto si deve usare la notazione con il punto `.`, a condizione che il tratto sia dichiarato nello stesso crate, oppure che lo si abbia importato tramite `use SomeNamespace::SomeTrait`.
E' possibile indicare nella definizione del tratto un'implementazione di default.

I puntatori ai tipi tratto vengono detti **oggetti-tratto**: possono essere condivisi o mutabili e devono rispettare le regole dell'esistenza in vita di un valore a cui fanno riferimento.
Sono implementati come Fat pointer.
Variabili o parametri destinati a contenere puntatori (riferimenti, Box, Rc, …) ad un
valore che implementa un tratto sono annotati con la parola chiave `dyn`

E' possibile organizzare i tratti in gerarchie tramite la keyword `Subtrait`: i tipi che vogliono implementare un subtrait devono implementare anche il tratto supertrait

![[Pasted image 20240408134718.png]]

### Tratti nella libreria standard

Supponiamo di dover confrontare due struct 'Cerchio' in Rust: il compilatore non può sapere in quale caso due cerchi sono considerati uguali, perciò è necessario implementare i tratti `Eq` o `PartialEq`. Facendo ciò sarà possibile confrontare due cerchi con `==` o `! =`.
Per gerarchie di ordine (per usare `< > >= <=`) allora dobbiamo implementare il tratto `Ord` o `PartialOrd`.
Tutti gli altri operatori binari (`+ - * / % & ^ << >>`) sono associati a uno specifico tratto (`Add, Sub, Mul, Div, Rem, BitAnd, BitXor, Shl, Shr`), mentre `- !` corrispondono a `Neg, Not`
Per le proprietà matematica occorre garantire 3 proprietà: **Riflessività, simmetria, transitività** 
# Tipi generici

Tramite i **generics** in Rust è possibile descrivere delle strutture dati e funzioni che contengono dati il cui tipo è rappresentato da una **meta-variabile**.

Possiamo definire funzioni generiche nel seguente modo:

```rust
fn max<T>(t1:T, t2:T) -> T where T: Ord {
	return if t1<t2 {t2} else {t1}
}
```

Con la clausola `where` possiamo indicare i **tratti obbligatori** che il parametro generico T deve rispettare.
Possiamo anche esplicitare subito i tratti obbligatori:

```rust
fn max<T: Ord>(t1:T,t2:T) -> T {
	...
}
```

Next: [[Lifetime]]