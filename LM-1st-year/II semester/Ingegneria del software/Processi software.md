# Modelli di Processo Software

## Waterfall

Il modello Waterfall (a cascata) è un approccio sequenziale per lo sviluppo del software. Le fasi del progetto (come requisiti, design, implementazione, test, integrazione e manutenzione) vengono completate una dopo l'altra. Questo modello è document-driven, il che significa che ogni fase produce un documento o un deliverable che "congela" il progresso prima che la fase successiva possa iniziare.

- Chiarezza e struttura ben definite.
- Facilita la gestione dei grandi progetti, con team distribuiti e contrattisti esterni.

- Mancanza di flessibilità: i cambiamenti ai documenti possono essere costosi e difficili da implementare.
- Burocrazia pesante e rigidità nelle modifiche ai requisiti e al design, che spesso sono le parti più difettose e necessitano di cambiamenti per adeguarsi ai bisogni dei clienti.

## Varianti del Waterfall

#### V-Model

Il modello V enfatizza le attività di verifica e validazione (VV). Ad esempio, i test di accettazione vengono scritti dopo o insieme ai requisiti, e i test di unità e di integrazione vengono scritti dopo o durante il design.

#### Prototyping + Waterfall

Questo modello include una fase di prototipazione per validare e analizzare i requisiti prima di procedere con il Waterfall. Aiuta a chiarire i requisiti, ma richiede competenze specifiche per costruire i prototipi, e c'è la pressione di mantenere il prototipo come prodotto finale.

#### Incremental

Il modello incrementale suddivide l'integrazione in più parti, con ogni iterazione che produce una parte del sistema che viene rilasciata al cliente in più incrementi. Questo modello offre un feedback più rapido e consente di ritardare gli incrementi che dipendono da componenti esterni.

## Agile

Le metodologie agili, come Scrum e XP (Extreme Programming), enfatizzano la flessibilità, la collaborazione e la risposta rapida ai cambiamenti. Gli sviluppatori lavorano in iterazioni brevi (sprint), consegnando frequentemente software funzionante e coinvolgendo il cliente nel processo.

- Adatto per progetti piccoli con team co-localizzati.
- Maggiore flessibilità e capacità di rispondere rapidamente ai cambiamenti.
- Favorisce la collaborazione continua con il cliente.

- Non sempre adatto per progetti grandi o distribuiti.
- Meno enfasi sulla documentazione dettagliata e sulla conformità ai requisiti iniziali.

## DevOps

DevOps integra le operazioni di sviluppo e di gestione del sistema, rimuovendo le barriere di comunicazione e conoscenza tra i due team. Utilizza pratiche come il build, il deployment e il test automatizzati, e si basa su CI/CD (Continuous Integration/Continuous Deployment).

- Riduce i tempi di rilascio e migliora la qualità del software.
- Permette deploy frequenti senza downtime.

- Richiede una cultura aziendale e un'organizzazione adeguata.
- Dipende fortemente da strumenti e automazione.

## Extreme Programming (XP)

XP è una metodologia agile che enfatizza pratiche come il pair programming, il test-driven development (TDD), e la semplicità nel design. Si basa su iterazioni brevi e consegne frequenti, con un forte coinvolgimento del cliente.

- Elevata qualità del codice grazie alle pratiche come il pair programming e il refactoring continuo.
- Rapidità nel rispondere ai cambiamenti nei requisiti.

- Richiede un forte impegno e disciplina da parte del team.
- Può essere visto come controverso per la mancanza di specialisti e la riduzione della documentazione.