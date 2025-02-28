# Collections framework
---
All'interno del package **java.util** abbiamo classi, interfaccie, algoritmi che operano sulle collezioni di oggetti e che vanno a definire il **collections framework**.
Tutti gli elementi utilizzano **tipi generici** da Java 5.

Le interfaccie sono divise in due categorie:
- Contenitori di gruppo (insiemi, liste, code)
- Contenitori associativi (coppie chiave-valore)

I contenitori di gruppo implementano tutti **iterable**, mentre un'altra interfaccia è **collection** che estende iterable, per fare altre operazioni come aggiungere o rimuovere elementi in una collezione.

Il collection framework offre numerose classi che appunto implementano queste interfaccie:

![[Pasted image 20230418220820.png]]


# Contenitori di Gruppo
---

### Collection

**Collection** è un gruppo di elementi (riferimenti a oggetti). Non si specifica se gli elementi siano ordinati o se siano presenti ordinati, e questo lo rende un interfaccia molto generica.

Inoltre tutte le classi che implementano Collection hanno sempre due costruttori: il primo senza parametri che crea la collezione, e un secondo costruttore che ricevuta come parametro una collezione e che copia i riferimenti.

```java
//first contructor, initialize the container
public C();
//Second constructor, copy c elements in the new collection
public C(Collection c);
```

Alcuni esempi di metodi dell'interfaccia Collection sono:

```java
int size();
boolean isEmpty();
//...
Object[] toArray();
void clean();
```

### List

Interfaccia che permette di avere **elementi duplicati**, l'**ordine d'inserzione è preservato** e l'utente può decidere se accedere in testa o in coda.

`ArrayList` e `LinkedList` sono due classi che implementano nativamente L'interfaccia List. Esse differiscono per la maniera in cui sono gestiti i dati.

Si preferisce dichiarare il riferimento `l` di tipo `List` e specificare dopo se arraylist o linkedlist, in modo da poter eventualmente cambiare tipo di lista in seguito e non dover modificare i metodi (che fanno riferimento a `List`).

```java
List<Integer> l = new ArrayList<>();
```

### Queue

L'interfaccia implementa una collezione per realizzare **code FIFO** e code **a priorità**:
- Il metodo `peek()` ritorna il primo elemento della coda
- Il metodo `poll()` ritorna il primo elemento e lo rimuove dalla coda

Queue è implementato dalle classi `LinkedList` e `PriorityQueue`

### Set

Questa interfaccia **non ha metodi**, ma eredita solo quelli di collection. Il metodo `add();` non permette di inserire un elemento già presente nella collezione, o meglio lo scarta senza dare errori.
Iterator attraversa gli oggetti senza un particolare ordine.

### SortedSet

Come Set ma Iterator scorre gli elementi secondo l'ordine naturale o secondo il comparatore che passa alla costruzione.

Hashset implementa Set, LinkedHashSet implementa SortedSet.

```ad-note
Per le collezioni ordinate è possibile sempre passare un comparatore per definire l'ordinamento
```


# Contenitori associativi
---
L'**interfaccia Map** è un container che associa chiavi a valori, dove le **chiavi sono uniche**.
Inoltre chiavi e valori devono **essere oggetti**.
I due metodi principali sono:

```java
//insert a new pair in the value
public put(K key, V value);
//retrieve the value
public get(K key)
```

Le **SortedMap** usano l'ordine naturale per attraversare i dati, o si può specificare il costruttore.
La classe **HashMap** implementa Map.