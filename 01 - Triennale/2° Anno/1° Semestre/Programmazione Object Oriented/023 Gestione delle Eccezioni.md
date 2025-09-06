---
aliases:
  - Eccezioni
cssclasses:
  - 
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Caratteristiche della Programmazione ad Oggetti
---

In Java la **Gestione delle Eccezioni** è un meccanismo versatile che consente di *gestire condizioni di errore* al runtime in un programma, questo per preservare il flusso regolare dell'applicazione.

Ci possono essere molte condizioni di errore, ma queste possono essere solo di due tipi:
- **Errore**: rappresenta una *una situazione anomala ed irrecuperabile*. Esso indica un problema serio, dovuto a condizioni accidentali non prevedibili, quindi non gestibili dal programmatore. Questi non dovrebbero essere gestiti dai programmi. Per esempio:
	- La JVM esaurisce le risorse necessarie;
	- Incompatibilità di versioni;
	- Stack Overflow;
	- ecc...
- **Eccezione**: è un *evento non accettato o non atteso*. Indica un errore più comune ed è quindi gestibile all'interno del programma. Per esempio:
	- Input non corretto;
	- Divisione per zero;
	- ecc...

Quando si incontra una eccezione, è possibile trasferire il controllo dell'esecuzione del programma dal punto in cui viene segnalato l'errore ad un *gestore delle eccezioni* che sia in grado di gestirlo. Il suo compito è quello di:
- eseguire il codice previsto per il trattamento dell'errore;
- *riprendere l'esecuzione normale* oppure *terminare il programma* con la segnalazione dell'errore.

---
### TIPI DI ECCEZIONI
Le eccezioni si distinguono in **Eccezioni Controllate** ed **Eccezioni NON Controllate**.

![[Pasted image 20221122122045.png|500]]

Le eccezioni Built-in sono delle eccezioni che Java mette a disposizione in alcune librerie, mentre quelle User-Defined sono delle eccezioni personalizzate create dal programmatore per segnalare errori specifici del programma.

Le seguenti appartengono alle Built-in:
- *Eccezioni Controllate*: queste vengono controllate mentre si compila un programma, quindi vengono segnalate (compilation error) annullando la compilazione.
	- È **obbligatorio** gestire questo tipo di eccezione, sono degli errori causati da eventi esterni e richiedono l'attenzione del programmatore.

- *Eccezioni NON Controllate*: queste invece non vengono controllate dal compilatore, quindi non vengono segnalate durante la compilazione.
	- **Non è obbligatorio** gestire questo tipo di eccezione e possono essere evitate con un'attenta programmazione.

> [!example]- <font color="orange">Esempio</font>
>*ECCEZIONE CONTROLLATA*
>![[Pasted image 20230207143625.png|600]]
>Qui Eclipse dà un errore, perché l'operazione potrebbe lanciare una `FileNotFoundException` (eccezione controllata), quindi si deve per forza gestire.
>Ci avvisa alla (scrittura) compilazione del codice.
>
>*ECCEZIONE NON CONTROLLATA*
>```Java
>public static void main(String[] args){
>	String name = null;
>	printLength(name);
>}
>
>public static void printLength(String myString){
>	System.out.println(myString.length());
>}
>```
>
>Questo darà il seguente errore:
>```markup
>Exception in thread "main" java.lang.NullPointerException: Cannot invoke "String.length()" because "myString" is null
>```
>Perché la stringa è `null`, pertanto il metodo `length()` lancia una `NullPointerException` (eccezione non controllata). Non ci darà errori in compilazione, quindi non è necessario gestirla, ma è buona regola farlo.


