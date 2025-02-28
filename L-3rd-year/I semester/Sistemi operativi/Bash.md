# Introduzione alle Shell
---
E' la **parte più esterna** del sistema operativo, che in UNIX non rientra nel kernel ma bensì nello spazio utente. [[Introduzione ai SO]]
All'interno di `/bin/sh` c'è il link per la shell in uso.

Per **terminare una Shell** occorre digitare `exit` o il carattere EOF (Ctrl+D).

All'avvio di una Shell si attivano file di Login e file non di login tra cui ad esempio `.bashrc` mentre al logout si attiva sempre il file `.bash_logout`

### Quoting

Le virgolette a seconda che singole o doppie assumono diversi significati:
- Apici singoli, la stringa interna **NON può essere espansa** dunque anche se metto all'interno una variabile non la printa
- Apici doppi allora la stringa **può essere espansa**.

### Cattura dello stdout

Per catturare lo stdout abbiamo due metodi:
- Usare il carattere `$`
- Usare le backtilde \`\`

Esempio di seguito
```bash
myvar= $(ls)
echo $myvar

myvar=`ls`
```

E' possibile vedere la cronologia dei comandi tramite il file `.bash_history`


# Scripting
---
I linguaggi di script sono interpretati e non compilati, questo si traduce in un tempo di start molto breve ma nel lungo termine un calo delle prestazioni.

Ogni file inizia con la shell da utilizzare per interpretare il comando.

```bash
#!bin/bash
```

Gli script possono essere **eseguiti in maniera** diretta trattandoli come eseguibili, ma va ricordato che occorre aggiungere i privilegi di esecuzione.
Di fatto l'esecuzione diretta è fatta da un'altra shell in background (di fatto crea un nuovo processo), perciò è buona norma terminare lo script con `exit 0`.

Per **l'esecuzione indiretta** si intende eseguire lo script riga per riga sulla shell attuale, in particolare tramite il comando:

```bash
source ./scriptname args
```

Inoltre tramite questo metodo posso eseguire script anche se non dispongo dei permessi i esecuzione!

Il debug di script avviene in modo totale o parziale, per la totale basta indicare un'opzione a livello dell'interprete, mentre per uno parziale occorre usare il comadno `set`.

![[Pasted image 20221121110122.png]]


### Variabili predefinite

![[Pasted image 20221121110542.png]]

### Condizioni e test

![[Pasted image 20221202213047.png]]
