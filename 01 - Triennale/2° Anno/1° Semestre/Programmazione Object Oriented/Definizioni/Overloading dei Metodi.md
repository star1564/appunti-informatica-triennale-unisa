---
aliases:
  - Overloading
cssclasses:
  - 
tags:
  - corsi/informatica/programmazione_object_oriented
---
Una tra le caratteristiche note di Java è l'**Overloading di Metodi**, che ci consente di "sovraccaricare" un [[Metodi|Metodo]] o [[Costruttore di Oggetti|Costruttore]] di una classe con diverse varianti, in base ai parametri passati come riferimento.

```java
public class Main {
	
	static void metodoA(Integer i) {
		System.out.println("Primo metodo: " + i);
	}
	
	static void metodoA(Double d) {
		System.out.println("Secondo metodo: " + d);
	}
	
	public static void main(String[] args) {
		int i = 10;
		metodoA(i);
	}
}
```

Non viene segnalato errore anche se i due metodi hanno lo stesso nome, perché hanno parametri diversi.

In questo caso la JVM capisce che al `metodoA` gli viene passato un `int` a cui viene effettuato un [[008 Boxing, unboxing e autoboxing#AUTOBOXING|autoboxing]] ed esegue il primo metodo. Stessa cosa se gli passavamo un `Integer`.

Se invece gli passavamo un `double` o la sua controparte [[008 Boxing, unboxing e autoboxing|Wrapper]], il programma eseguiva il secondo metodo.

%%[[000 Indice POO|↖ Ritorna all'indice ↖]]%%