# Memoria principale

Per eseguire un programma occorre che sia caricato dal disco in **memoria** **principale**. La memoria principale e i registri sono le uniche memorie a cui la CPU può accedere direttamente. L'accesso ai registri è fatto in colpo di clock (o meno).
La memoria **cache** sta in mezzo alla memoria principale e ai registri.

### Protezione

Per garantire la **protezione dei sistema operativo** si usa il concetto di **spazio** di **indirizzamento**: usiamo un registro **base** e **limit** per definire la grandezza dello spazio di indirizzamento.
E' bene che questi indirizzi si trovino nei registri della CPU, in modo da essere facilmente visibili e accessibili.

![[Pasted image 20240311121628.png]]

Supponiamo di avere due processi in esecuzione: entrambi vengono allocati all'indirizzo 0 di memoria ma questo non è fisicamente possibile. L'idea è quella di usare **indirizzi logici** rilocando gli indirizzi aggiungendo a quest'ultimi il **base register**.

### Address binding

Dovendo caricare in memoria un processo, non lo si può allocare all'indirizzo zero: quando scriviamo il sorgente vediamo un simbolo, il quale in fase di compilazione sarà **associato a indirizzi rilocabili**. Sarà compito del linker/loader trasformare questi indirizzi rilocabili in **indirizzi assoluti**.

L'associazione delle **istruzioni** a indirizzi di memoria può avvenire in tre momenti differenti:
- Nel caso di indirizzi conosciuti a priori, possiamo avere il binding già in fase di **compilazione**.
- Se gli indirizzi non sono noti in compilazione, saranno generati **indirizzi rilocabili in fasi di load**.
- Siccome il loader carica il programma a frammenti spesso non si hanno informazioni complete fin quando non si va in esecuzione. In quel caso il binding è fatto solo durante l'**esecuzione**.

Definiamo quindi due tipologie di indirizzi:
- **Indirizzo logico**: anche chiamato virtuale, è generato dalla CPU ed è quello che vediamo nel codice sorgente
- **Indirizzo fisico**: è l'indirizzo che vede l'unità di memoria
Entrambi vanno a definire rispettivamente lo **spazio di indirizzamento** fisico e logico: nel caso dello spazio logico è spesso un **intervallo**.

### Memory Management Unit (MMU)

Questo blocco hardware (facente parte della CPU stessa) si occupa della **traduzione degli indirizzi**: si base sul **relocation** register e un comparatore.
Il relocation register possiede l'informazione da sommare all'indirizzo logico, mentre il comparatore ha la sola funzione di **protezione** (non andando a sforare gli indirizzi possibili).

![[Pasted image 20240311123812.png]]

### Dynamic linking & loading

E' possibile che **un programma non sia caricato completamente in memoria**, nonostante sia in esecuzione: ad esempio possiamo caricare le funzioni solamente alla loro chiamata.
Questo porta a un miglior utilizzo della memoria, evitando (ad esempio) di caricare funzioni mai utilizzate.
Se adottiamo l'approccio del **dynamic loading**, tutte le routine sono mantenute su disco in un **formato rilocabile** pronto per il loading.

Per **dynamic linking** si intende il lasciare dei collegamenti irrisolti (ad esempio non si vanno a tenere in considerazione tutti i .h). Concettualmente sono concetti separati, ma spesso sono adottati insieme.

### Allocazione contigua

Una **partizionamento** **variabile** significa assegnare uno spazio di indirizzamento variabile a seconda delle esigenze del programma.
Andando a creare o eliminare i processi dalla memoria principale si vanno a creare dei **buchi** di dimensione variabile in posizioni casuali.
Se arriva una nuova richiesta di esecuzione di un processo come posso allocare la nuova memoria?
- First fit: allochiamo nel primo buco libero con spazio a sufficienza
- Best-fit: allochiamo nel buco più piccolo, ma abbastanza grande da soddisfare lo spazio
- Worst-fit: allochiamo nel buco più grande possibile

La **frammentazione** si verifica per la casualità dell'allocazione e deallocazione dei processi: otteniamo tanti piccoli buchi liberi dispersi per la memoria.
Definiamo la frammentazione **esterna** come le porzioni di memoria libere esterne ai processi.
Esiste la frammentazione **interna**: quando sovrallocachiamo della memoria rimane sempre una parte non utilizzata (che è appunto interna al processo).

