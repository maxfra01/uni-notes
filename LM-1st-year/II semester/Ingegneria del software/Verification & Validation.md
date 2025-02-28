# Definizioni introduttive

### Verification & Validation

La validazione e la verifica sono due processi critici nel campo dell'ingegneria del software e di altri campi tecnici, come l'ingegneria dei sistemi e l'ingegneria dell'hardware. Ecco le definizioni e le spiegazioni di entrambi i concetti:
1. **Verifica**:
    
    - **Definizione**: La verifica è il processo di conferma che un prodotto, un servizio o un sistema soddisfi i requisiti specificati prima della sua consegna o implementazione. In termini più semplici, la verifica si concentra sul "fare bene le cose" e riguarda l'accuratezza e la correttezza del lavoro svolto.
    - **Spiegazione**: Durante la verifica, vengono esaminati i documenti, i progetti, il codice sorgente, o qualsiasi altra forma di prodotto per garantire che corrisponda ai requisiti specificati. Questo processo è spesso eseguito attraverso revisioni, ispezioni, test e analisi. L'obiettivo è rilevare e correggere eventuali difetti o discrepanze tra il prodotto e i requisiti in modo da garantire che il prodotto finale sia accurato e completo.
2. **Validazione**:
    
    - **Definizione**: La validazione è il processo di conferma attraverso prove oggettive che il sistema soddisfi i requisiti per cui è stato progettato e che sia adatto all'uso previsto. In altre parole, la validazione si concentra sul "fare le cose giuste" e riguarda l'idoneità e l'efficacia del prodotto rispetto agli obiettivi desiderati.
    - **Spiegazione**: Durante la validazione, il sistema completo viene testato o valutato per garantire che sia conforme alle aspettative dell'utente finale e che risolva effettivamente i problemi per cui è stato sviluppato. Questo processo coinvolge spesso test di accettazione dell'utente, simulazioni di utilizzo reali, valutazioni delle prestazioni e altro ancora. L'obiettivo è assicurare che il prodotto finale sia appropriato e soddisfi le esigenze degli utenti finali.

In breve, mentre la verifica si concentra sulla correttezza e l'accuratezza del lavoro svolto rispetto ai requisiti specificati, la validazione si concentra sull'efficacia e l'idoneità del prodotto rispetto alle esigenze degli utenti finali e agli obiettivi del progetto. Entrambi i processi sono essenziali per garantire la qualità e l'affidabilità dei prodotti e dei servizi tecnici.

![[Pasted image 20240513175216.png]]

### Traceability

Un ulteriore concetto importante è la **tracciabilità** del lavoro svolto: è necessario scrivere coerentemente i documenti dei requisiti e il codice (ad esempio inserendo un riferimento tra una funzione e il relativo requisito). Tramite questa *good practice* è possibile risalire ad errori nella fase di verification in modo semplice.

Man mano che scendiamo in profondità nello sviluppo di un progetto, la scoperta di nuovi difetti o errori diventano più costosi in termini di denaro e tempo:

![[Pasted image 20240513175728.png]]

### Failure, Fault and defect

- **Failure (Fallimento)**: Un evento di esecuzione in cui il software si comporta in modo inatteso.
- **Fault (Guasto o Difetto)**: La caratteristica del software che causa un fallimento. Può essere dovuto a un errore nel codice, requisiti incompleti o incorretti.
- **Defect (Difetto)**: Un errore o problema nel software o nel sistema che può causare un fallimento o un comportamento indesiderato. Un difetto può essere la causa di un guasto o di un fallimento.

A questo punto possiamo dire che l'obiettivo della fase V&V è quella di **minimizzare i difetti inseriti nel codice** (non possiamo scendere a 0, a causa della grande complessità del codice) e **massimizzare la scoperta di difetti e la relativa rimozione/correzione**.

### Debito tecnico

![[Pasted image 20240513180458.png]]

Definiamo il **debito tecnico** come gli sforzi che un'azienda dovrà effettuare per sistemare un progetto (più **interessi**). Il debito tecnico si verifica quando si decide di "prendere in prestito" tempo per rilasciare rapidamente una soluzione, sacrificando la pulizia del codice, l'efficienza, la manutenibilità o altri attributi desiderabili. Tuttavia, come qualsiasi debito, questo debito tecnico deve essere pagato con interessi nel tempo sotto forma di costi aggiuntivi di manutenzione.

# Testing

**Testing:**
- **Tipi di Testing:** Include tecniche statiche come ispezioni e analisi del codice sorgente, e tecniche dinamiche come i test di sistema e i test di unità.
- **Definizione:** È il processo di esecuzione di un sistema o di un'unità eseguibile in condizioni specifiche per osservare o registrare i risultati al fine di rilevare le differenze tra il comportamento effettivo e quello richiesto (fallimenti).
    
**Scopo del Test:**
- **Scopo:** Trovare difetti nei prodotti software. Un test è considerato riuscito se individua un difetto.

