# Class diagram

Tramite **Unified Model Language** (UML) possiamo modellare i concetti di classe, istanza, attributo, operazione (metodo) e associazione (relazione logica fra concetti).

Per modellare  un **oggetto/istanza** possiamo considerare un entità fisica e/o non fisica (coinvolta nel software) caratterizzato da identificatore, attributi (o proprietà), operazione (metodi, comportamenti) e messaggi che può ricevere.
Nel caso di uno studente possiamo identificarlo con nome, cognome e matricola.

Il concetto di **classe** descrive un **insieme di oggetti** con caratteristiche comuni: quando crea un'istanza di una classe vado a creare un oggetto di quel tipo:

Ogni classe è caratterizzata da **attributi**: ogni proprietà deve avere un nome e il tipo di dato corrispondente.

### Uso dei class diagram

Le applicazioni del class diagram sono:
- Modellazione del **glossario**
- Modellazione di un **sistema** (HW e SW) 
- Modellazione di **classi software** che implementeranno ciò che si sta definendo
- Modellazione delle classi a livelli diversi: business, data, presentation

### Collegamenti in UML

Spesso una proprietà non può essere rappresentata da un unico oggetto: ad esempio considerando un sistema che gestisce l'iscrizione degli studenti in atenei, dobbiamo considerare che uno studente (della classe Studente) è iscritto a un ateneo (classe Università).
Per rappresentare questo concetto possiamo usare delle **associazioni**, ovvero collegamenti fra oggetti di classi diverse o fra oggetti della stessa classe.
Possiamo inoltre fornire il nome che assume una classe in una relazione e la relativa molteplicità.

![[Pasted image 20240311182206.png]]

```ad-note
E' buona norma:
- Dare un singolo nome alle classi
- Usare un verbo per descrivere un'associazione 
- Non inserire il tipo degli attributi nei modelli concettuali
```

E' possibile inserire un'entità in un'associazione, detta **classe di associazione**.

![[Pasted image 20240313084227.png]]

### Specialization, Generalization

Consideriamo due entità A e B: si che **B specializza A** e ciò significa che B ha le stesse proprietà di A, più alcune proprietà specifiche di B.
B è un **caso particolare** si A, mentre **A** è una generalizzazione di B.

![[Pasted image 20240313084116.png]]

Di base il meccanismo corrisponde all'**ereditarietà** della classi, quindi le classi figlie ereditano attributi e metodi.

# Do/Do not nei class diagram

- Decidere l'obiettivo della modellazione
- Tenere in considerazione diversi fattori come le entità, ruoli, organizzazione, geografia, eventi, intervalli di tempo e documenti
- Non usare il plurale per le classi
- Ricordarsi le molteplicità, e ricordarsi di inserire le associazioni

![[Pasted image 20240313085112.png]]

- Non usare attributi composti, ovvero non usare come attributi una classe
- Non usare attributi che rappresentano più elementi, quell'attributo va trasformato in una classe con molteplicità multipla
- Non usare delle rappresentazioni dinamiche per le relazioni tra classi
- Non ripetere gli attributi (evitare ridondanze)
- Cercare di non creare loop con le relazioni, nel caso sia inevitabile controlliamo che abbia senso
- Non modellare oggetti che appartengono al **software design**: ad esempio non modellare collezioni, GUI, bottoni ...
- Le sottoclassi non possono cambiare: se un oggetto fa parte di una sottoclasse lo rimane per sempre. Nel caso sia concesso cambiare, allora è meglio rappresentare le sottoclassi attraversi i ruoli

# Deployment diagram