Per ridurre la frammentazione esterna tramite la **compattazione**: questo è possibile solamente se la rilocazione della memoria è dinamica (cambiare il base register e muovere i dati in memoria). Durante le fase di I/O non possiamo compattare la memoria, perciò compatteremo i processi solamente in fase di ready (e non quelli in waiting).
Lo stesso problema è quello del **backing store** (vedi più avanti).
### Paging (allocazione non contigua)

Fare compattazione è un'operazione costosa, perciò si è deciso di passare dall'allocazione contigua a una non contigua: questo previene il problema della frammentazione esterna, e dell'allocazione di memoria variabile.
Il **Paging** fa uso di allocazione di memoria a dimensione fissa: 
- Dividiamo la memoria fisica in **frames**
- Dividiamo la memoria logica in **pagine**
Pagine e frames hanno la **stessa dimensione**

```ad-warning
title: Frame vs Page
Al di fuori di questo contesto i termini frame e page sono spesso confusi e usati in modo scorretto.
Inoltre il termine **blocco** verrà usato per indicare zone di memoria di massa.
```

I frames hanno una dimensione pari a potenze di 2, in modo che terminino con molti zeri (e quindi sia più semplice tradurre gli indirizzi). Si **tiene traccia** dei frames liberi in memoria, andando a costruire una **page table** che traduce gli indirizzi.
Ogni pagina logica va in un frame in modo non contiguo: l'unica cosa importante è non perdere la page table.
A differenza del base register, la page table non sta nella CPU

![[Pasted image 20240311135459.png]]

La MMU è più complessa: gli indirizzi generati dalla CPU si dividono in **page number (p)** e **page offset (d)**.
- Il page number viene usato come **indice della page table** per conoscere il base address di quella pagina.
- Il page offset verrà sommato al base address per generare l'indirizzo fisico in memoria a cui si deve accedere.

![[Pasted image 20240311140513.png]]

Se prendiamo indirizzi su $m=4$ bit e pagine grandi $2^2=4$ elementi ($n=2$): i primi due bit più significativi sono il page number, i due più bassi l'offset.

Notiamo che la page table usa come indice solamente i primi $m-n$ bit dell'indirizzo, in quanto l'offset pilota sia lo spazio logico che lo spazio fisico.

### Page table

Dal punto di vista della frammentazione interna, si preferisce avere pagine più piccole: ciò significa meno frammentazione, ma anche più pagine e quindi una page table di dimensioni maggiore.
La **page table è una struttura del kernel** tenuta in memoria principale. Dato che la tabella non è altro che un vettore, è ragionevole memorizzare altre informazioni (poche) oltre all'indirizzo fisico. La struttura di ogni entry è la seguente:

![[Pasted image 20240311141332.png]]

Per implementare la page table dobbiamo ricordarci che, essendo in memoria principale, ha bisogno di:
- Page-table base register (PTBR) che punta all'indirizzo della page table
- Page-table length register (PTLR) indica la dimensione della page table
Dato che per ottenere l'indirizzo di un frame serve fare un accesso alla page table, vado a fare di fatto 2 letture per ottenere un solo dato.
Dobbiamo quindi introdurre il TLB

### Translational lookaside buffer (TLP)

E' una specie di cache per la page table che implementa quella che si chiama **memoria associativa** (lookup in O(1)).
La probabilità di ottenere un hit è fortemente disuniforme.

![[Pasted image 20240315083803.png]]

Il contenuto della TLB ha una struttura simile alla seguente:

![[Pasted image 20240315084129.png]]

Contiene informazioni sulla validità nella pagina: il campo **valid** serve a distinguere fra righe usate e non usate, quindi se devo caricare una nuova pagina in TLB verifico se prima è valida. Nella TLB si preferisce ottenere un'alternanza di "pieno-vuoto" rispetto ad avere delle righe valid fino a un certo punto e poi vuoto. 

Abbiamo due strategie per gestire la TLB:
- Quando si cambia processo si preferisce svuotare completamente la TLB e dopo qualche miss la CPU la riempirà correttamente
- La TLB è abbastanza grande e contiene le entry di più processi: è possibile avere la stessa page number su due (o più) entries diverse, la differenza sta nel fatto che le due entry appartengono a processi diversi. Per fare questo nel contenuto del TLB si aggiunge il **Address-space identifier** (ASID) che di fatto corrisponde al PID di un processo.

