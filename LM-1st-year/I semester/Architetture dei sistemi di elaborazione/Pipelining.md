# Introduzione pipeline

La **Pipeline** è una tecnica che permette di sovrapporre l'esecuzione di più istruzioni in contemporanea.
Diverse unità (**pipe stages**) completano segmenti di diverse istruzioni in parallelo.
Questo naturalmente incrementa le prestazioni di un programma.

![[Pasted image 20231016101504.png]]

Il **throughput** è il numero di istruzioni che escono dalla pipeline per unità di tempo.
Tutte gli stadi della pipeline sono sincronizzati e la durati di quest'ultimi è determinato dallo **stadio più lento**: questa durata è detta **ciclo macchina**, che normalmente corrisponde alla durata di un ciclo di clock.

In una pipeline ideale dove ogni stadio ha la stessa durata abbiamo un incremento delle performance pari a:
$$
\text{throughput}_{\text{pipelined}} = \text{throughput}_{unpipelined} \cdot  n
$$
### Stadi della pipeline

- Instruction **fetch** cycle (IF)
- Instruction **decode** cycle (ID)
- **Execution** address cycle (EX)
- Memory **access**/ branch completion cycle (MEM)
- **Write-back** cycle (WB)

# Data path, senza pipeline

![[Pasted image 20231016102522.png]]

Il primo stadio di **Fetch** fa le solite due cose: caricare nell'IR l'istruzione contenuta all'indirizzo del PC, infine incrementa il PC:
$$
IR \leftarrow Mem[PC] \quad \quad NPC \leftarrow PC+4
$$
Nello stadio di **Decode** si procede con la decodifica dell'istruzione su 32 bit, spacchettando i vari operandi e immediati. L'opcode mandato alla ALU determina l'istruzione da eseguire:
$$
\begin{cases}
A \leftarrow Regs[IR_{6\dots 10}] \\
B \leftarrow Regs[IR_{11\dots 16}] \\
Imm \leftarrow Regs[IR_{16\dots 31}]
\end{cases}
$$

Lo stadio di **esecuzione** opera in maniera diversa a seconda del tipo di istruzione da eseguire:
- Register-register ALU instruction, ci sarà l'operazione fra registri A e B e il risultato nel registro ALU-Output.
- Register-immediate, stessa cosa ma con un immediato al posto di B
- Branch, in questo caso nell'ALU-Output andrò a mettere la somma del PC più l'immediato
- Memory reference, si eseguono calcoli per l'indirizzamento, quindi sommerò all'indirizzo di un registro un immediato (ad esempio `lw r1, v2(44)`)

Lo stadio di **Memory address/branch completion** si vanno ad eseguire le operazioni per completare queste operazioni:
- nel caso di memory reference vado a prelevare in memoria all'indirizzo che ho calcolato
- per il branch vedo se il flag di branch è attivo e successivamente andrò a sovrascrivere il PC

Nella fase di **writeback** si va semplicemente a scrivere il risultato dell'istruzione in un registro.

# Data path con pipeline

![[Pasted image 20231016115613.png]]

Istruzioni multiple sono velocizzate, mentre istruzioni singole sono rallentate a causa del controllo sulla pipeline stessa.

### Hazards

Sono situazioni che impediscono a un'istruzione di essere eseguita durante il ciclo macchina designato. Possono essere di 3 tipi:
- **strutturali**: quando due risorse sono richieste contemporaneamente dalla pipeline
- **data**: in questo caso il risultato di un'istruzione dipende da un'altra
- **controllo**: dipendono da branch e modifiche del P

### Stalli

Per non incorrere in **hazards** si introducono gli **stalli della pipeline**: si forza il processore a non fare nulla per un certo periodo di clock.
Le istruzioni prima dello stallo continuano nella pipeline, mentre quelle successive sono in stallo: si va a introdurre una **bolla** nella pipeline.

### Forwarding

Procedura per la quale si rileva se l'output di una operazione ALU deve essere usato come input di una seguente operazione ALU. L'hardware deve essere in grado di garantire un percorso per 'forwardare' il dato o in caso di interrupt/stallo di non fare nulla.

![[Pasted image 20231017084941.png]]

