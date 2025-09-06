---
tags:
  - corsi/informatica/programmazione_object_oriented
---
Una **Lista in Java** funziona in maniera simile alle [[020 Liste nel C|Liste nel C]].

```Java
ArrayList<Tipo> nome = new ArrayList<Tipo>();
```

L'`ArrayList` funziona esattamente come un'[[Array (Java)|array]] a dimensione variabile.

Per esempio:
```Java
ArrayList<Integer> listaInteri = new ArrayList<Integer>();

int index = 0; //Indice della lista
Integer val1 = 60, val2 = 10;

listaInteri.add(index, val1);
index++;
listaInteri.add(index, val2);

System.out.println(listaInteri);
```

```
Output: [60, 10]
```

[Metodi Lista](https://www.javatpoint.com/java-list).

%%[[000 Indice POO|↖ Ritorna all'indice ↖]]%%