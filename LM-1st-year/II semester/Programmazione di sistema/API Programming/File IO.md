# File system

Spesso dobbiamo offrire persistenza di dati, quindi sono necessari i **file**.
Rust tramite la sua libreria standard mette a disposizione delle funzioni per **navigare agilmente il file system** di un sistema operativo.
Per Rust un file aperto in lettura e/o scrittura è rappresentato dalla struct `std::fs::File`.

Per quanto riguarda i percorsi, le cui implementazioni dipendono dal sistema operativo in uso, posso essere gestiti con `std::fs::Path` e `std::fs::PathBuf` che sono rispettivamente:
- **unsized** ed accessibili in sola lettura
- Possessori del loro contenuto e modificabili

I seguenti metodi sono disponibili dal crate di Rust:

```rust
use std::fs;

fn read_dir(dir: &Path) -> Result<ReadDir>

fn create_dir(dir: &Path) -> Result<()> 

fn remove_dir(dir: &Path) -> Result<()>

fn copy(from: &Path, to: &Path) -> Result<i64>

fn rename(from: &Path, to: &Path) -> Result<()>

fn remove_file(path: &Path) -> Result<()>
```

# File

La struct File offre due metodi per aprire o creare un file:

```rust
fn open(path: P) -> Result<File> where P: AsRef<Path>

fn create(path: P) -> Result<File> where P: AsRef<Path>
```

Possiamo anche usare le funzioni contenute nel crate `std::fs::OpenOption` che permettono una gestione avanzata dei permessi.

Per leggere e scrivere un file sono disponibili le funzioni `write` e `read_to_string` che però possono essere usate per leggere file di piccole dimensioni. Spesso vogliamo lavorare con file di notevoli dimensioni, quindi occorre una funzione apposita: possiamo usare la **`BufReader`**, che permette di leggere un file riga per riga.

```rust
let input = File::open(path);
let buffered = BufReader::new(input);

for line in buffered.lines() {
	println!("{}", line?);
}
```

La lettura e scrittura da file è anche implementata tramite diversi **tratti** in Rust:

![[Pasted image 20240506122428.png]]

In particolare l'implementazione del tratto Read offre i seguenti metodi:

![[Pasted image 20240506123616.png]]

Per la lettura di contenuti strutturati si può usare un crate esterno, chiamato **Serde**.

