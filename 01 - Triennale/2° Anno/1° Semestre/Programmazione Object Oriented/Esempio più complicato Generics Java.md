
> [!quote] Esempio
> Supponiamo di voler modellare il comportamento di un braccio meccanico che deve afferrare una bottiglia con un determinato contenuto posta su un tavolo. Il comportamento del braccio sarà lo stesso qualunque sia il contenuto della bottiglia che afferra. 

Il movimento del braccio può essere visto come l'algoritmo indipendente dal tipo di bottiglia sul quale agisce. Il contenuto della bottiglia determina invece un parametro generico. Ora modelliamo il tutto in classi Java che facciano uso di Generics.

Iniziamo con il definire due semplici classi per le tipologie di contenuto di una bottiglia:

```java
public class Acqua {
	@Override
	public String toString(){
		return "una bottiglia d'acqua.";
	}
}
```

```java
public class Vino {
	@Override
	public String toString(){
		return "una bottiglia di vino.";
	}
}
```

Definiamo quindi la classe `Bottiglia` come classe Generics:

```java
public class Bottiglia<T> {
	private T contenuto; //Dichiarazione del contenuto della bottiglia (tipo T)
	
	public Bottiglia(T t){ //Costruttore della bottiglia al quale passiamo una classe come parametro
		contenuto = t;
	}
	
	public T getContenuto() {
		return contenuto;
	}
}
```

Una classe Generics specifica l'uso di un parametro di tipo con la notazione `<NomeTipo>`, una convezione sintattica prevede di usare lettere maiuscole per il carattere, alcuni caratteri tipici sono T, E, V e K. 

Nel nostro caso abbiamo definito una bottiglia in cui il parametro variabile è rappresentato dal suo contenuto `T`, il parametro `T` può essere visto come un *segnaposto*. All'atto della creazione dell'oggetto sarà sostituito, come vedremo tra poco, da un tipo classe specifico.

Continuiamo con la definizione della classe relativa al braccio meccanico:

```java
public class BraccioAutomatico {
	public void prendiBottiglia(Bottiglia<?> bottiglia){
		System.out.println("Ho preso " + bottiglia.getContenuto());
	}
}
```

Con la [[019 Wildcards (Java)|Wildcard]] **`?`** stiamo dichiarando una "bottiglia generica", indipendentemente dal suo contenuto. La classe modella quindi il comportamento del braccio che è in grado di afferrare una bottiglia senza preoccuparsi del suo contenuto. 

Completiamo invece l'esempio con una classe `Demo` che illustri il funzionamento:

```java
public class Demo {
	public static void main(String[] args) {
		Bottiglia<Acqua> bottiglia1 = new Bottiglia<Acqua>(new Acqua()); //Bottiglia di acqua
		Bottiglia<Vino>  bottiglia2 = new Bottiglia<Vino>(new Vino()); //Bottiglia di vino
		BraccioAutomatico braccio = new BraccioAutomatico(); //Braccio automatico
		
		braccio.prendiBottiglia(bottiglia1);
		braccio.prendiBottiglia(bottiglia2);
	}
}
```

Se eseguiamo il codice avremo come stampa sulla console:

```
Ho preso una bottiglia d'acqua.
Ho preso una bottiglia di vino.
```

Possiamo notare come in fase di creazione dell'oggetto `Bottiglia` sia stato specificato, al posto del parametro, il tipo classe `Acqua` in un caso e il tipo classe `Vino` nell'altro, definendo cosi due tipi di bottiglia. 
Questa sintassi implica che abbiamo costruito un oggetto di tipo `Acqua` e `Vino` direttamente alla costruzione del tipo `Bottiglia`.

La classe `BraccioAutomatico` eseguirà quindi la stessa azione per due tipi di bottiglie differenti. 

%%
#corsi/informatica/programmazione_object_oriented 
%%