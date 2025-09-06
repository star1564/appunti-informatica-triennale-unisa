---
aliases:
  - Oggetti Immutabili
tags:
  - corsi/informatica/programmazione_object_oriented
---
Il valore di una variabile **Final** *non può essere modificato* (costante), altrimenti si generà un errore in fase di compilazione.

```Java
final nomeTipo nomeVariabile;
```

Una variabile final è diversa da una costante poiché il suo valore non è necessariamente noto durante la compilazione. Questa può essere inizializzata solo una volta, tramite inizializzazione o assegnazione.

#### OGGETTI IMMUTABILI
Si usa `final` per rendere un oggetto **Immutabile**, ovvero istanze il cui stato non cambia dopo che sono state inizializzate.
Se un oggetto deve essere immutabile, non ci devono essere setter.

> [!example]- <font color="orange">Esempio</font>
>```java
>public final class Immutable {
>    private final int value;
>    
>    public Immutable(int value) {
>        this.value = value; // value può essere solamente inizializzata nel costruttore
>    }
>    
>    // Questo oggetto è solo read only
>    public int getValue() {
>        return value;
>    }
>}
>```


%%
[[000 Indice POO|↖ Ritorna all'indice ↖]]
%%