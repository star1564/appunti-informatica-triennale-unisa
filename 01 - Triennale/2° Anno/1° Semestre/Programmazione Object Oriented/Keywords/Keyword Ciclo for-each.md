---
aliases:
  - for-each
tags:
  - corsi/informatica/programmazione_object_oriented
---
Una importante variante del ciclo `for` è quella che potremmo definire **for-each** che serve nel caso particolare (ma comune) in cui si voglia eseguire un determinato blocco di codice per ogni elemento di una data collezione (come un [[Array (Java)|array]] o una [[Lista (Java)|lista]]).

```java
for(Tipo item : itemCollection ) {
	// ...
}
```

Questo si legge come:
> Prendi uno ad uno gli elementi della collezione `itemCollection` di un certo `Tipo`, assegna ciascuno di essi alla variabile `item` ed esegui per ciascun elemento il blocco (che potrà quindi usare `item` al suo interno).


%%
[[000 Indice POO|↖ Ritorna all'indice ↖]]
%%