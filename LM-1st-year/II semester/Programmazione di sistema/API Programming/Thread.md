# Thread in Rust

- Si crea un thread nativo in Rust attraverso la funzione `std::thread::spawn(...)`
  - Accetta una funzione lambda che rappresenta la computazione che il thread deve svolgere
  - Ritorna una struct di tipo `std::thread::JoinHandle<T>`, dove `T` rappresenta il tipo restituito dalla computazione del thread (ovvero il tipo ritornato dalla funzione lambda)
- Per sapere quando la computazione del thread è terminata e quale valore abbia prodotto, occorre utilizzare il metodo `join()` offerto dalla handle
  - Restituisce un’enumerazione di tipo `std::thread::Result` che contiene, nell’opzione `Ok`, il valore finale e nell’opzione `Err` il valore eventualmente passato alla macro `panic!`, nel caso in cui sia stata invocata nel corso della computazione del thread stesso
- Non si crea nessun rapporto di parentela tra thread creatore (quello in cui si invoca `spawn(...)`) e thread creato
  - Né occorre che l’uno sopravviva all’altro
  - Quando l’handle di un thread esce dallo scope e viene rilasciata, non c’è più modo di avere notizie (dirette) sull’esito del thread creato, che acquisisce lo stato detached
# Configurare un thread

- E’ possibile configurare un thread prima di lanciare la sua esecuzione tramite la struct `std::thread::Builder`
  - Permette di assegnare al thread un nome a scelta e di definire la dimensione dello stack da associare al thread
  - Il metodo `spawn(...)` consuma l’oggetto Builder, crea il thread corrispondente e restituisce un enum di tipo `io::Result<JoinHandle>`

# I tratti della concorrenza

- Nell’ambito del proprio sforzo di garantire la correttezza degli accessi alla memoria e l’assenza di comportamenti non definiti, Rust introduce due tratti marcatori (senza metodi), il cui scopo è fornire indicazioni sul comportamento di un tipo in un contesto multi-thread
  - Il tratto `std::marker::Send` è applicato automaticamente a tutti i tipi che possono essere trasferiti in sicurezza da un thread ad un altro, ovvero in grado di garantire che non è possibile avere accessi al loro contenuto contemporaneamente
  - Il tratto `std::marker::Sync` è applicato automaticamente a tutti i tipi `T` tali che `&T` risulta avere il tratto `Send`, ovvero che possono essere condivisi in sicurezza tra thread differenti, senza creare problemi di comportamenti non definiti
- Puntatori e riferimenti non hanno il tratto `Send` in quanto l’esecuzione indipendente dei thread non consente al borrow checker di fornire le proprie garanzie di correttezza

### I tratti della concorrenza 

- `pub unsafe auto trait Send { }`
  - Se un tipo dispone del tratto Send, è lecito passarlo per valore ad altri thread
  - L’uso del movimento (o della copia, là dove possibile) garantisce la non contemporaneità degli accessi
- I tipi composti (struct, tuple, enum, array) godono del tratto `Send` se tutti i loro campi lo posseggono
  - E’ possibile forzare l’assegnazione/rimozione di tale tratto solo all’interno di un blocco `unsafe`: il programmatore deve essere conscio della scelta adottata e farsi carico della relativa responsabilità
- `pub unsafe auto trait Sync { }`
  - Se un tipo dispone del tratto Sync, è lecito passarlo come riferimento non mutabile ad altri thread o, in altre parole, un tipo è Sync se è lecito accedervi in modo concorrente a partire da un riferimento non mutabile
  - I tipi che implementano una mutabilità interna (come `Cell` e `RefCell`) non dispongono di questo tratto

![[Pasted image 20240521134914.png]]

E’ possibile creare thread solo se i dati catturati dalla funzione lambda che ne descrive la computazione e il suo tipo di ritorno hanno il tratto `Send`
In mancanza di questa garanzia, il borrow checker genera un errore di compilazione, rilevando la propria impossibilità a garantire la correttezza di quanto si sta chiedendo di eseguire


# Modelli di concorrenza

