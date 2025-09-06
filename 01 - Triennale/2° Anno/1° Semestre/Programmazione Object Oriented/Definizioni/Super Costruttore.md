---
tags:
  - corsi/informatica/programmazione_object_oriented
---
Il costruttore **`super()`** richiama il [[Costruttore di Oggetti|Costruttore]] della [[015 Ereditarietà#^28eda7|Superclasse]] attuale.

Si può invocare nel costruttore della sottoclasse ed aggiunge all'oggetto in costruzione le specifiche passate.

![[Pasted image 20221022093542.png|650]]

```Java
// Superclasse
public class Bottiglia {
	// Dati (modificabiil dalle classi derivate)
	protected String marca;
	protected String dimensione;
	
	// Costruttore
	public Bottiglia(String m, String d){
		this.marca = m;
		this.dimensione = d;
	}
	
	// ...
}

// Sottoclasse (in un altro file)
public class BottigliaInVendita extends Bottiglia{
	// Dati (oltre a quelli di Bottiglia)
	private double costo;

	// Costruttore
	public BottigliaInVendita(String m, String d, double c){
		super(m, d);
		this.costo = c;
	}
	
	// ...
}
```

- [[Keyword protected|protected ↩]].

![[Keyword super|super]]


%%[[000 Indice POO|↖ Ritorna all'indice ↖]]%%