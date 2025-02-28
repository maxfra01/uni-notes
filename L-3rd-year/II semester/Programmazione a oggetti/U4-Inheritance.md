# Introduzione all'ereditarietà
---
Una classe può essere un sotto tipo di una base class.
La nuova classe derivata **eredita attributi** e metodi della classe padre, mentre conterrà solo le differenze che ha con essa.

Il principale vantaggio dell'ereditarietà è il riuso delle classi già definite, senza dover riscrivere codice molto simile; inoltre fixando un bug nel padre lo si corregge in tutti i figli.

Il **Polimorfismo** è una caratteristica che permette di trattare diversi oggetti nello stesso modo, grazie appunto alla relazione gerarchica.

![[Pasted image 20230330161352.png]]

Questo livello gerarchico è descritto dal parametro **DIT, deapth of inheritance tree**, che in generale va mantenuto inferiore a 5.

### Override

E' possibile sovrascrivere un metodo nella classe figlia, così facendo se il metodo è chiamata da quest'ultima si eseguirà il codice sovrascritto:
```java
@override
public int oldMethod(){
	//implementation
}
```
La clausola `@Override` informa il compilatore della nostra intenzione di eseguire un override, quindi se ad esempio sbagliamo a scrivere la signature ci resistutirà un errore.
Se facciamo l'override non possiamo restringere la visibilità del metodo!

Un metodo **dichiarato `final`** non può essere overridato!

### Dynamic binding

Quando invoco qualcosa, il compilatore parte dalla classe del riferimento, poi procede in maniera bottom-up verso le classi più generiche cercando una definizione del metodo, questo solo a patto che ci sia la stessa signature e che sia stato overridato. 
Questo concetto è noto come **dynamic binding**.

```ad-note
Il **Duck tiping** è un alternativa al dynamic binding, dove i controlli sulla classe vengono fatti a run-time, non dal compilatore
```

### Casting

In Java, è possibile castare i tipi, sia in modo **implicito** che **esplicito**:
```java
int i=44;
float f=i; //implicit cast

float d= (float) i; //explicit cast
```
Queste conversioni possono essere fatti sulla gerarchie di una classe, padre e figlio.

### Upcast

Passaggio da una classe più specifica a una meno specifica.
L'upcast è sempre sicuro e fatto in modo automatico dal compilatore.
Inoltre ricordare che i cast **cambiano i riferimenti**, gli oggetti non vengono influenzati.

### Downcast

Procedura inversa, da classe meno specifica a più specifica.
Al contrario dell'upcast, il downcast non è sempre sicuro e **dev'essere sempre esplicito**.
Il programmatore deve prendersi la responsabilità di controllare la compatibilità.
Il compilatore si fida di ogni downcast ma è la JVM che da errore se qualcosa non è compatibile.

Il seguente operatore verifica se una certa reference può essere downcastata nella classe indicata:
```java
aReference instanceof aClass
```
In altre parole esso ritorna vero quando l'oggetto appartiene a quella data classe o a una della sue sottoclassi.

### Visibilità e Ereditarietà

Metodi e attributi possono essere dichiarati:
- `public` sempre accessibili
- `protected` accessibili dall'interno della classe, sottoclassi e classi del package
- `package` accessibili dall'interno della classe, e classi nel package
- `private` accessibili solo all'interno della classe stessa

![[Pasted image 20230330164101.png]]
### Costruttori e ereditarietà

Java mette automaticamente un costruttore (con o senza parametri) che viene eriditato dalla classe padre.
Nel caso di un costruttore specifico per una classe figlia, allora quello di default scompare.

Usando la clausola `super()` si fa una chiamata al costruttore padre passando i parametri corretti, inoltre super() deve essere la prima istruzione nel costruttore figlio