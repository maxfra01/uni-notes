# Comandi per la gestione dei processi
---
Vedi precedente [[Processi]].
Normalmente in una shell i comandi sono eseguiti in foreground e in maniera sequenziale.
Tramite la sintassi: `comando &` è possibile eseguire un processo un background ottenendo subito la console libera.

Tramite il comando

```bash
ps
ps -aux
```

è possibile visualizzare in modo più o meno compatto i processi attivi in quel momento, con i loro dettagli. Se specificato **defunct** significa che è un processo zombie.


Per terminare un processo si utilizza il comando `kill`, con sintassi:

```bash
kill [-sig] pid
```

Per l'elenco dei [[Segnali]] usare `kill -l`
I segnali possono essere specificati tramite nome oppure numero.
Il segnale di defult è `-SIGTERM` oppure `-TERM`.

Per uccidere tutti i processi di un dato nome, è possibile usare:

```bash
killall [-sig] name
```

Utile per uccidere tutti i processi generati da un programma fuori controllo.


# Aspetti teorici
---
Il sistema operativo tiene traccia per ogni processo di:
- lo stato del processo
- il suo relativo program counter
Queste informazioni sono memorizzate nel **Process Control Block  PCB**.

### Stati di un processo

| Stato      | Descrizione                                                        |
| ---------- | ------------------------------------------------------------------ |
| New        | Il processo è creato e sottomesso al SO                            |
| Running    | Il processo è in esecuzione                                        |
| Ready      | Logicamente pronto ad essere eseguito, in attesa della CPU         |
| Waiting    | In attesa di risorse da parte del sistema o in attesa di un evento |
| Terminated | Il processo finisce e rilascia le risorse del sistema                                                                   |

![[Pasted image 20221024103836.png]]

Quando un nuovo processo è creato, esso è ammesso allo stato ready, qui aspetta la CPU.
Quando la CPU lo eseguo allora va in stato di running.
Da qui è possibile tramite interrupt far tornare il processo allo stato ready, oppure mandare in waiting (aspetto I/O) o terminare il processo.

### Content switching
Ogni volta che la CPU è assegnata a un nuovo processo, il kernel deve salvare lo stato del processo running e caricare un nuovo processo ripristinandone lo stato precedentemente salvato.
Il tempo di questa operazione va limitato, perchè in quel tempo la cpu non esegue processi.

![[Pasted image 20221024104636.png]]

L'operazione di content switching è operato dall **Scheduler** che deve massimizzare l'utilizzo della CPU.


# Controllo avanzato
---
Se voglio che padre e figlio, dopo una fork, eseguano codici diversi (non sezioni di codice differenti), perciò si deve fare uso dei comandi `exec`.

A differenza della fork, l'exec **sostituisce il processo con un nuovo programma**.
Dato che la exec sostituisce di fatto il processo con uno nuovo, quando ne incontro una nel codice, il codice che segue quella exec non verrà mai eseguito :(
Il PID del processo è il medesimo.

![[Pasted image 20221024105125.png]]

### Prototipi della exec

```c
int execl (char *path, char *arg0, ..., (char *)0);
int execlp (char *name, char *arg0, ..., (char *)0);
int execle (char *path, char *arg0, ..., (char *)0, char *envp[]);

int execv (char *path, char *argv[]);
int execvp (char *name, char *argv[]);
int execve (char *path, char *argv[], char *envp[]);
```

Esistono 6 versioni differenti della exec, a seconda delle terminazioni del nome
- l $\implies$ riceve una lista di argomenti (Terminato da `(char *)0`  )
- p $\implies$ cerca nel $PATH di default
- v $\implies$ riceve un vettore di argomenti (terminato da NULL)
- e $\implies$ lista di environment che specificano le varibili di ambiente

Valori di ritorno della exec:
- Nessuno, in caso di successo
- $-1$ in caso di errore

Dato che la exec sostituisce di fatto il processo con uno nuovo, quando ne incontro una nel codice che chiama lo stesso programma, il codice che segue quella exec non verrà mai eseguito.

### Scheletro di una Shell UNIX

La shell unix rispetta la seguente struttura in c:
Esecuzione Foreground (il padre aspetta)

```c
while (TRUE) {
	write_prompt ();
	read_command (command, parameters);
	if (fork() == 0)
		/* Child: Execute Command */
		execve (command, parameters);
	else
		/* Father: Wait Child */
		wait (&status);
}
```

In Background il padre non attende il figlio:

```c
while (TRUE) {
write_prompt ();
read_command (command, parameters);
	if (fork() == 0)
		/* Child: Execute Command */
		execve (command, parameters);
	#if 0
	else
	/* Padre: NON Attende */
		wait (&status);
	#endif
}
```


### System call system

```c
#include "stdlib.h"

int system(const char *string);
```

Questa syscall passa il parametro `string` all'ambiente host affinchè esso venga eseguito.
Valori di ritorno:
- $-1$ se fallisce
- $127$ se fallisce la exec con cui è implementata
- il valore di terminazione della shell che esegue il comando

Ad esempio se nel codice c incontro `system("ls -la")`, il programma passa alla shell la stringa, e la shell la esegue.


