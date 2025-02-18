# Regex in Java
---
Per usare le regex in Java devo importare `java.util.regex` che implementa due principali classi: **Pattern e Matcher**

**Pattern** crea la macchina a stati: accetta un'espressione regolare e tramite il metodo `compile` creo l'automata.

La classe **Matcher** è quella che confronta le espressioni con regex, tramite appunto il metodo `matches`.
I seguenti metodi si invocano su oggetti di tipo Matcher:
| Metodo      | Info                                                              |
| ----------- | ----------------------------------------------------------------- |
| matches()   | Cerca di confrontare l'intera stringa con la regex                |
| lookingAt() | Cerca di confrontare una porzione di stringa partendo dall'inizio |
| find()      | Matcha qualunque sottostringa                                                                  |

Le sequenze che vengono riconosciute sono accessabili tramite `group(i)`:
In particolare `group(0)` rappresenta tutta l'espressione regolare, mentre i gruppi 1,2... sono quelli racchiusi tra parentesi tonde.

Vediamo un esempio:

```java
Pattern p = Pattern.compile("([+-]?)([0-9]+)");

//stringa da confrontare 
Matcher m = p.matcher ("38 ...fe ud");
while (m.find()){
	System.out.println(
		"Group 1: " + m.group(1)
	);
}
```

```ad-note
Quando c'è un backslash nell'espressione regolare va raddoppiato, in quanto uso serve per riconoscere il backslash stesso all'interno di una stringa.
```