Talvolta nella pipeline dei percorsi di forward vanno indietro nel tempo, quindi non essendo implementabili si introduce uno stallo nella pipeline:
- E' possibile generare uno stallo inserendo una `nop`
- E' possibile generare uno stallo congelando il Program Counter
- E' possibile generare uno stallo forzando il Fetch/Decode della pipeline allo stesso valore

Il **Forwarding** è implementato dall'output della ALU/output data memory **a ALU input, data memory inputs, o zero detection unit**.

Si va a confrontare il registro destinazione dell'istruzione in fase di EX/MEM con i registri sorgente dell'istruzione successiva già in fase di FETCH/DECODE.

### Hazard di controllo

Sono dovuto a salti o istruzioni che modificano il PC dopo che l'istruzione seguente è entrata in fase di FETCH.
Distinguiamo due tipi di salti:
- taken
- untaken

Una possibile soluzione è quella di decidere in anticipo se il salto va preso oppure no, oppure aggiornare prima il Program Counter.

Nell'immagine sotto, in caso di **taken jump** l'istruzione di fetch dell'istruzione seguente al salto sarà inutile, in quanto si salterà altrove nel programma.

![[Pasted image 20231017090254.png]]

Se aggiorniamo subito il nuovo program counter allora eviteremo di caricare nel registro della pipeline di fetch un'istruzione inutile.

Esistono anche altre tecniche per evitare che la pipeline diminuisca le performance:
- **Congelare la pipeline**, finchè non si decide se prendere o meno il salto
- **Predict untaken**: di base si assume che il salto non sia preso, si caricano le istruzioni successive come se nulla fosse ed eventualmente si annullano nel caso di salto taken.
- **Predict taken**: si assume che il salto sia sempre preso, si andrà perciò a prendere l'indirizzo a cui devo saltare (trattato più avanti nel corso)

```ad-note
title: Ruolo del compilatore
Se un determinato hardware supporta il predict taken o untaked allora il compilatore può generare codice efficiente per massimizzare le possibilità di fare le predizioni corrette.

```

- **Delayed branch**:Questa tecnica si base sul prendere l'istruzione precedente al salto, e ne si sposta l'esecuzione dopo l'istruzione di salto (in ogni caso va sempre eseguita). Ovviamente il compilatore cerca di trovare un'istruzione che non ha dipendenze con il salto. Usando questa tecnica solamente il 30% dei salti provoca rallentamenti.

# Operazioni Multi-cycle

Finora abbiamo assunto che la fase di esecuzione di tutte le istruzioni duri un colpo di clock: nella realtà non è così, ad esempio per operazioni floating point si usano più colpi di clock (altrimenti dovremmo avere o un clock lento, o unità molto complesse).
Per ovviare a ciò si introducono **diverse unità funzionali che vanno a comporre lo stadio di EX**.

![[Pasted image 20231017093313.png]]

Definiamo la **latenza** come il numero di cicli di clock che dovrebbero essere tra un'istruzione che procede un risultato e un'istruzione che usa lo stesso risultato.

L'**initation interval** indica il numero di colpi di clock che devo attendere nel mezzo di due operazioni di uno stesso tipo.

### Hazard strutturali e di dato

A causa degli stadi delle operazioni floating point è possibile che istruzioni successive nel codice vengano completate prima delle precedenti: questo porta a problemi di **accesso concorrente e problemi di dipendenza (data hazard)**.
Le soluzioni a questo problema possono essere:
- aggiungere porte di scrittura ma è troppo costoso
- Inserire stalli dopo la fase di decode oppure prima della fase MEM/WB: questo approccio può andare a stallare la pipeline per molto tempo, a causa degli stadi multipli per operazioni floating point 

![[Pasted image 20231017095218.png]]

Il Data hazard visto sopra è detto **RAW, Read After Write**.
Dovrò quindi aspettare che una scrittura di registri sia terminata per eseguire una nuova lettura.

Un nuovo tipo di hazard consiste nella scrittura contemporanea di un registro, quindi le istruzioni in mezzo verranno corrotte: nel caso sotto il registro F2 viene scritto può essere scritto prima dalla load e poi dalla add.
Questo problema è noto come **WAW, Write After Write**, e per risolverla devo stallare un ciclo di clock prima della scrittura in memoria.

![[Pasted image 20231017100156.png]]

# MIPS R4000 Pipeline

![[Pasted image 20231017102736.png]]