La libreria standard di Rust supporta due modelli base per la realizzazione di programmi concorrenti:

1. La condivisione di dati basata su sincronizzazione degli accessi ad una struttura dati condivisa, a cui tutti i thread interessati possono accedere in lettura e scrittura.
2. La condivisione di dati basata sullo scambio di messaggi che prevede uno o più mittenti ed un solo destinatario.

Sono inoltre disponibili librerie esterne che supportano ulteriori modelli:
- La libreria `actix` supporta il modello degli attori.
- La libreria `rayon` supporta il modello work stealing.
- La libreria `crossbeam` permette la condivisione di dati memorizzati nello stack del thread genitore con i thread figli.


# Condivisione dello stato

- Per poter condividere dati modificabili tra thread differenti, occorre disporre di un qualche meccanismo che permetta, ad un solo thread alla volta, di acquisire il permesso di modifica
  - Bloccando lo svolgimento degli altri thread che dovessero richiedere accesso alla stessa risorsa
- Il modo più semplice di ottenere questo comportamento è attraverso l’uso di un mutex (MUTual EXclusion lock)
  - Costrutti di questo tipo sono offerti nativamente dai sistemi operativi e sono riesportati in modo indipendente dalla piattaforma dalle librerie standard C++ e Rust
- Gli oggetti nativi offerti dai sistemi operativi offrono, come minimo, due metodi: `lock()` e `unlock()`
  - Invocando `lock()`, un thread richiede il possesso del mutex: se questo non può essere garantito al momento, perché il mutex è in uso ad un altro thread, l’invocazione si blocca fino a che il mutex non è stato rilasci

![[Pasted image 20240521134843.png]]

L’accesso ad uno **stato condiviso** in Rust richiede l’utilizzo di due blocchi in cascata:
- Il primo volto a permettere il possesso multiplo di una struttura dati in sola lettura da parte di più thread, realizzato mediante il costrutto `std::sync::Arc<T>`
- II secondo che consente l’acquisizione in lettura/scrittura della struttura dati, realizzato
alternativamente mediante il costrutto `std::sync::Mutex<T>`, con `std::sync::RwLock<T>` oppure ricorrendo ai tipi atomici

In Rust, un oggetto di tipo `Mutex<T>` incapsula un dato di tipo `T` e un riferimento a un mutex nativo del sistema operativo. Questo garantisce l'accesso sicuro ai dati condivisi tra più thread.

L'unico modo per accedere ai dati incapsulati in un `Mutex<T>` è invocare il metodo `lock()`. Questo metodo:
- Restituisce un oggetto di tipo `LockResult<MutexGuard<T>>`.
- Resta bloccato finché non è possibile acquisire il mutex nativo.
- Può restituire un errore se il mutex si trova nello stato avvelenato (ad esempio, se l'ultimo thread che ha acquisito il mutex è terminato prima di averlo rilasciato).

Se `lock()` ha successo, la risposta contiene un `MutexGuard<T>`. Questo oggetto:
- Implementa il tratto `Deref<T>`, comportandosi come uno smart-pointer.
- Può essere dereferenziato per ottenere un riferimento mutabile al dato `T`.
- Rilascia il mutex nativo quando esce dallo scope, permettendo ad altri thread di acquisirlo.

In pratica, `MutexGuard<T>` implementa il pattern RAII (Resource Acquisition Is Initialization). Questo significa che il mutex viene automaticamente rilasciato quando il `MutexGuard<T>` viene distrutto, garantendo una gestione sicura e automatica delle risorse

I mutex possono avere un solo possessore e per questo vengono sempre incapsulati in oggetti di tipo `Arc`:
```rust
let m = Arc::new(Mutex::new(T));
```

### Condition variable

Per **condition variable** si intendono Strutture dati di sincronizzazione che permettono di  bloccare l’esecuzione di un thread, così da evitare il consumo di CPU, nell’attesa che qualcosa succeda.
Il pattern di utilizzo prevede che esista una espressione booleana il cui valore possa essere usato per determinare se occorre attendere o meno:
- La valutazione di tale espressione deve avvenire mentre si possiede un mutex, per garantire l’assenza di corse critiche
- In Rust, questo vuol dire che le variabili che consentono la valutazione della condizione sono incapsulate nel mutex

Ogni condition variable **deve essere usata in coppia con un singolo mutex**.

![[Pasted image 20240522180941.png]]

Quando un thread esegue una wait viene **sospeso e aggiunto alla lista d'attesa**: quando invece chiamo una notify allora sblocco uno o più thread bloccato (nel caso di un solo thread, non c'è determinismo).
All'uscita della wait è sempre **necessario controllare la condizione bloccante**: può essere fatto in modo diretto o usando il metodo `wait_while`.

