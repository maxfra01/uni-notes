# Data science

Introduzione su [[Big Data]]
**Machine Learning e Data Mining** rappresentano estrazione di informazioni utili dai dati in modo automatico, secondo precisi algoritmi.
Un primo esempio di utilizzo di queste tecniche è il **profiling**.
I dati sono generati ad esempio da social network o e-commerce online, e tramite queste informazioni si può implementare un **recommendation system** o eseguire **market basket analysis**.

### Knowledge Discovery from data

^fb090c

![[Pasted image 20231005102137.png]]

I dati grezzi non sono pronti per essere elaborati, vanno prima selezionati, pre processati, se necessario applicare trasformazioni per ottenere dei modelli **(pattern)** su cui applicare gli algoritmi d'interesse.

Durante la fase di **Pre Processing** distinguiamo due fasi: **data cleaning** (dove si riduce il rumore e gli outliers) e **data integration** (combinare i dati, integrare i metadati e risolvere conflitti e ridondanze).

### Association rules

Un primo tipo di obiettivo è quello di **estrarre frequenti correlazioni fra pattern**: ad esempio analizzando il carrello della spesa troviamo correlazioni fra due articoli che vengono acquistati spesso insieme.

![[Pasted image 20231005103431.png]]

### Classification

L'obiettivo della classification è **predire un'etichetta** da un insieme discreto di etichette.
Per costruire questo sistema si parte da un **training set** con il quale si allena il modello. Per testarlo realmente si usano **unclassified data**.

![[Pasted image 20231005103623.png]]

### Clustering

Lo scopo di questo algoritmo è quello di raggruppare dati simili, ed identificare outliers ed eccezioni.
I soli algoritmi di clustering non specifica il perchè si raggruppino certi dato: per ovviare a ciò si usano in combinazione con algoritmi di classificazione.

![[Pasted image 20231005104005.png]]

Altre tecniche sono: **sequence mining** (usato per identificare l'ordine sui dati analizzati, ad esempio l'ordine delle basi azotate del DNA), **Time series and geospatial data** (analizzano serie temporali di dati temporali e geografici), **Regressione** (ovvero la previsione di un valore continuo, come l'andamento di una valuta) e infine la **ricerca di outliers** (ad esempio per verificare traffico anomalo su una rete).

[[DBMS]]