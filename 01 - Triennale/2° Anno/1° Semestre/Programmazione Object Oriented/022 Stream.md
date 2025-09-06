---
aliases: 
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Caratteristiche della Programmazione ad Oggetti
cssclasses:
  - 
---
> [!success] [Video Consigliato](https://www.youtube.com/watch?v=t1-YZ6bF-g0)

Lo **Stream** in Java rappresenta una sequenza di elementi sui cui eseguire *certe operazioni* che fanno pesante uso delle [[021 Espressioni Lambda|Espressioni Lambda]]. 

![[Pasted image 20221031093929.png|800]]

Uno **Stream Pipeline** è costituito da:
- Una *fonte* (Source): può essere un [[Array (Java)|array]], una [[Lista (Java)|lista]], un file, ecc. (in generale una collezione di oggetti);

- Una *sequenza di operazioni*, che possono essere intermedie o terminali:
	- <font color="#9cb254">Operazioni Intermedie</font>: Come `filter`, `map` o `sort`, *ritornano uno stream* in modo da concatenare diverse operazioni intermedie. 
		- Sono ammesse da 0 a N operazioni intermedie. [[Operazioni Intermedie|Operazioni Intermedie principali ↩]].
	- <font color="#c75c45">Operazioni Terminali</font>: Come `forEach`, `collecct` o `reduce`, *ritornano void oppure un risultato non-stream*.
		- È ammessa solo una operazione terminale. [[Operazioni Terminali|Operazioni Terminali principali ↩]].

Template di uno stream:
```Java
import java.util.stream.*;

public class Main{
	public static void main(String[] args){
		TipoStream
			.operazioneIntermedia1()
			.operazioneIntermedia2()
			.operazioneIntermediaN()
			.operazioneTerminale();
	}
}
```


> [!example]- <font color="orange">Esempio 1: Stream di interi</font>
> Utilizziamo l'operazione intermedia `.range(int min, int max)` per creare un flusso di interi che parte dall'intero `min` fino a `max-1`.
> Utilizziamo inoltre l'operazione `.forEach()` per stampare ogni elemento dello stream. Questo può avvenire in due modi:
> - `.forEach(System.out::print)`;
> - `.forEach(x -> System.out.print(x))`.
>```Java
>import java.util.stream.*;
>
>public class Main {
>	public static void main(String[] args) {
>		IntStream
>			.range(1, 10)
>			.forEach(System.out::print); // Oppure .forEach(x -> System.out.print(x));
>			// Si può usare anche println per far andare a capo dopo ogni elemento del flusso
>	}
>}
>```
>Output: 
>```markup
>123456789
>```

> [!example]- <font color="orange">Esempio 2: Somma di uno Stream di interi</font>
> Utilizziamo l'operazione terminale `.sum()` per ritornare la somma di tutti gli elementi di un flusso di interi.
> 
>```Java
>import java.util.stream.*;
>
>public class Main {
>	public static void main(String[] args) {
>		int i = IntStream
>				.range(1, 5)
>				.sum();
>		// Mette all'interno di i la somma
>		System.out.println(i);
>	}
>}
>```
>Output: 
>```markup
>10
>```

> [!example]- <font color="orange">Esempio 3: Riordinamento di uno Stream derivante da un Array di stringhe</font>
> Utilizziamo la parola `Stream.of()` per inizializzare uno stream secondo quello che passiamo come argomento.
> 
>```Java
>import java.util.stream.*;
>
>public class Main {
>	public static void main(String[] args) {
>		String[] nomi = {"Ava", "Aneri", "Alberto"};
>
>		Stream.of(nomi)
>			.sorted()
>			.forEach(System.out::println);
>	}
>}
>```
>Output: 
>```markup
>Alberto
>Aneri
>Ava
>```

> [!example]- <font color="orange">Esempio 4: Filtra uno Stream di stringhe</font>
> Utilizziamo l'operazione intermedia `.filter()` passandogli come argomento una espressione lambda che specifica se la stringa corrente comincia con un certo valore.
> 
>```Java
>import java.util.stream.*;
>
>public class Main {
>	public static void main(String[] args) {
>		String[] nomi = {"A-1", "B-1", "A-2", "B-2", "C-1", "A-3"};
>
>		Stream.of(nomi)
>				.filter((s) -> s.startsWith("A"))
>				.forEach(System.out::println);
>	}
>}
>```
>Output: 
>```markup
>A-1
>A-2
>A-3
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