**Testing vs Debugging:**
- **Differenze:** Il testing cerca di individuare fallimenti, mentre il debugging cerca e rimuove i difetti corrispondenti. Sono attività diverse e possono essere eseguite da ruoli diversi in momenti diversi.

![[Pasted image 20240513184722.png]]

Per **test suit** si intende una collezione di tests correlati.

Un "**oracolo**" è un concetto che si riferisce a un meccanismo o a una risorsa che può essere utilizzata per determinare se il comportamento di un sistema è corretto o conforme alle specifiche.
L'oracolo può essere un insieme di dati di test predefiniti, una funzione di calcolo, un esperto umano o qualsiasi altra risorsa che può valutare l'output del sistema e confrontarlo con un risultato atteso. In sostanza, funge da punto di riferimento per valutare se il software sta producendo risultati accurati.

![[Pasted image 20240513215053.png]]

La teoria fondamentale del testing del software è un insieme di principi che definiscono i limiti e le possibilità del testing nella valutazione della correttezza del software. È importante comprendere questi principi per applicare il testing in modo efficace e aumentare la fiducia nella qualità del software.

- **Il testing esaustivo è impossibile:** Non è possibile testare ogni singolo input possibile per un programma.
- **Il testing si concentra sulla ricerca di difetti:** L'obiettivo è identificare gli errori nel programma, non dimostrare che sia perfetto.
- **Tesi di Dijkstra:** Il testing può solo rivelare la presenza di bug, non la loro assenza.
- **Selezione di casi di test efficaci:** Poiché il testing esaustivo è impraticabile, è fondamentale scegliere i casi di test giusti.
- **Criteri per la selezione dei test:** Affidabilità e validità sono due criteri importanti per la selezione dei casi di test.
- **Validità:** Un criterio valido garantisce che almeno un caso di test farà fallire il programma se contiene un errore.
- **Affidabilità:** Un criterio affidabile garantisce che tutti i casi di test superino insieme se il programma è corretto, o che tutti falliscano se è errato.
- **Teorema fondamentale:** Se viene utilizzato un criterio di selezione dei test affidabile e valido e tutti i casi di test superano, ciò dà fiducia che il programma sia corretto, ma non può dimostrarlo definitivamente.
- **Criterio di uniformità:** Questo criterio si concentra sulla selezione di test che coprono l'intera specifica del programma.
- **Teorema di Howden:** Afferma che non esiste un algoritmo in grado di garantire il rilevamento di tutti gli errori in un programma arbitrario.

# Black Box testing

Scriviamo test in base a **criteri** che raggruppano una categoria di possibili input.


# White Box Testing

Si basa sulla **copertura** di tutti gli elementi strutturali del codice. Occorre avere conoscenza completa del codice.
Per prima cosa dobbiamo definire il **Control Flow Graph** dove:
- I nodi sono rappresentati da istruzioni atomiche o decisioni
- Gli archi sono il trasferimento del controllo
Se abbiamo diverse istruzioni sequenziali possiamo accorparle in un **basic block**:

```js
function homeworkAverage(scores: number[]): number {
	let min = 99999;
	let total = 0;
	for (let i = 0; i < scores.length; i++) {
		if (scores[i] < min)
		min = scores[i];
		total += scores[i];
	}
	total = total - min;
	return total / (scores.length - 1);
}
```

| Senza basic block                    | Con basic block                      |
| ------------------------------------ | ------------------------------------ |
| ![[Pasted image 20240522085740.png]] | ![[Pasted image 20240522085749.png]] |

### Node coverage

Andiamo a definire la **node coverage**:
$$
\text{node coverage \%} = \frac{\text{number of nodes executed}}{\text{total number of nodes}}
$$
Questa quantità può essere definita sia per un singolo test, sia per una intera **test suite** (meglio).
Si parla anche di **statement coverage** che corrisponde sempre alla node coverage.

### Edge coverage

Con questa seconda metrica si prova a coprire tutte le decisioni possibili nel programma (tutti i rami degli if):
$$
\text{decision coverage \%} = \frac{\text{number of decision covered}}{\text{total number of decision}}
$$
Questo corrisponde sempre con il voler coprire tutti gli archi (edge coverage).
E' importante notare che se copro tutti gli archi, allora copro anche tutti i nodi: quindi posso dire che **la edge coverage implica la node coverage** (NON viceversa).

### Condition coverage

Ci si basa su tutte le condizioni nel codice:

![[Pasted image 20240522092458.png]]

![[Pasted image 20240522092553.png]]

### Path e Loop coverage

Per la **Loop**: si considera ogni ciclo (for while) separatamente e si scrivono per ognuno 3 test: uno per quando non si entra nel loop, uno per entrarci una volta, uno per entrarci più volte.

Il Path prevede di visitare ogni possibile percorso nel CFG.