# Condivisione di Messaggi in Rust

Rust offre un meccanismo di comunicazione e sincronizzazione tra thread basato sulla condivisione di messaggi, come alternativa alla condivisione dello stato.

#### Canali Asincroni (`mpsc`)

La funzione `std::sync::mpsc::channel<T>()` restituisce una coppia formata da una struct `Sender<T>` e una struct `Receiver<T>`.

- **Invio e Ricezione dei Messaggi**:
  - `send(...)`: Metodo per inviare dati attraverso il canale. Non blocca il chiamante.
  - `recv()`: Metodo per ricevere dati. Si blocca fino a quando non è disponibile un messaggio o il `Sender` è stato terminato.

- **Proprietà**:
  - **Multiple Producer, Single Consumer**: Permette di creare più cloni dell'oggetto `Sender`, ma obbliga ad avere una singola copia dell'oggetto `Receiver`.

- **Errore**:
  - Se il `Receiver` viene deallocato, gli invii falliscono con `SendError<T>`.
  - Se tutti i trasmettitori vengono deallocati, le letture falliscono con `RecvError`.

##### Esempio di Utilizzo

```rust
use std::sync::mpsc::channel;
use std::thread;

let (tx, rx) = channel();
for _ in 0..3 {
    let tx = tx.clone();
    thread::spawn(move || {
        tx.send("ok").unwrap();
    });
}

drop(tx); // Chiude l'ultimo sender per evitare che `rx` aspetti all'infinito.

while let Ok(msg) = rx.recv() {
    println!("{}", msg);
}
```

#### Canali Sincroni (`sync_channel`)

La funzione `std::sync::mpsc::sync_channel<T>(bound: usize)` restituisce una coppia di valori di tipo `(SyncSender<T>, Receiver<T>)`.

- **Proprietà**:
  - Limitato: se il numero di messaggi nel canale raggiunge il limite definito (`bound`), `send(...)` diventa bloccante fino a quando non si libera un posto.
  - Se il `bound` è 0, diventa un canale di tipo `rendezvous`, dove ogni operazione di lettura deve sovrapporsi temporalmente a una di scrittura.

##### Esempio di Utilizzo

```rust
use std::sync::mpsc::sync_channel;
use std::thread;

let (sender, receiver) = sync_channel(1);
sender.send(1).unwrap(); // Questo ritorna immediatamente

thread::spawn(move || {
    sender.send(2).unwrap(); // Questo si blocca fino a quando il messaggio precedente è ricevuto
});

assert_eq!(receiver.recv().unwrap(), 1);
assert_eq!(receiver.recv().unwrap(), 2);
```

### La Libreria Crossbeam

Crossbeam è una libreria ben documentata che offre costrutti a supporto dell'elaborazione concorrente.

- **Costrutti Atomici**: `crossbeam::atomic::AtomicCell<T>` estende la mutabilità interna a contesti multithread, utilizzando primitive atomiche o un lock interno.
- **Strutture Dati Concorrenti**: `crossbeam::deque` e `crossbeam::queue` implementano code di messaggi basate sul paradigma multiple-producer-multiple-consumer.
- **Canali MPMC**: `crossbeam::channel::{bounded(...), unbounded()}` creano canali unidirezionali con capacità limitata o illimitata.

#### Esempi di Pattern Concorrenti

##### Fan-Out / Fan-In

