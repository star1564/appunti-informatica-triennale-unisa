---
aliases:
  - generics
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Caratteristiche della Programmazione ad Oggetti
cssclasses:
  - 
---
> [!success] [Video Consigliato](https://www.youtube.com/watch?v=K1iu1kXkVoA)


I **Generics** sono stati introdotti a partire dalla versione J2SE 5, il termine ha un significato di **tipo parametrizzato**. 
I tipi parametrizzati rivestono particolare importanza perché consentono di creare [[Classi]], [[018 Interfacce|interfacce]] e [[Metodi]] per i quali *il tipo di dato sul quale si opera può essere specificato come parametro*. Parliamo quindi di classi, interfacce e metodi *generici*.

Attraverso un codice generico possiamo realizzare un algoritmo che funzioni su qualsiasi tipo di dato. Per esempio un'algoritmo di ordinamento deve funzionare su qualsiasi tipo di numero (a volte anche stringhe).

Supponiamo di voler creare una classe che stampi il contenuto di un oggetto.
Senza l'utilizzo dei generics dovremmo creare una classe per ogni tipo di oggetto che vogliamo stampare:
```Java
// Per interi
public class PrinterInteger{
	Integer daStampare;
	
	public PrinterInteger(Integer i){
		this.daStampare = i;
	}
	
	public void print(){
		System.out.println(this.daStampare);
	}
}

// Per double
public class PrinterDouble{
	Double daStampare;
	
	public PrinterDouble(Double d){
		this.daStampare = d;
	}
	
	public void print(){
		System.out.println(this.daStampare);
	}
}

// ...
```

Una classe Generics specifica l'uso di un parametro di tipo con la notazione `<NomeTipo>`, una convezione sintattica prevede di usare lettere maiuscole per il carattere, alcuni caratteri tipici sono T, E, V e K. 

I generics permettono in questo caso di creare una sola classe che funzioni per tutti i tipi che vogliamo. Rappresentiamo il tipo variabile con `T`, il parametro `T` può essere visto come un *segnaposto*. 
All'atto della creazione dell'oggetto sarà sostituito da un tipo classe specifico.

Creiamo un generico `Printer` ed il `main`:
```Java
// Per tutti i tipi
class Printer<T>{
	T daStampare;
	
	public Printer(T daStampare){
		this.daStampare = daStampare;
	}
	
	public void print(){
		System.out.println(this.daStampare);
	}
}

// Main
public class Main{
	public static void main(String[] args){
		Printer<Integer> printer1 = new Printer<>(23);
		Printer<Double> printer2 = new Printer<>(33.5);
		
		printer1.print(); // Integer
		printer2.print(); // Double
	}
}
```

Passiamo il tipo che vogliamo racchiuso tra `< >`. Si possono solo passare oggetti o [[008 Boxing, unboxing e autoboxing|Tipi Wrapper]].

- [[Esempio più complicato Generics Java|⇉ Esempio più complicato Generics Java]].

#### CLASSI GENERICHE CON PIÙ PARAMETRI
Non siamo limitati all'uso di un solo parametro generico, infatti possiamo definire **classi generiche con più parametri**:

```java
public class GenericClass<T, V /*, ...*/> {
	//...
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
- [Guida da html.it](https://www.html.it/pag/18028/il-tipo-generics-in-java/)