---
tags:
  - corsi/informatica/programmazione_object_oriented
---
Una **Interfaccia Funzionale** è una [[018 Interfacce|Interfaccia]] che *contiene un solo metodo astratto*.

Potrebbe essere utile scrivere l'[[Annotazioni|annotazione]]  `@FunctionalInterface` al di sopra dell'interfaccia. Questo fa capire al compilatore che si sta creando una interfaccia funzionale, quindi darà problemi se ci sono errori per questo tipo di codice.

```Java
@FunctionalInterface
public interface MyInterface{
	//Metodo Astratto
}
```



%%
[[000 Indice POO|↖ Ritorna all'indice ↖]]
%%
