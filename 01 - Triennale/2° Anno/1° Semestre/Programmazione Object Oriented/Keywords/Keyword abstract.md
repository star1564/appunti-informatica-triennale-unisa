---
aliases:
  - Classe Astratta
  - Classi Astratte
tags:
  - corsi/informatica/programmazione_object_oriented
---
Una [[Classi|Classe]] dichiarata con la keyword **`abstract`** è detta **Astratta** e contiene [[Metodi]] astratti, che devono essere specificati nella implementazione.

```Java
abstract class Bike{  
	abstract void run();  
}

class Honda4 extends Bike{  
	void run(){
		System.out.println("running safely");
	}  
	
	public static void main(String args[]){  
		Bike obj = new Honda4();  
		obj.run();  
	}  
}
```


%%
[[000 Indice POO|↖ Ritorna all'indice ↖]]
%%
