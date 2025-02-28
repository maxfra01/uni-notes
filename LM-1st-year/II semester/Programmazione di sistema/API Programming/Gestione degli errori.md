# Errori ed eccezioni

Tutte le computazioni possono fallire prematuramente, impendendo che possa essere restituito il valore atteso o che vengano eseguiti tutti gli aspetti collaterali richiesti.
Distinguiamo i malfunzionamenti (tralasciando le molteplici cause) in:
- **Malfunzionamenti recuperabili**: lo stato del programma non è compromesso, è dunque possibile attivare una strategia di ripristino
- **Malfunzionamenti non recuperabili**: viene causata un'alterazione dello stato che implica l'impossibilità di procedere ulteriormente

Nel caso non recuperabile, spesso occorre **terminare il processo** (ed eventualmente attuare una qualche pulizia esterna). E' buona norma che al verificarsi di un errore il controllo del programma, dopo la gestione, al chiamante della funzione.

Rust, a differenza di altri linguaggi, offre l'utilizzo dei tipi `Result<T,E>` e `Option<T>` per esprimere gli esiti delle computazioni. Inoltre è possibile usare la macro `panic!` per far terminare il thread corrente.

# Gestione degli errori in Rust

La gestione degli esiti delle computazioni è modellata con il tipo `Result<T,E>`.

```rust
enum Result<T,E> {
	Ok(T),
	Err(E)
}
```

Per scorporare un tipo Result è possibile usare il costrutto di match:

```rust
fn read_file(name: &str)-> Result<String,io::Error> {
	let r1 = File::open(name);
	let mut file = match r1 {
		Err(why) => return Err(why),
		Ok(file) => file
};

let mut s = String::new();
let r2 = file.read_to_string(&mut s);
match (r2) {
	Err(why) => Err(why),
	Ok(_) => Ok(s),
	}
}
```

Result mette a disposizione diversi metodi per analizzare i risultati:
- I metodi `is_ok(&self)` e `is_err(&self)` permettono, rispettivamente, di determinare se l'esito di un'operazione ha avuto successo o meno
- I metodi `ok(self)` e `err(self)` consumano il risultato trasformandolo in un oggetto di tipo `Option<T>` piuttosto che `Option<E>`
- Il metodo `map(self, op: F)->Result<U,E>` applica la funzione al valore contenuto nel risultato, se questo è ok, altrimenti lascia l'errore invariato
- Il metodo `contains(&self, x: &U)` restituisce vero se il risultato è valido e contiene un valore che equivale all'argomento
- Il metodo `unwrap(self)->T` restituisce il valore contenuto, se è valido, ma invoca la macro `panic!(...)` se il risultato contiene un errore.

Se un Result **contiene un errore** allora bisogna applicare qualche strategia per gestirlo: si può provare a ripetere l'esecuzione, terminare il programma oppure propagare l'errore al chiamante.

La **macro `panic!()`** ha come effetti la terminazione immediata del thread corrente, con **contrazione dello stack** per tutti i tipi che implementano il tratto **Drop**. Se il **thread corrente è il principale** allora il programma termina con un codice di uscita diverso da 0, altrimenti continua.

### Ignorare gli errori

Un'altra pratica comune è usare i costrutti **unwrap ed expect**, che di fatto permettono di ignorare l'errore senza gestirlo.

```rust
let res = do_something().unwrap().expect("Data not founf!");
```

In altre situazione è sufficiente sapere se la computazione è avvenuto o meno, in tali casi si utilizza il tipo `Option<T>` che può restituire un valore effettivamente di tipo `T` oppure il valore `None`.
### Propagare gli errori

