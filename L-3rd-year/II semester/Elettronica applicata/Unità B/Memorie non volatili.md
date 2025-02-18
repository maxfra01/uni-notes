# ROM
---
Le Mask-Programmable ROM sono le più semplici memorie non volatili.
Sono programmate in fabbrica, non cancellabili e servono per memorizzare programmi permanenti, lookup table, boot ecc...

Le celle di una memoria ROM sono di diverso tipo:

![[Pasted image 20230404182657.png]]

Le prime che usano il diodo non sono più usate perchè consumano troppo.
Nella prima versione con i transistor, quando la WL è attiva allora anche la bitline è alta, quando è spenta è tutto zero.
Nella seconda versione il principio è lo stesso: la Bitline quando non c'è niente allora è alta; se invece WL è attiva la BL è a zero.

### NOR based ROM

Le celle MOS ROM 2 viste qui sopra trovano applicazione in questo tipo di memorie:

![[Pasted image 20230404183050.png]]

Attivando una WL posso accedere a tutti i bit di quella linea, e a seconda della presenza del transistor ottengo 0 (se c'è), mentre è 1 se non c'è (grazie al pull-up).

Per esempio alla prima riga ho 1 0 1 1, seconda riga invece 0 1 1 0.

### NAND Based ROM

![[Pasted image 20230404183527.png]]

Stesso funzionamento di prima ma complementare.
Se una wordline è attiva, allora i transistor si chiudono e portano la tensione a terra. Se non sono attivi allora la rete di pullup porta la tensione al valore di Vdd.


# Flash
---
Esse combinano gli approcci di programmazione e cancellazione rispettivamente dalle E2PROM e EPROM.

![[Pasted image 20230405114042.png]]

Le memorie flash hanno le seguenti caratteristiche:
- sono cancellabili elettricamente
- Possono essere programmate o cancellate "in system" senza bisogno di ausilii esterni.
- Sempre per cancellazione e programmazione ci sono On Chip dei generatori di tensione appositi per performare quelle operazioni.
- La **Cancellazione avviene a blocchi** di dimensione variabile per tutte le celle del blocco.

Le FLASH di tipo NAND si usano per memorie di massa (usb, ssd, sd card).
Le FLASH di tipo NOR sono usate per memorie di configurazione (ad esempio nei microcontrollori).

| NOR Flash                                | NAND Flash                           |
| ---------------------------------------- | ------------------------------------ |
| ![[Pasted image 20230405114545.png]]     | ![[Pasted image 20230405114553.png]] |
| Accesso casuale                          | Accesso a **pagine**                 |
| Lettura veloce                           | Dense, più economiche                |
| Scrittura lenta                          | Veloce in scritta e cancellazione    |
| Utili per accessi casuali                | Utili per accessi sequenziali        |
| I/O su bus separati per dati e indirizzi | I/O su unico bus multiplato per dati e indirizzi                                     |

Di solito si **preferiscono le NAND** per l'elevata densità e la velocità in scrittura.

### Operazioni su memorie Flash

Per **scrittura o programmazione** si intende portare un bit da 1 a 0.
Per **Cancellazione** invece si ripotano tutti i bit a 1.

La struttura di una NAND Flash è la seguente:

![[Pasted image 20230405120413.png]]

La minima unità di lettura o scrittura è la **pagina**, mentre il parallelismo in questo caso è di 8 bit.
Nella pagina alcuni byte (64 nell'esempio) sono riservati per codice di recupero errori. I dati effettivi stanno nel resto della pagina.
Le pagine sono raggruppate in **blocchi**, che rappresenta la **minima unità di cancellazione**.

Alcuni parametri sono il $t_{PROG}$, ovvero il tempo per scrivere una pagina.
$t_{R}$ è il tempo per fare la lettura di una pagina.
Il tempo di cancellazione $t_{BERS}$ è il tempo per cancellare un blocco intero.

### Segnali per una memoria FLASH

- RE# Read Enable
- WE# Write Enable
- CE# Chip enable, segnale di attivazione generale della scheda
- NON è presente un clock in quanto le flash sono tipicamente asincrone
- R/B# Ready, busy è un segnale che serve per avere informazioni sullo stato della memoria, ad esempio se è nello stato busy non è possibile eseguire determinate informazioni.

```ad-info
Il simbolo # significa che i segnale sono attivi bassi!
```


### Degradazione delle memorie FLASH

A seguito di numerose operazioni effettuate sulla memoria, è possibile che venga influenzato il corretto funzionamento della flash.

![[Pasted image 20230405122145.png]]

Nel grafico a destra si nota come a seguito di numero cicli di Programming/Erasing la tensione di soglia delle celle si alza fino a un punto in cui smette di funzionare.

Un altro parametro che si degrada è il tempo necessario per effettuare scrittura e cancellazione.

Nel caso si danneggiamento di una sottoparte, allora la logica interna è in grado di far ruotare le pagine riorganizzando gli indirizzi in modo da bypassare questi danneggiamenti.

### Memorie FLASH seriali

Sono speciali tipi di Flash a cui si accede in modo seriale.
Esse usano l'**interfaccia SPI**, per appunto accedere serialmente, quindi si legge o si trasmette un solo bit alla volta.

![[Pasted image 20230405122719.png]]

A differenza delle normali FLASH sono sincrone, quindi è presente un segnale di CLK.
I segnali importanti sono SCK, ovvero il clock, SI serial in, e SO serial out per trasmettere i bit.
Il principale scopo di queste memorie non è avere alte prestazioni quanto più avere un chip molto compatto.