---
### LANCIARE UN'ECCEZIONE
Gli errori e le eccezioni sono delle [[015 Ereditarietà#^28eda7|sottoclassi]] della classe **`Throwable`**.

![[Immagine 2022-11-22 121116.png|400]]

Per **lanciare** un'*eccezione* si usa la keyword `throw`, lanciando un oggetto di tipo "eccezione".

Quando si lancia un'eccezione, l'elaborazione passa al gestore delle eccezioni, che ferma l'esecuzione del programma e mostra un *errore personalizzato descritto nel codice*.

Si usa insieme al costruttore `new` ed una classe.

```Java
throw new NomeEccezione(argomenti);
```

> [!example]- <font color="orange">Esempio</font>
>```Java
>public class Main {
>	public static void main(String[] args) throws Exception {
>		int amount = 5, balance = 0;
>		
>		if(amount > balance)
>			throw new Exception("Amount exceeds balance");
>	}
>}
>```
>
>Output:
>```markup
>Exception in thread "main" java.lang.Exception: Amount exceeds balance
>at Main.main(Main.java:6)
>```

---
### GESTIRE LE ECCEZIONI
Per gestire una eccezione si usano le keyword **Try e Catch**.
Tutte le [[023 Gestione delle Eccezioni|eccezioni]] dovrebbero essere gestite in qualche punto del programma, altrimenti viene solo visualizzato un messaggio di errore facendolo terminare.

Per gestire eccezioni si usa l'enunciato **`try` / `catch`**, che va scritto in una zona del programma che sappia come gestire una particolare eccezione.

Il blocco `try` contiene enunciati che possono provocare il lancio di un'eccezione del tipo che si vuole gestire ed è seguito da clausole `catch`.

```Java
try{
	enunciato
	enunciato
	...
}
//Se try produce un eccezione del seguente tipo, si passa al catch 
catch (ClasseEccezione oggettoEccezione){
	enunciato
	enunciato
	...
}
```

> [!example]- <font color="orange">Esempio</font>
>```Java
>//Esempio, PARTI DA MAIN
>class ThrowExcep
>{
>	static void fun()
>	{
>		try //Si ha un'eccezione all'interno del try
>		{
>			throw new NullPointerException("demo");
>		}
>		//Quindi viene presa dal catch
>		catch(NullPointerException e)
>		{
>			//Gestisci cosa fare con l'eccezione:
>			//Stampa un messaggio per sapere cosa succede
>			System.out.println("Caught inside fun().");
>			throw e; 
>			//Rilancia l'eccezione ritornando nel main
>		}
>	}
>
>	public static void main(String args[])
>	{ //PARTI QUI
>		try //Vai a fun()
>		{
>			fun();
>		}
>		//fun() ha generato un eccezione
>		//Questa viene presa nel catch
>		catch(NullPointerException e)
>		{
>			//Gestisci cosa fare con l'eccezione:
>			//Stampa un messaggio per sapere cosa succede
>			System.out.println("Caught in main.");
>		}
>	}
>}
>
>```

---
### SEGNALARE ECCEZIONI
Per segnalare le eccezioni controllate che un metodo può lanciare si usa la keyword **`throws`**.

> [!example]- <font color="orange">Esempio</font>
>```Java
>public class Customer implements Cloneable{
>	private String name;
>	private BankAccount account;
>	
>	public Object clone() throws CloneNOtSupportedException{
>		Customer cloned = (Customer)super.clone();
>		cloned.account = (BankAccount)account.clone();
>		return cloned;
>	}
>	
>	...
>}
>```

---
### ECCEZIONI PERSONALIZZATE
Per creare un'eccezione personalizzata si può creare una nuova classe che [[Keyword extends|estende]] la classe `Exception`.

> [!example]- <font color="orange">Esempio</font>
>```java
>public class MyCustomException extends Exception {
>    // Costruttore
>    public MyCustomException(String message) {
>        super(message);
>    }
>}
>```
>
>```java
>public class MyCustomException extends Exception {
>    // Costruttore
>    public MyCustomException() {
>        super("Errore MyCustomException!");
>    }
>}
>```

___
[[000 Indice POO|↖ Ritorna all'indice ↖]]

```dataviewjs
/*
function extractUpperCaseLetters(inputString) {
	// Use a regular expression to match uppercase letters (A-Z)
	const uppercaseLetters = inputString.match(/[A-Z]/g);
	
	// Check if uppercaseLetters is not null, and join the matched letters into a string
	if (uppercaseLetters !== null) {
		return uppercaseLetters.join('');
	} else {
	    // If no uppercase letters were found, return an empty string
	    return '';
	}
}
*/

function extractNumberFromString(inputString) {
	const match = inputString.match(/^\d{3}/);
	
	if (match) {
		return match[0];
	} else {
		return null; // Return null if no match is found
	}
}

function startsWithNumber(inputString, targetNumber) {
  // Use a regular expression to check if the string starts with the target number
  const pattern = new RegExp(`^${targetNumber}`);
  return pattern.test(inputString);
}

function sort2Array(array){
	let res = []
	
	if (array[0] == undefined)
		res.push(array[1], array[1])
	else if (array[1] == undefined)
		res.push(array[0], array[0])
	else if (parseInt(extractNumberFromString(array[0])) > parseInt(extractNumberFromString(array[1])))
		res.push(array[1], array[0])
	else
		res.push(array[0], array[1])
	
	return res
}

let toDisplay = []
function searchPrevAndNext(tag, currentNumber) {
	const prevNumber = (parseInt(currentNumber) - 1).toString().padStart(3, "0");
	const nextNumber = (parseInt(currentNumber) + 1).toString().padStart(3, "0");
	
	let prevAndNext = [(dv.pages(tag).where(p => startsWithNumber(p.file.name, prevNumber) || startsWithNumber(p.file.name, nextNumber)).file.name)]
	
	// Prev = [0]; Next = [1]
	let sortedPrevAndNext = sort2Array(prevAndNext[0])
	
	if (currentNumber == "001"){ 
		// Se è la prima pagina aggiungi solo il prossimo
		let nextLink = "[[" + sortedPrevAndNext[1] + "|" + "Nota Successiva →" + "]]"
		toDisplay.push(nextLink)
	} else if (prevAndNext[0][1] == undefined){
		// Se è l'ultima pagina aggiungi solo il precedente
		let prevLink = "[[" + sortedPrevAndNext[0] + "|" + "← Nota Precedente" + "]]"
		toDisplay.push(prevLink)
	} else {
		let nextLink = "[[" + sortedPrevAndNext[1] + "|" + "Nota Successiva →" + "]]"
		toDisplay.push(nextLink)
		
		let prevLink = "[[" + sortedPrevAndNext[0] + "|" + "← Nota Precedente" + "]]"
		toDisplay.push(prevLink)
	}
	
	
}

if (dv.current().tags[0] == null || dv.current().tags[0] == undefined){
	dv.header(1, "Errore - Inserire il tag nelle proprietà del file")
} else {
	let tag = "#" + dv.current().tags[0]

	// Purtroppo obsidian non riesce a leggere i link e a creare connessioni nel grafo,
	// quindi ho disattivato il link all'indice automatico e la funzione extractUpperCaseLetters
	//let indexName = "000 Indice " + extractUpperCaseLetters(tag)
	//let index = dv.page(indexName).file
	//let indexLink = "[[" + index.name + "|" + "↖ Ritorna all'indice ↖" + "]]"
	//toDisplay.push(indexLink)
	
	let currentPage = dv.current().file
	let currentPageNumber = extractNumberFromString(currentPage.name)
	
	searchPrevAndNext(tag, currentPageNumber)
	
	dv.list(toDisplay)
}
```

Altri collegamenti: 
- [Video differenza tra eccezioni controllate e non](https://www.youtube.com/watch?v=bCPClyGsVhc)