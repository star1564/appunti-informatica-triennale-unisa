---
tags:
  - corsi/informatica/programmazione_object_oriented
---
> [!failure] Gli array vengono usati pochissimo in Java.

Un **Array** in *Java* è un contenitore che permette di gestire una sequenza di lunghezza fissa di elementi tutti del medesimo tipo. 

La lunghezza di un array, deve essere dichiarata ala momento della sua allocazione e nun può essere cambiata.

Un array si dichiara in questo modo:
```Java
Tipo[] nome;
```

`Tipo` può essere sia un tipo primitivo che una classe. 

Per default le variabili di tipo array sono inizializzate con il valore `NULL`. Quindi, prima di poterle usare, bisogna inizializzarle allocando la memoria per mezzo di new:

```Java
int[] num = new int[12];

num[0] = 31;
num[9] = 10;
//...
```


%%[[000 Indice POO|↖ Ritorna all'indice ↖]]%%