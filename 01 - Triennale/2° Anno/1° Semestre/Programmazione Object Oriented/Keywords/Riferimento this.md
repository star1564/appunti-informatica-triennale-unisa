---
tags:
  - corsi/informatica/programmazione_object_oriented
---
Il Riferimento `this` si riferisce all'*oggetto corrente* in un metodo o costruttore.

È usato principalmente per non confondere gli attributi ed i parametri di una classe con gli stessi nomi.

```java
public class Main {
  private int x;

  // Costruttore con lo stesso nome della classe
  public Main(int x) {
    this.x = x; 
    //La variabile parametro ha lo stesso nome dela variabile privata
  }

  public static void main(String[] args) {
    Main myObj = new Main(5); //Chiamata
    System.out.println("Value of x = " + myObj.x);
  }
}
```



%%
[[000 Indice POO|↖ Ritorna all'indice ↖]]
%%