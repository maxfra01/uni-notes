# Deadlock
---
Una **condizione di stallo** è la situazione in cui un task chiede una risorse non disponibile, entra in attesa e l'attesa non termina più.

**Deadlock** implica starvation e non il contrario!
Nello starvation gli altri task vanno avanti, in deadlock sono tutti bloccati.

Per il verificarsi del deadlock devono essere rispettate tutte le seguenti condizioni:

| Condizione                  | Descrizione |
| --------------------------- | ----------- |
| Mutua esclusione            |       intuitive      |
| Possesso e attesa           |             |
| Impossibilità di prelazione |             |
| Attesa circolare                            |             |

Occorre modellizzare il deadlock per poterlo gestire e dopodichè trattarlo:
- tecnica struzzo, ovvero supporre probabilità di deadlock bassa (usata in Windows e Unix)
- rilevare e ripristinare
- prevenire ed evitare

# Modellizzazione
---
Grafo di allocazione(assegnazione delle risorse).

I vertici sono divisi in due insieme:
- I processi in gioco, indistinguibili e in numero definito, ognuno dei quali accederà a risorse tramite accesso, utilizzo e rilascio
- Risorse del sistema, suddivise in classi con un certo numero di istanze (7 stampanti a colori, ecc...)

Gli archi si dividono in:
- Archi di richiesta, se un processo chide una risorsa
- Archi di assegnazione, se viene assegnata

![[Pasted image 20230109104016.png]]

A volte conviene semplificare il grafo in un grafo di **attesa**. Si eliminano i vertici risorsa e si compongono gli archi tra vertici rimanenti.

In modo analogo si genera il grafo di **rivendicazione** dove ispezioniamo gli archi di richiesta futura (tratteggiati) che potranno essere istanziati.

# Gestione a posteriori
---
Si permette al deadlock di verificarsi, dopodiche si interviene e si ripristina il sistema in uno stato senza deadlock.

**Rilevazione**: è possibile verificare la presenza di cicli
Se non ci sono cicli allora non c'è deadlock.
Se ci sono cicli e le risorse sono unitarie allora c'è deadlock.
Se ci sono cicli e istanze multiple non possiamo dire nulla (vedi Algoritmo del banchiere).

Ogni fase di rilevazione ha un costo, pari a quello di una DFS, che serve per identificare i cicli tramite archi backward.

La periodicità con cui effettuare una rilevazione:
- ogni volta che un processo fa una richiesta e questa non viene risolta subito.
- A intervalli di tempo fissi
- Quando l'attività della CPU scende sotto una certa percentuale.

Per **Ripristinare** possiamo:
- terminare tutti i processi in stallo
- terminare un processo alla volta tra quelli in stallo (costoso, per rollback e modifiche su file)
- Rimuovere le richieste verso risorse non concesse
- Prelazioniamo le risorse a un processo alla volta

In sintesi sono costose e complesse, quindi preferiamo prevenire.

# Prevenzione 
---
Si cerca di prevenire una di quelle 4 condizioni di cui si è parlato poco fa.
- Per prevenire la mutua esclusione possiamo vietare le risorse non condivisibili, oppure proibire l'attesa su risorse non condivisibili
- Per evitare possesso e attesa potremmo richiedere tutto all'inizio (**Request All First RAF**) ma non sempre possibile, oppure (**Release before Request RBR**) dove prima di chiedere una nuova risorsa si rilasciano tutte quelle di cui si è in possesso.
- Prelazione: permettere la prelazione (costoso)
- Attesa circolare: Uso di **Hierarchical Resource Usage HRU**, ovvero far in modo che tutti rispettino questa gerarchia 

# Evitare
---
Si forzano i processi a fornire informazioni aggiuntive riguardo le richieste che vorranno fare in futuro.
Gli algoritmi usati si basano sul concetto di stato sicuro e sequenza sicura:
- **Stato sicuro**: stato per cui il sistema è in grado di allocare le risorse a tutti i processi, senza stalli e trovando una sequenza sicura
- **Sequenza sicura**: E' una sequenza di schedulazione di processi che garantisce la terminazione di ogni processo grazie alle risorse che ha in possesso e di quelle rilasciate dai processi che terminano prima di lui.

![[Pasted image 20230109111009.png]]

Nel grafico abbiamo l'andamento di due processi, mentre le zone colorate sono quelle in cui i processi vogliono la disponibilià di quelle risorse.
Occorre studiare la traiettoria delle risorse, in particolare il rettangolo blu può solo andare in alto o a destra, dunque si verificherà deadlock, quindi quello blu è uno stato non sicuro!
