# Definizioni introduttive

Possiamo definire l'ingegneria del software con la  seguente affermazione:
- "**Multi person** construction of **multi version** software"
La presenza di più persone porta a problematiche di comunicazione e coordinamento, mentre la presenza di più versione porta problemi di mantenimento del software.

Un **software** è definito come un insieme di elementi tra i quali programmi, procedure, regole, e dati, come ad esempio documenti dei requisiti, project plan, test cases, documentazioni, manuali.
Esistono tipi di software diversi come embedded o stand-alone.

**Problematiche** legate al software:
- **Software safety**: riguarda la sicurezza fisica di persone, come ad esempio un software legato alla guida autonoma di un automobile
- **Mission critical**: errori che possono danneggiare il business delle aziende

**Proprietà del software**:
- Proprietà strettamente funzionali: cosa fa quel programma
- Proprietà non funzionali, come l'**usabilità**, l'**efficienza**, **reliability**, **Mantenibilità**, **sicurezza** e **dependability**.

# Processo software

Per **Processo software** si intende la combinazione di tre elementi: 
1. **Development**
2. **Operate**
3. **Maintenance**

### Development

Nella fase di sviluppo devo tenere conto di requisiti, design, coding e testing in questo ordine.

![[Pasted image 20240306084305.png]]

Nella fase di testing si procede con i **test di unità** dove si provano tutte le varie componenti minori separate, poi nella fase di **integration** le componenti separate iniziano a essere unite e testate. Infine esiste il **testing end-to-end** che rappresenta il test più completo che si può fare sul programma.

L'obiettivo del processo software è quello si sviluppare un software (codice, dati, documentazioni) con delle proprietà di processo (costi, tempo) e delle proprietà funzionali (cosa fa il software).
Possiamo applicare una metodologia **bottom up**: 
1. Partendo dal codice dobbiamo organizzare il codice in maniera strutturata
2. Per creare una struttura adeguata dobbiamo precedentemente fare la fase di design
3. Per ottenere il design occorre una analisi approfondita dei requisiti

![[Pasted image 20240306102236.png]]

Il modello descritto scopra è detto **waterfall**, dove ogni fase non può avviarsi se non si completa la precedente.
Alla fine di ogni fase vengono avviate le **attività di verification e validation (V&V)** e viene prodotto un documento con le informazioni della fase corrispondente.

Le figure presenti nel processo software sono:
- **Project manager**: colui che assegna il lavoro e monitora il progresso del progetto, inoltre stima e monitora il budget.
- **Configuration manager**: identifica e tiene traccia dei documenti e delle unità, inoltre tiene traccia della storia del progetto
- **Quality assurance**: definisce gli obiettivi qualitativi, come viene svolto il lavoro e controlla il risultato

![[Pasted image 20240306103135.png]]

### Operate e Maintenance

![[Pasted image 20240306104928.png]]

La manutenzione di un software prevede le stesse fasi dello sviluppo, ma sono presenti dei **vincoli** dettati dagli sviluppatori precedenti (ad esempio il tipo di linguaggio usato)
Seguono negli anni diverse **release** del software.

```ad-note
title: Software vs System process
A differenza di un software stand-alone, è possibile considerare lo sviluppo di un vero "sistema", ad esempio nel contesto embedded.
Il processo di sistemi si basa sulle medesime fasi del processo software, con l'unica differenza nella fase di coding, dove bisogna distinguere fra sviluppo hardware e sviluppo software.
```

### Approcci al SE

1. **Document based, Semiformal, UML** (trattato in questo corso), largamente usato nelle aziende 
2. **Formal/based model**, non scalabile, usabili in contesti limitati
3. **Agile**, pochi documenti ed enfasi su codice e testing

### Business model

1. Pay per use
2. Freeware and pro version
3. Shareware, il software è rilasciato gratis per incentivare il trial, poi chi decide di tenerlo lo paga (WinRAR)
4. Adware, gratis ma ci sono banner pubblicitari

Next: [[Requisiti]]