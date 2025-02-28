# Data Lakes

A valle dei [[Data Warehouse]] sono state introdotto delle nuove tecnologie per conservare i dati.
I **Data Lakes** sono depositi di dati in **formato non lavorato** (quindi originale), e di fatto non ci sono preparazioni di dati: possono essere in formato strutturato e non, file binari, mail o file di testo.
Le **query** per questo tipo di dati sono più simili a ricerche su google.

Perchè sono necessari i dati lakes?
Spesso **non conosciamo a priori le informazioni di rilievo** e quindi di fatto non si riesce a preconfigurare una forma di magazzino di dati ottimale.

### Caratteristiche dei Data Lakes

- I **Data lakes** memorizzano **tutti i dati che si raccolgono** (a differenze dei DW dove c'è una selezione dei dati di rilievo o no) e **di tutti i tipi** che potrebbero un giorno tornare utili.
- Sono **in grado di gestire tutti i tipi di dato**
- **Gestiscono facilmente i cambiamenti** 
- Forniscono un **rapido accesso ai dati**

Finora abbiamo visto che i dati nei DW provenivano da sistemi transazionali, DB operazionali e applicazioni.
I DW erano caratterizzati da **Schema-on-write**, alto costo di immagazzinamento, alta qualità dei dati, analisi avanzata, utenti non esperti di dati e query in formato standard.

Al contrario i dati **all'interno di un Data Lakes** sono di vario genere e possono provenire da numerosi le fonti, lo schema è definito nei momenti dell'analisi (**Schema-on-read**), immagazzinamento a basso costo, bassa affidabilità dei dati (possono o non possono essere corretti), gli utenti sono data scientists e le query sono più avanzate (usano ML, predictive analytics, profiling ecc...)

### Pro e contro dei Data Lakes

| Pro                                                                                 | Contro                                                                    |
| ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| Grande capacità di raccolta dati, da sorgenti eterogenee e in poco tempo            | I dati sono memorizzati senza contesto                                    |
| Le strutture rigorose sono definite solo in lettura                                 | La consistenza e qualità dei dati non è garantita                         |
| Self service analytics: grazie a figure qualificate che interagiscono nel data lake | Gli utenti business non hanno tempo di imparare ad analizzare dati grezzi |
| Possibilità di creare Sandbox per le analisi                                        | Alcune Query possono mandare in down i cluster                                                                          |

La domanda fondamentale è: vale la pena di collezionare tutti quei dati senza una strategia organizzativa dietro? C'è il rischio che si passi a un **Data swamp** con enormi quantità di dati completamente inaccessibili.