Come si valutano le prestazioni di una TLB?
Definiamo lo **hit ratio** come la percentuale degli accessi in memoria che trovano il contenuto in TLB.
- Supponiamo un hit ratio di 0.8, probabilità di miss 0.2
- Supponiamo che l'accesso in memoria costi 10ns: se trovo il contenuto in TLB ci metto 10ns, se devo anche cercare in memoria ce ne metto 20.
Possiamo calcolare l'**effective access time** (EAT) come la media pesata del tempo di accesso:
$$
\text{EAT} = 0.80 \cdot 10 + 0.20 \cdot 20 = 12 \text{ns}
$$

### Memory protection

Viene implementato un meccanismo per distinguere le pagine **read-only o read-write.**
Posso anche distinguere fra execute-only o fetch-only e così via.
Nella page table il bit **valid/invalid** chiarifica se quella pagina fa parte dello spazio di indirizzamento.

### Shared pages

Supponiamo di avere tre processi: tutti stanno usando la stessa librerie (stesso IDE) e hanno 4 pagine da cercare in memoria. Ogni processo dovrebbe cercare in memoria 4 frame diversi: l'idea è quindi quella di modificare la page table per fare in modo che puntino agli stessi frame.

![[Pasted image 20240315090034.png]]

Dal punto di vista software dobbiamo fare in modo che le funzioni siano **rientranti**.
Il vantaggio delle pagine condivise è il **risparmio della memoria RAM**.

# Struttura della page table

Calcoliamo la dimensione di una page table.
- Supponiamo di avere un indirizzamento su 32 bit
- Supponiamo che la dimensione di una pagina sia di 4KB ($2^{12}$)
Date queste due informazioni sappiamo che il numero di entry (se tutti gli indirizzi sono validi) della page table è $2^{32} / 2^{12} = 2^{20}$ righe.
E' da notare che possiamo avere meno RAM di quella logicamente presente: gli indirizzi fisici non occuperanno così tanti bit come gli indirizzi logici.
In generale $p \neq f$ sia nella situazioni in cui ho più ram fisica o nei casi in cui ho meno ram fisica.
- Nel caso dell'esempio sopra, l'indirizzo p per determinare la pagina sarà di 20 bit
- Se ogni entry è grande 4 byte allora (2^20 righe) avrò **per ogni processo 4MB di page table**.

Lo spazio utilizzato per processo potrebbe risultare eccessivo, non si vogliono allocare quei MB di spazio contiguo: perciò esistono tre alternative.

### Hierarchical paging

Con questa strategia possiamo scorporare la page table in altre page table più piccole (poi si usano i bit più significativi per scendere nella gerarchia):

![[Pasted image 20240315091621.png]]

Nel caso di una sotto tabella con tutti invalid posso evitare di allocare quel pezzo e mettere invalid alla entry nella tabella padre. Lo svantaggio lo si ha in termini di tempo per l'accesso in memoria.
Nel caso di un **two-level paging** otteniamo un mapping simile a questo:

![[Pasted image 20240315092018.png]]

### Hashed page table

Consiste nell'usare una funzione di hash per determinare l'indirizzo della pagina da cercare.
Si usa una hash table con chaining: posso avere più dati in una riga della tabella di hash, ciò comporta una lettura extra (si spera sempre di compensare con la TLB):

![[Pasted image 20240315092555.png]]

### Inverted page table

Finora le soluzioni viste sopra hanno una tabella per processo: nel caso della **inverted page table** usiamo una sola tabella per tutti i processi.
L'unico problema è sempre la ricerca lineare: per risolvere si mette a monte una hast table.
Cosa cambia allora dalla hash page table? 
Semplicemente la inverted ha una unica hash table per tutti i processi, mentre la hash page table è unica per processo.

### Swapping

E' possibile che la memoria principale si saturi completamente: se vogliamo lanciare un nuovo processo dobbiamo usare il concetto di **swapping**.
Un processo può essere spostato dalla memoria al disco in zone dette **backing store** per fare spazio a nuovi processi. E' ovvio che la scelta del processo da spostare dev'essere fatto seguendo una certa politica.
Quando riporto il processo swappato in memoria devo rimetterlo nello stesso posto? 
Dipende da come si gestisce la page table.

![[Pasted image 20240315095019.png]]

Se il prossimo processo da eseguire non è in memoria (e la memoria è piena) allora devo fare un **context switch** (operazione che richiede parecchio tempo).

Esiste anche lo **swapping con paginazione**: non "swappiamo" interi processi ma solamente un sottoinsieme delle pagine.

