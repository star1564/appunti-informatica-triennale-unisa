---
aliases:
  - super
tags:
  - corsi/informatica/programmazione_object_oriented
---
Attraverso la Keyword **`super`** possiamo invocare un [[Metodi|Metodo]] della [[015 Ereditarietà#^28eda7|Superclasse]] attuale.

```Java
public class SuperClasse{
	public void metodo(){/*...*/}
}

class SottoClasse{
	public void metodo(){
		super.metodo(); 
		// In questo caso si va a chiamare il metodo con lo stesso nome all'interno della superclasse
	}
}
```



%%
[[000 Indice POO|↖ Ritorna all'indice ↖]]
%%
