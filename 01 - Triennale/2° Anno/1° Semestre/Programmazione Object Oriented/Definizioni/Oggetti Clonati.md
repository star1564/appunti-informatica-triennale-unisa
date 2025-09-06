---
tags:
  - corsi/informatica/programmazione_object_oriented
---
Copiando il riferimento a un oggetto si ottengono semplicemente due riferimenti al medesimo oggetto:
```Java
BankAccount account = new BankAccount(10000);
BankAccount account2 = account;
account2.deposit(5000);
// Ora sia account sia account2 puntano a un conto avente saldo 15000
```

Se si vuole effettivamente creare una copia di un oggetto si deve utilizzare il metodo **`clone()`**. 
Questo restituisce un *nuovo* oggetto, avente lo stato identico a quello di un oggetto esistente. Si trova nella classe `Object`, [[015 Ereditarietà#^28eda7|superclasse]] di qualsiasi classe che creiamo.

Per invocarlo la classe appartenente deve implementare l'[[018 Interfacce|Interfacca]] `Cloneable`:
```Java
BankAccount clonedAccount = (BankAccount) account.clone();
```

![[Pasted image 20230207105106.png|500]]

Il tipo del riferimento che viene restituito dal metodo `clone` è `Object`, quindi, quando si invoca si deve usare un cast.

Questo è il metodo base che si può implementare in una classe (si usa l'[[Annotazioni#OVERRIDE|annotazione override]]):
```Java
public class BankAccount implements Cloneable{
	...
	@Override
	public Object clone(){
		return super.clone;
	}
}
```

> [!warning] Copia superficiale (shallow copy)
> Il metodo `clone` va usato con cautela, perché se un oggetto contiene un riferimento a un altro oggetto, il metodo `Object.clone()` crea una *copia di tale riferimento*, non un clone dell'oggetto stesso.

Supponiamo di avere una classe `Customer` contenente una `String` ed un `BankAccount`. Invocando il metodo, i riferimenti alle stringhe ed ai valori numerici vengono clonati correttamente. Però nell'oggetto clonato al posto di un `BankAccount` diverso ma con stessi valori, si ha una copia del riferimento allo stesso `BankAccount`. 

![[Pasted image 20230207105336.png|800]]
Per questo si sono messe delle protezioni come l'interfaccia `Cloneable`: se un oggetto non la implementa, viene lanciata una [[023 Gestione delle Eccezioni|eccezione]] (nota che anche se la implementa, bisogna lo stesso mettere il [[023 Gestione delle Eccezioni#GESTIRE LE ECCEZIONI|try-catch]]).

Se un oggetto contiene un riferimento a un altro oggetto *modificabile*, si deve invocare `clone` con tale riferimento:
```Java
public class Customer implements Cloneable{	
	private String name;
	private BankAccount account;
	...
	
	@Override
	public Object clone() throws CloneNotSupportedException {
		try {
			Customer clonato = (Customer) super.clone();      // Clonamento superficiale di tutti gli attributi,
															  // in questo caso name va bene perché è una stringa.
			clonato.account = (BankAccount) account.clone();  // Nell'oggetto clonato si modifica account con un clone creato correttamente.
			return clonato;
		} catch (CloneNotSupportedException e) {
			return null;
		}
	}
}
```

Con questo si ha una **Clonazione Totale**.

> [!note]- Clonazione di Oggetti Immutabili
> Nota che se una classe deve avere [[Keyword final|Oggetti Immutabili]] si inserisce `final` solamente nelle variabili che non sono riferimenti ad altre classi (come le stringhe). Se si vuole rendere immutabile anche il riferimento, si deve mettere `final` a tutte le variabili di esso.
>```Java
>public class Customer implements Cloneable{	
>	private final String name;
>	private BankAccount account; // Qui non si può mettere, altrimenti si avrà un errore.
>	...
>}
>
>public class Customer implements Cloneable{	
>	private final double balance;
>	...
>}
>```


%%[[000 Indice POO|↖ Ritorna all'indice ↖]]%%