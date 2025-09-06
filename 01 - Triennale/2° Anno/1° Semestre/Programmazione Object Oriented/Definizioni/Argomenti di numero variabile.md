---
aliases:
  - varargs
  - Variable Arguments
cssclasses:
  - 
tags:
  - corsi/informatica/programmazione_object_oriented
---
I **varargs** (*Variable Arguments*), sono un meccanismo per definire un numero di argomenti indefinito all'interno di un [[Metodi|Metodo]].

Utilizzando il modificatore **`...`** è possibile definire la presenza di un numero variabile di argomenti.

```java
// Esempio di utilizzo di varargs
public class Prodotto {
	private int id;
	// ...
	public Prodotto(int id, String... desc)
	{
		// ...
	}
}
```

Il metodo si aspetta *0 oppure N parametri* di tipo `String` da utilizzare indipendentemente dalla logica del metodo o del [[Costruttore di Oggetti|Costruttore]].

Il vantaggio principale nell'usare i varargs sta nel poter passare parametri ai metodi in modo più sintetico:

```java
// chiamata con varargs
metodoX(1, "uno","due","tre");
// chiamata senza varargs
metodoX(1, new String["uno","due","tre"]);
```

Ecco un semplice programma per testare il funzionamento del costrutto:

```java
// Test del varargs
public class Main{
	public static String concatena(String... strings) {
		String toRet = "";
		for (String string : strings) {
			toRet += string;
		}
		return toRet;
	}
	
	public static void main(String[] args){
		System.out.println(concatena("Test1", "Test2", "Test3"));
	}
}
```

Come si vede dal corpo del metodo utilizziamo il costrutto [[Keyword Ciclo for-each|for-each]] e di fatto utilizziamo il parametro `strings` come un array.
Si potrebbe anche utilizzare la sintassi di un array:
```Java
String s = strings[0];
```

> [!warning]- Overloading con varargs
> Ovviamente l'[[Overloading dei Metodi]] non funziona se come *UNICO* parametro c'è un varargs.


%%
[[000 Indice POO|↖ Ritorna all'indice ↖]]
%%