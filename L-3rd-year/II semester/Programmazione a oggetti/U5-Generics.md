Spesso vogliamo eseguire operazioni e manipolazioni su dati che sono di diversa natura: la prima soluzione è quella di usare dunque riferimenti **object** ma questo porta a diversi problemi di correttezza, tra cui integritò dei dati, downcast, ecc...
Si preferisce usare **classi e metodi generici**.

# Classi generiche
---
Per dichiarare una classe generica procediamo nel seguente modo:

```java
public class Pair<T> {
	T a,b;
	
	//constructor
	public Pair(T a, T b){
		this.a=a;
		this.b=b;
	}
	
	//list of methods
	public T first() {return this.a;}
	public T second() {return this.b;}
	public void set1st(T x){a=x;}
	public void set2nd(T x){b=x;}

}
```

La dichiarazione avviene poi specificando il tipo nel **diamond operator**:

```java
Pair<String> sp = new Pair<>("One", "Two");
Pair<Integer> ip = new Pari<>(1,2);
```

In questo modo il compilatore può verificare la compatibilità dei tipi.

Convenzionalmente si usa una unica lettera maiuscola per rappresentare i **parametri di tipo**.

```ad-note
Tutte le interfacce e classi da Java5 in poi sono dichiarate generiche per essere più sicure, compatte, semplici fa comprendere.
```

Anche i metodi possono essere dichiarati per tipi generici:

```java
public static <T> boolean contains(T[] ary, T element){
	//do stuff
	//using T
}
```

### Unbounded Types

In generale i tipi usati fino ad ora sono detti **unbounded** nel senso che non posseggono vincoli.
L'unico vincolo presente è che **T estende Object**: se proviamo ad usare .compareTo il compilatore dirà che il metodo non è definito per T, in quanto estende solo object.

### Bounded Types

A volte può essere utile dichiarare un tipo generico che estenta un classe per cui sono definiti metodi che ci fa comodo usare:

```java
< T extends B >
```

Per riprendere l'esempio di prima possiamo dichiarare T che estende Comparable e quindi potremo usare .compareTo

### Generic Subtyping

Sappiamo di per certo che la classe Integer è una sottoclasse di Object, ma questo **non vale per i tipi generici**, in quanto `Pair<Integer>` non è una sottoclasse di `Pair<Object>`

Parliamo di **Covarianza** quando un l'ereditarietà vale sia per gli elementi che per il contenitore.

Parliamo di **Invarianza** se appunto l'ereditarietà non è estesa anche al contenitore, rispetto agli elementi.

```ad-important
I tipi generici sono INVARIANTI.
```

### Wildcards

E' possibile usare la **wildcard Unknown** `<?>` per invocare metodi qualunque sia il tipo passato.
Il compilatore è molto cauto e non è possibile usare assegnazioni, ma solo metodi definiti da Object.


# Type Erasure
---
Il compilatore genera un'unica classe a partire da tutti i tipi possibili, mentre i tipi generici vengono cancellati e sostituiti con un **raw type**.

Con l'esempio delle coppie, anche se c'è una coppia di integer o una di stringhe, sono comunque istanze di una singola classe.

