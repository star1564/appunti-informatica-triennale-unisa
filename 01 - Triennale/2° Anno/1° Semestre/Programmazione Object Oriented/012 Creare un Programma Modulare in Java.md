---
aliases: 
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Basi della Programmazione ad Oggetti
cssclasses:
  - 
---
> [!example]+ <font color="orange">Esempio</font>
> >Si vuole creare un conta-persone, usato per il conteggio di persone.
>Ad ogni click del conta-persone, il contatore aumenta di uno. Il contatore deve anche avere la possibilità di resettarsi a zero e mostrare il conto attuale.

---
#### MODULARITÀ DEL PROGRAMMA
Per avere un programma modulare abbiamo bisogno di più file nella cartella del progetto. Per questo esempio abbiamo bisogno di due file:
- `Main.java`;
- `Counter.java`.

`Main.java` controllerà cosa dovrà fare il contatore, mentre all'interno di `Counter.java` sarà implementato tutto quello di cui abbiamo bisogno.

---
#### CREARE LE CLASSI
Inizieremo con la creazione della [[Classi|Classe]] `Main` e della classe `Counter` nei rispettivi file.
Nel main inseriamo anche il metodo `main`.

```Java
//Nel file Main.java
public class Main{
	public static void main(String[] args) {
		//...
	}
}

//Nel file Counter.java
public class Counter{
	//...
}
```

---
#### CREARE UNA VARIABILE ESEMPLARE
Per realizzare la classe `Counter`, dobbiamo decidere quali dati vengono memorizzati all'interno di ciascun oggetto contatore.
In questo esempio dobbiamo usare una [[011 Variabile Esemplare|Variabile Esemplare]] per conservare il numero di avanzamenti del conteggio.

Quindi nel nostro caso avremo:
```Java
//Nel file Counter.java
public class Counter{
	//Variabili di esemplare
	private int value;
	
	//...
}
```

---
#### CREARE I METODI
Ogni volta che l'operatore preme il pulsante del conta-persone, il valore del contatore viene incrementato di un'unità: modelliamo questa operazione mediante il metodo `click`.

Implementiamo i [[Metodi]] (che hanno lo stesso meccanismo delle funzioni nel C, specificamente le funzioni degli [[016 Introduzione agli ADT|ADT]]) specificando prima la modalità di accesso (pubblica o privata, se è pubblica il metodo è accessibile agli altri file), il valore di ritorno (void se non c'è), nome ed i parametri.

Scriviamo i metodi `click()`, `getValue()`, `reset()`.
```Java
//Nel file Counter.java
public class Counter{
	//Variabili di esemplare
	//...
	
	//Metodi

	//Metodo che incrementa il contatore
	public void click(){
		value = value+1;
	}

	//Ritorna il valore del contatore
	public int getValue(){
		return value;
	}

	//Azzera il contatore
	public void reset(){
		value = 0;
	}
}
```

---
#### DEFINIRE I COSTRUTTORI
Vogliamo avere la possibilità sia di inizializzare a zero un contatore, sia inizializzarlo ad un numero prestabilito. Creiamo quindi due costruttori:

```Java
//Nel file Counter.java
public class Counter{
	//Variabili di esemplare
	//...

	//Costruttori
	public Counter(){
		value = 0;
	}

	public Counter(int starting){
		value = starting;
	}

	//Metodi
	//...
}
```

---
#### CREARE UN OGGETTO
Per creare un nuovo oggetto in un file diverso da quello della classe si usa il **Costruttore di Oggetti**: 
```Java
new nomeClasse(parametri);
```
Questo *ritorna l'oggetto della classe specificata* con i parametri assegnati.

Scriveremo nel main:
```Java
//Nel file Main.java
public class Main{
	public static void main(String[] args) {
		Counter tally = new Counter(); //Crea un nuovo contatore
		tally.click(); //1
		tally.click(); //2
		int result = tally.getValue(); //Assegna a result il valore 2
		System.out.println(result);
	}
}
```

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
