# Pipelining
---

Uno **Stream** è una sequenza di elementi che viene generata da una **sorgente** che supporta una serie di **processazioni**.
Alcune features fornite dall'API di Java sono **pipelining, iterazioni interne e lazy evalaution**.
Lo stream risulta molto comodo per: prima memorizzare i dati e successivamente elaborarli.

![[Pasted image 20230424160206.png]]

Consideriamo una sorgente che può essere un array di parole, e delle generiche operazione.

```java
//stadio source
Stream.of("All","Along",...);
//intermediate
.sorted();
//...
//Terminal
.forEach(System.out::println);
```

### Operazioni di sorgente

Uno stream alla sorgente è costruito in tre modi:

![[Pasted image 20230424160425.png]]

```java
String[] s={"Red", "Green"...};
Arrays.stream(s).forEach(...);
```

```java
Collection<Student> oopClass = new LinkedList<>;
//insert element into collection
Collection.stream(oopClass).forEach(...);
```

Altre operazioni che sono in grado di generare uno stream sono le seguenti:

![[Pasted image 20230424160740.png]]

Nel primo caso il **Supplier** può essere costituito da una funzione che genera un numero casuale:

```java
Stream.generate(
	()-> Math.random()*10
);
```

Nel secondo caso si deve fornire un operatore unario, cioè che accetta un solo parametro:

```java
Stream.iterate(2,
	(prev) -> prev+2			  
);
```

Tutti questi metodi **generano stream infiniti** quindi è bene fare dei controlli per non andare avanti all'infinito.

Fin'ora i metodi visti usano tipi generici ma in Java sonoo definiti alcuni stream per oggetti primitivi:

```java
DoubleStream;
IntStream;
LongStream;
```

E' possibile convertire uno stream generico a uno stream primitivo tramite il metodo `mapToX()` dove `X` è un tipo primitivo per il quale è disponibile lo stream.
Siccome sono tipi primitivi gli stream hanno metodi ulteriori, come quello per la media.

### Operazioni intermedie

Le principali operazioni sono qui riassunte:

![[Pasted image 20230424161530.png]]

Nel metodo filter è possibile usare un operatore :: se esiste un metodo che fornisce un booleano, oppure è possibile usare lambda function.

### Operazioni terminali

![[Pasted image 20230424162255.png]]

![[Pasted image 20230424162524.png]]

FindAny può non ritornare il primo elemento che corrisponde, in quanto se è possibile lavorare in parallelo è possibile che il primo dato venga elaborato con un ritardo maggiore rispetto a un altro (Medesima cosa per anyMatch()).

Quando consideriamo le operazioni terminali possiamo distinguerle in due gruppi: **Stateless** e **Stateful**, ovvero se richiedono memoria aggiuntiva.
Gli stateful si dividono in **Bounded** e **Unbounded** se richiedono una quantità finita di memoria (limit), oppure  che ne richiedono infinita (collect).

### Metodo Reduce

```java
T reduce (T identity, BinaryOperator<T> merge);
```

Il seguente metodo compatta gli elementi andando ad assegnare all'identity un merge con il nuovo dato che arriva:
Ad esempio con il seguente codice si ha lo scenario sotto:

```java
ooPClass.stream().
	map(Student::getFirst).
	map(String::lenght).
	reduce(0,Math::max);
```

![[Pasted image 20230424163224.png]]

### Metodo Collect

Questo metodo prende 3 argomenti: un supplier, una funzione di accumulazione e un combiner che serve per andare a combinare più stream paralleli.

```java
classs Acc{int n};

int s = Stream.of(numbers).
	collect(Acc::new,
	(a,i) -> a.n+1.
	(a1,a2)->a1.n+a2.n);
```

Il supplier è il contenitore che ospitera i dati, in molti casi l'istanza di una classe.
La funzione di accumulazione specifica come inserire nel supplier i nuovi elementi dello stream.
Il combinatore è necessario solo per combinare stream paralleli.

### Lazy evaluation

L'elaborazione sui dati vengono svolte solo se necessario, quindi il workflow sarà:
- creazione della pipeline
- Successivamente quando si chiama una funzione terminale si esegue tutto


# Collettori predefiniti
---
Sono degli oggetti che vengono utilizzati per raccogliere gli elementi in uno stream.
Sono utilizzabili in modo statico e sono definiti in:

```java
import static java.util.stream.Collectors.*;
```

Sono argomenti che, passati opportunamente al metodo collect performano operazioni predefinite

### Summarizing collectors

![[Pasted image 20230426161925.png]]

### Accumulating collectors

![[Pasted image 20230426161944.png]]

Per esemio in uno stream di parole, se devo trovare le prime 3 parole più lunghe posso procedere nel seguente modo:

```java
List<String> longestWords = Stream.of(text)
	.filter(w -> w-lenght()>10)
	.distinct()
	.sorted(comparing(String::lenght).reserved())
	.limit(3)
	.collect(toList());
```

Lo stream viene prima filtrato passando un predicato a `filter`, cosicchè rimangano solo le parole con più di 10 caratteri, poi eliminiamo i duplicati, ordiniamo per lunghezza maggiore, prendiamo i primi 3 e salviamo in una lista con un collettore predefinito.

### Grouping collectors

![[Pasted image 20230426162819.png]]

Da uno stream andiamo a creare una mappa che contiene come chiavi un'informazione estrapolata dal **classifier** e poi come valori una lista di elementi dello stream che rispetta la condizione o che rientra nella categoria.

Ad esempio dato un insieme di stringhe che voglia raggruppare per lunghezza allora avrò:

```java
Map<Integer,List<String>> byLenght =
	Stream.of(text).distinct
	.collect(groupingBy(String::lenght));
```


### Collector composition

![[Pasted image 20230426163259.png]]


# Esempi a lezione
---
Reduce accetta un binary operator, oppure c'è una variante che accetta un seed, e un binary operator

```java
//Concatenazione di stringhe con reduce
Stream.of(parole).
	reduce("testo:", (a,b)-> a+ " "+b);
```