[[MIPS]]
La fase di fetch è divisa in due parti. Anche la fase MEM è divisa in 3 stadi, in quanto l'accesso alla memoria spesso non è veloce quanto il processore perciò serve garantire più stadi.

# Eccezioni ed interrupt in pipeline

Le **Eccezioni** sono reazioni ad un evento che modificano il normale ordine di esecuzione del programma.
In un processore con pipeline, gestire le eccezioni è più complesso a causa dell'elevato numero di istruzioni in esecuzione.

Considerando ad esempio il seguente programma devo poter saltare da una parte all'altra del codice senza problemi:

```MIPS
ld F1,1
ld F2,0
div F3, F1, F2 ; qui si genera un'eccezione
Add F4, F3, F1
```

Spesso il termine *exception* e *interrupt* sono confusi:
- Una eccezione indica un evento interno alla CPU come un overflow FP, MMU fault, un trap (interrupt sincrone), non aligned memory access
- L'Interrupt invece è scatenato da eventi esterni di I/O come una device request o un reset

Le eccezioni sono classificabili in:
- **Sincrone o asincrone**: nel primo caso sono scatenate in posizione precise del codice, mentre il secondo tipo è spesso generato da dispositivi esterni.
- **User requested o coerced**: a seconda che siano o meno richieste dall'utente stesso
- **User maskable o non-maskable**: Normalmente l'utente può forzare o meno l'hardware a reagire a un'eccezione.
- **Within o between instructions**
- **Resume o terminate**: a seconda se, dopo la gestione dell'eccezione, il programma riparte o viene terminato

Normalmente al giorno d'oggi tutti i processori sono **restartable**: possono gestire un eccezione, salvare lo stato, e ripristinarlo senza compromettere l'esecuzione del programma.

### Gestione delle eccezioni in pipeline

Appena l'eccezione è rilevata la pipeline deve eseguire le seguenti operazioni:
- Si **forza un TRAP** sulla prossima operazione di Instruction FETCH.
- Finchè il TRAP è attivo, **tutte le istruzioni di scrittura dell'istruzione difettosa e quelle successive sono stoppate.**
- Quando l'unità che gestisce l'eccezione riceve il controllo **si salva immediatamente il Program counter**.
- Infine delle istruzioni speciali **ripristinano il PC e il flusso di istruzioni riprende**

### Gestione di interrupt in 80x86 e ARM

L'**Architettura 80x86** gestisce gli interrupt nel seguente modo:

![[Pasted image 20231023124900.png]]

Le **Architetture basate su ARM** invece le gestiscono nel seguente modo:

![[Pasted image 20231023125017.png]]

### Precise exceptions

Un Processore ha delle **precise exceptions** se la pipeline può essere interrotta in modo che:
- L'istruzione **precedente** a quella che ha scatenato l'eccezione **è completata**
- L'istruzione **successiva** a quella che ha scatenato l'eccezione **può essere ripresa da zero**
E' una feature abbastanza costosa, ma necessaria almeno per le operazioni intere: alcune architetture (MIPS R800) le implementane solo in debug mode, in quanto causa rallentamenti x10.

Con le operazioni multi-stadio (FP) gestire le precise exceptions è più complesso.
Come soluzioni possiamo:
- Accettare le **imprecise exceptions**
- Fornire una modalità operativa veloce ma *imprecise* e un'altra lenta ma *precise*
- Forzare le unità FP a **prevedere** se si genererà un'eccezione
- Tenere un buffer dei risultati di tutte le istruzioni già completate.

### Eccezioni in MIPS

A seconda degli stadi si possono generare diverse eccezioni:

| Stage | Eccezione                       |
| ----- | ------------------------------- |
| IF    | Page Fault on instruction fetch |
|       | Misaligned memory access        |
|       | Memory protection violation     |
| ID    | Undefined or illegal OP-code    |
| EX    | Arithmetic exception            |
| MEM   | Page fault on data fetch        |
|       | Misaligned memory access        |
|       | Memory protection violation     |
| WB    | --                                |

```ad-note
title: Eccezioni contemporanee
Caso rara, ma capita che due eccezioni vengano scatenate nello stesso momento per due istruzioni diverse: in questo caso si procede gestendo le eccezioni nell'ordine delle istruzioni.

```

