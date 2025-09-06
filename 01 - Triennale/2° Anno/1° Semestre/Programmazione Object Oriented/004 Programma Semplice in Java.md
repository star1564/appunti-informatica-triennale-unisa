---
aliases: 
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Basi della Programmazione ad Oggetti
cssclasses:
  - 
---
Programma "*Hello, World!*" in Java:

```Java
public class HelloPrinter{
	public static void main(String[] args){
		System.out.println("Hello, World!");
	}
}
```

##### DICHIARAZIONE DELLA CLASSE
La prima riga (`public class HelloPrinter`) contiene la *dichiarazione* di una [[Classi|Classe]] di nome `HelloPrinter`.

La parola **`public`** indica che la classe è utilizzabile "pubblicamente" da altri file. 
Questa si omette dalla dichiarazione se la classe non deve essere usata da altri file.

Ogni programma Java completo è costituito *da una o più classi*, mentre ciascun file sorgente può contenere **al massimo una classe pubblica**. 

> [!warning] Nome di una classe pubblica
>Il nome di una classe pubblica deve *corrispondere* al nome del file che contiene la classe. 
>Quindi per l'esempio di sopra, il nome del file deve essere `HelloPrinter.java`.

##### METODO MAIN
Il seguente costrutto:

```Java
public static void main(String[] args){
	//codice
}
```

definisce un [[Metodi|Metodo]], chiamato **`main`**, da cui parte l'intero programma. Ogni programma Java completo deve avere **esattamente un solo metodo main**.
La maggior parte dei programmi Java contiene altri metodi oltre al main. 

- [[Keyword static|Static ↩]];
- [[3 Eclipse Help -  Passare gli args (Eclipse)|String[] args ↩]].

##### METODO DI STAMPA
Nel semplice programma di sopra, il metodo `main` ha solo un enunciato: 
```Java
System.out.println("Hello, World!");
```

Questo enunciato visualizza (o "stampa") *qualsiasi cosa gli venga passato come argomento*; alla fine della stampa, **va a capo**. 
In questo caso stampa la stringa "Hello, World!" e va a capo.
La versione `System.out.print` non va a capo.

Funziona anche per variabili di qualsiasi tipo:
```Java
System.out.println(varDaStampare); //Stampa il contenuto di varDaStampare
```

Come per le [[009 Tipo String|Stringhe]], si può usare l'operatore `+` per combinare diverse variabili e stringhe per ottenere un particolare output:
```Java
int i = 1;
char c = 'a';
System.out.println("Numero: " + i + "; Lettera: " + c); //Output: "Numero: 1; Lettera: a"
```

---
### COMPILARE ED ESEGUIRE CON TERMINALE
Per *compilare* un file `.java` con l'utlilizzo di un terminale si inserisce il seguente comando: **`javac Programma.java`**.
Questo produrrà un file `.class`.

Invece, per *eseguire* un file `.class` si inserisce il seguente comando: **`java Programma`** (senza mettere l'estensione). Il programma verrà lanciato in quel terminale.

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