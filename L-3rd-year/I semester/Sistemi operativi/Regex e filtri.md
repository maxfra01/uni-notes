# Espressioni regolari
---
Formalismo per ricercare un insieme di parole stando a determinate regole (una certa lettera iniziale, numero ecc...)
Sono state **standardizzate da POSIX** e ne esistono di diversi tipi: BRE, ERE, PCRE.
Di fatto è composta da **operatori compatti**.
Vengono utilizzate per i comandi grep, awk ecc... per esplorare il [[File System]] dei sistemi GNU-Linux.

Per **letterale** intendiamo una qualsiasi lettera o sequenza di lettere che andiamo a utilizzare per il match.
Per **Metacarattere** intendiamo un qualsiasi carattere che abbia un particolare significato.
Per **Sequenza di escape** intendiamo il carattere `\` che serve per utilizzare un metacarattere come letterale.

| Metacarattere    | Significato                                                        |
| ---------------- | ------------------------------------------------------------------ |
| \[...\]          | Specifico un elenco di carattere, e dovrà esserce uno dei compreso |
| (...)            | Gestisce la precedenza dei caratteri                               |
| sbarra verticale | Permette di effetuare l'OR logico fra regexp                       |
| \\<              | Inizio della parola                                                |
| \\>              | Fine della parola                                                  |
| ^                | Inizio riga                                                        |
| $                | Fine riga                                                          |
| *                | Elemento presente preso da 0 a $\infty$ volte                      |
| +                | Elemento presente preso da 1 a $\infty$ volte                      |
| ?                | Elemento presente da 0 a 1 volte                                   |
| \[c1c2c3\]       | Un possibile carattere fra c1 c2 c3                                |
| \[c1-c2\]        | Un possibile carattere nel range specificato                       |
| \[^c1-c2\]       | Tutti i caratteri non nel range specificato                        |
| {n}              | Elemento presente esattamente $n$ volte                            |
| {n1,n2}          | Elemento presente da $n_{1}$ a $n_{2}$ volte                       |
| c                | Un carattere qualsiasi                                             |
| .                | Qualsiasi carattere (non \\n)                                      |
| \\s              | Uno spazio o tabulazione                                           |
| \\d              | Una cifra fra 0 e 9                                                |
| \\D              | Non una cifra                                                      |
| \\w              | Qualsiasi carattere 0-9, a-z, A-Z                                  |
| \\W              | Qualsiasi carattere non 0-9, a-z, A-Z                                                                   |



# Comando Find
---
Comando di shell che permette di ricercare file, link e direttori che soddisfino certi criteri e eventualmente eseguire azioni su di essi.

```bash
find <direttorio> [-opzioni] [-azioni]
```

In sostanza la find visita tutto l'albero del file system a partire dal `<direttorio>`, crea l'elenco che soddisfa le opzioni e eventualmente specifica per ogni file le azioni.

## Opzioni

| Opzione                | Significato                                                                                     |
| ---------------------- | ----------------------------------------------------------------------------------------------- |
| -name pattern          | In pattern opportunamente inseriamo un nome con cui fare il match                               |
| -regex expr            | E' possibile inserire una regex per il match del **path completo**                              |
| -regextype type        | Occorre specificare che classe di regex uso, tipo `posix-basic` , e va inserito prima di -regex |
| -atime\[+-\]n          | Filtra l'ultimo Access al file                                                                  |
| -ctime\[+-\]n          | Filtra                                                                                          |
| -mtime\[+-\]n          | Filtra la modifica del file                                                                     |
| -size\[+-\]n\[bckwMG\] | Filtra in base alla dimensione del file (lettere finali per l'unità di misura)                  |
| -type t                | Filtra in base al tipo fILE, sOCKET, lINK, dIRECTORY, pIPE                                      |
| -user u                |  Dev'essere il proprietario del file                                                                                               |
| -group g               |  Gruppo di appartenenza del proprietario                                                            |
| -readable              | Indica solo i file di cui si dispone di diritto di lettura                                      |
| -writable              | Come sopra ma per scrittura                                                                     |
| -executable            | Come sopra ma per eseguire                                                                                                |
| -quit                  | Dopo il primo match stampo e interrompo la ricerca                                              |
| -mindepth a            | Indica la minima profondità in cui effettuare la ricerca nell'albero dei direttori              |
| -maxdepth a            | Indica la massima profondità a cui effettuare la ricerca                                        |


## Azioni

| Azione   | Significato                                                       |
| -------- | ----------------------------------------------------------------- |
| -print   | Stampa tutti i match che trova dalla find, è l'opzione di default |
| -fprint  | Effettua l'output su file                                         |
| -print0  | Stampa ma non va a capo                                           |
| -execdir | Esegue il comando specificato                                     |
| -exec    | Versione POSIX sicura dell'azione precedente                      |
| -delete  | Elimina l'oggetto rintracciato                                                                  |


# Filtri
---
In UNIX un filtro è un comando che riceve tramite stdin un input, filtra secondo determinate opzioni e produce output su stdout.

## Cut

Un primo filtro è cut, che è in grado di rimuovere sezioni specifiche di testo in un file di testo

```bash
cut [opzioni] file
```

Usando ad esempio il comando `cut -f 1-3 file` rimuove dal file le colonne (fields) da 1 a 3.

| Opzioni | Significato                                                |
| ------- | ---------------------------------------------------------- |
| -f      | Considera i Fields e posso specificare i valori o un range |
| -d      | Indica il delimitatore che usiamo per separare le colonne  |
| -c      | Seleziona i caratteri di posizione indicata                                                           |


## Tr

Filtro utile per modificare le occorrenze di alcuni caratteri con altri che scegliamo.

```bash
tr [opzioni] set1 [set2]
```

| Opzioni | Significato                                                         |
| ------- | ------------------------------------------------------------------- |
| -s      | Squeeze, compatta i caratteri specificati uguali di fila in una sola occorrenza |
| -d      | Cancella le occorrenze del carattere specificato                                                                    |

## Uniq

Filtro in grado di riportare o eliminare le righe ripetute in un file

```bash
uniq [options] [inFile] [outFile]
```

| Opzioni | Significato                                              |
| ------- | -------------------------------------------------------- |
| -c      | Stampa per ogni riga il numero delle occorrenze nel file |
| -d      | Visualizza solo le righe ripetute                        |
| -f N    | Ignora i primi N campi e poi fa normale                  |
| -I      | Case insensitive                                                         |

In assenza di opzioni il filtro uniq elimini i ripetuti.

## Basename

Filtro in grado di rimuovere path e estensione dal nome di un file

```bash
basename nome [estensione]
```

Il comando 'duale' per ottenere la directory di un file uso `dirname`.

## Sort

Ordina i file di input in ordine alfabetico

```bash
sort [opzioni] [file]
```

| Opzioni | Significato                                  |
| ------- | -------------------------------------------- |
| -b      | Ignora gli spazi iniziali                    |
| -d      | Considera solo spazi e caratteri alfabetici  |
| -f      | Case insensitive                             |
| -I      | Case insensitive                             |
| -n      | Confronta in base all'ordine numerico        |
| -r      | Ordine inverso                               |
| -k c1   | Specifico il campo secondo il quale ordinare |
| -m      | Merge di file già ordinati senza riordinare  |
| -o=f    | Output su file                                             |


## Grep

Cerca nel contenuto di un file le **righe che hanno un match con il pattern indicato** e le stampano su stdout.
Grep sta per **Global Regular Expression Print**

```bash
grep [opzioni] pattern [file]
```

| Opzioni    | Significato                                         |
| ---------- | --------------------------------------------------- |
| -e Pattern | Permette di specificare un pattern aggiuntivo       |
| -B N       | Stampa N righe prima di una riga del match          |
| -A N       | Stampa N righe dopo una riga di un match            |
| -H         | Stampa anche il nome del file in cui trova il match |
| -I         | Case insensitive                                    |
| -n         | Stampa il numero di riga del match                  |
| -r, -R     | Procede in maniera ricorsiva sul sottoalbero        |
| -v         | Inverse, stampa solo le righe che non matchano                                                    |