Distribuisce attività a più thread indipendenti e raccoglie i risultati in un singolo punto.

```rust
use crossbeam::channel::bounded;
use std::thread;

fn worker(id: usize, rx: Receiver<i32>, tx: Sender<String>) {
    while let Ok(value) = rx.recv() {
        tx.send(format!("W{} ({})", id, value)).unwrap();
    }
}

fn main() {
    let (tx_input, rx_input) = bounded::<i32>(10);
    let (tx_output, rx_output) = bounded::<String>(10);
    let mut worker_handles = Vec::new();

    for i in 0..3 {
        let rx = rx_input.clone();
        let tx = tx_output.clone();
        worker_handles.push(thread::spawn(move || worker(i, rx, tx)));
    }

    for i in 1..=10 { 
        tx_input.send(i).unwrap(); 
    }
    drop(tx_input);
    drop(tx_output);

    while let Ok(result) = rx_output.recv() {
        println!("Received result: {}", result);
    }

    for handle in worker_handles {
        handle.join().unwrap();
    }
}
```

##### Pipeline

Crea una serie di fasi di lavorazione, ciascuna eseguita da un singolo thread.

```rust
use crossbeam::channel::bounded;
use std::thread;

fn stage_one(rx: Receiver<i32>, tx: Sender<String>) {
    while let Ok(value) = rx.recv() {
        tx.send(format!("S1({})", value)).unwrap();
    }
}

fn stage_two(rx: Receiver<String>, tx: Sender<String>) {
    while let Ok(value) = rx.recv() {
        tx.send(format!("S2( {} )", value)).unwrap();
    }
}

fn main() {
    let (tx_input, rx_input) = bounded::<i32>(10);
    let (tx_stage_one, rx_stage_one) = bounded::<String>(10);
    let (tx_output, rx_output) = bounded::<String>(10);

    let stage_one_handle = thread::spawn(move || stage_one(rx_input, tx_stage_one));
    let stage_two_handle = thread::spawn(move || stage_two(rx_stage_one, tx_output));

    for i in 1..=10 {
        tx_input.send(i).unwrap();
    }
    drop(tx_input);

    while let Ok(result) = rx_output.recv() {
        println!("Received result: {}", result);
    }

    stage_one_handle.join().unwrap();
    stage_two_handle.join().unwrap();
}
```

##### Producer / Consumer

Permette a uno o più thread produttori di generare valori elaborati dal primo thread consumatore disponibile.

```rust
use crossbeam::channel::bounded;
use std::thread;

fn producer(id: usize, tx: Sender<(usize,i32)>) {
    for i in 1..=5 {
        tx.send((id,i)).unwrap();
    }
}

fn consumer(id: usize, rx: Receiver<(usize,i32)>) {
    while let Ok((sender_id, val)) = rx.recv() {
        println!("Consumer {} received {} from {}", id, val, sender_id);
    }
}

fn main() {
    let (tx, rx) = bounded::<(usize,i32)>(10);
    let mut handles = Vec::new();

    for i in 0..3 {
        let tx = tx.clone();
        handles.push(thread::spawn(move || producer(i, tx)));
    }

    for i in 0..2 {
        let rx = rx.clone();
        handles.push(thread::spawn(move || consumer(i, rx)));
    }

    drop(tx);

    for handle in handles {
        handle.join().unwrap();
    }
}
```

### Il Modello degli Attori

Introdotto inizialmente in Erlang, il modello degli attori implementa la concorrenza a livello di tipo usando entità dette attori. Ideato da Carl Eddie Hewitt nel 1973, elimina il bisogno di lock e sincronizzazione fornendo un modo più pulito e lineare di introdurre la concorrenza in un sistema.

- **Attore**: Primitiva principale che contiene una mailbox per i messaggi inviati in modo asincrono.
- **Messaggi**: Incapsulano richieste inviate agli attori, elaborati in modalità FIFO.

#### Libreria `actix`

La libreria `actix` offre un'implementazione di questo modello basata sul framework asincrono Tokio.

- [Repository GitHub di actix](https://github.com/actix/actix)